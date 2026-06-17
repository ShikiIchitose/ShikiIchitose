# アナリティクスエンジニアリングにおけるモデリングと運用方針について

> [English version](analytics_engineering_modeling_operating_approach_overview.md)
>
> この資料は、アナリティクスエンジニアリングに関するモデリング資料を短くまとめた概要版です。  
> 資料全体を確認したい方向けに、詳細版も用意しています。  
> [Analytics Engineering Modeling Perspectives](00_analytics_engineering_modeling_index.ja.md)

## 1. 位置づけと基本方針

この資料では、dbtモデリング、model grain、テスト、ドキュメンテーション、BI / semantic layer での利用、ステークホルダーとの認識合わせ、意思決定支援のための出力を中心に、アナリティクスエンジニアリング業務への取り組み方を整理します。

内容は、ポートフォリオ実装での経験と公開情報に基づいています。特定企業の内部データ、本番システム、既存の dbt project へアクセスしていることを前提にはしていません。また、特定企業向けの完全な本番アーキテクチャを提案するものでもありません。目的は、事業上の問いを、分析モデルと意思決定支援のための出力へどのように接続できるかを、構造化して示すことです。

ポートフォリオプロジェクトは、スコープを絞った実装であり、本番システムではありません。ただし、明示的な model grain、dbtテスト、ドキュメンテーション、lineage、再現性、事業側が利用できる分析出力など、本番運用を意識したデータエンジニアリングの実践を前提に構築しています。

中心となる流れは次の通りです。

```text
business question
  -> business process
  -> source data
  -> model grain
  -> facts and dimensions
  -> tests and documentation
  -> BI / semantic layer or reporting marts
  -> decision-support output
```

アナリティクスエンジニアリングは、単にテーブルを変換する作業ではありません。データを信頼しやすく、説明しやすく、再利用しやすく、行動につなげやすい形に整えるための実践です。

---

## 2. モデリング原則

この資料で用いる dbtモデリングの考え方は、次の原則に基づいています。

| 原則 | 適用方法 |
|---|---|
| 事業上の問いから始める | テーブル名から考え始める前に、そのモデルがどの判断、指標、ダッシュボード、レビュー workflow を支えるべきかを明確にする。 |
| 業務プロセスを特定する | 注文、サブスクリプション、プロダクト利用、承認、請求、在庫スナップショット、価格変更、サポート対応など、測定可能なイベント、トランザクション、状態をモデル化する。 |
| model grainを明示する | 1行が何を表すかを明確にし、指標の二重計上や安全でない粒度での集計を防ぐ。 |
| ソースデータの整形と業務上の意味づけを分離する | staging models は raw source grain に近い状態を保ち、業務ルール、結合、re-graining、指標ロジックは、意図をドキュメント化・テストしやすい後段のレイヤーで導入する。 |
| 再利用可能なファクトとディメンションをモデル化する | 測定可能な業務プロセスにはファクトモデルを使い、比較的安定したエンティティや説明用の文脈にはディメンションモデルを使う。 |
| stock metrics と flow metrics を区別する | 注文、セッション、収益、支出などの期間中の活動量と、在庫残高、未処理リクエスト、アクティブなサブスクリプション、月末時点の承認済みユーザー数などの特定時点の状態を区別する。 |
| 変換処理の失敗とレビューシグナルを分離する | 構造上・意味上の契約違反は dbtテストで失敗させる。一方で、データとしては妥当だが望ましくない業務状態は、marts やダッシュボード上のレビューシグナルとして表示する。 |
| 前提をテストし、ドキュメント化する | dbtテストとモデルドキュメントを、後付けではなく、モデル契約の一部として扱う。 |

Dimensional modeling、特に Kimball 型のファクトとディメンションの分離は、分析用データモデリングにおける最も確立された基礎の一つです。現代のデータチームでは、Data Vault、Third Normal Form（第3正規形）、One Big Table、wide denormalized marts、semantic layer を中心にした設計など、別の手法も利用・議論されています。それでも、測定可能な業務プロセスをファクトとして表し、それを説明する業務上の文脈をディメンションとして整理する考え方は、再利用可能で BI に公開しやすい分析モデルを作るうえで、現在でも有用です。したがって、本資料では、ファクトとディメンションの区別をモデリング上の基礎原理として採用します。

典型的なレイヤー分離は次のようになります。

```text
sources
  -> staging
  -> intermediate
  -> marts / dimensional models
  -> BI or semantic layer
```

正確なフォルダ名はプロジェクトによって異なります。重要なのは、各レイヤーに明確な責任があること、公開される各モデルにドキュメント化された grain があること、下流の利用者がそのモデルの意味と解釈方法を理解できることです。

特に重要なのは、変換処理の失敗と業務レビューシグナルの違いです。

> Business exceptions are outputs. Transformation inconsistencies are failures.

つまり、業務上の例外は出力であり、変換処理の不整合は失敗です。

必須キーの欠損、主キーの重複、許可値にない値、解決できないリレーションシップは、テストを失敗させるべきです。一方で、利用率が低い、在庫が滞留している、承認なしの利用がある、利用なしの支出がある、利益率がしきい値を下回っている、といった妥当な行は、業務レビューシグナルとして扱う方が適切です。これらは、デフォルトで壊れた pipeline として扱うのではなく、レビュー対象として表示するべきです。

---

## 3. 本番運用を意識した運用方針

本番運用を意識した環境では、一般に Looker、Tableau、dbt Semantic Layer、または同等の BI / semantic layer を、再利用可能な dimensional models の主な利用層として扱います。

典型的なパターンは次の通りです。

```text
sources
  ↓
staging
  ↓
intermediate
  ↓
marts / dimensional models
  - dim_*
  - fct_*
  ↓
BI / semantic layer
  - relationships / data models
  - governed measures
  - semantic metrics
  - explores or self-service analysis paths
  ↓
dashboards / self-service BI / stakeholder reporting
```

この構造では、dbt がテストされ、ドキュメント化されたファクトとディメンションを提供します。その上で、BI / semantic layer が、利用者向けのリレーションシップ、measure、ダッシュボード、explore、セルフサービス分析体験を定義します。このパターンにより、ダッシュボード固有の結合や計算をすべて ad hoc SQL に押し込むことを避けつつ、事業ユーザーが管理されたデータを探索できるようにします。

同時に、dbtレポーティングマートは、ロジックを materialize し、レビュー可能・テスト可能・再利用可能・最適化可能にする必要がある場面で価値を持ちます。例えば、重い、または繰り返し実行される集計、複雑な stock-and-flow metrics、ダッシュボード性能の最適化、ダッシュボード向けに整えた抽出データ、業務レビュー候補テーブル、急ぎの軽量レポーティング、BI・SQL・notebook・script をまたいだ再利用などが該当します。

例えば、`fct_sales_order` と `dim_product` を使った利用者向けの探索を定義するには、BIツールが適している場合があります。一方で、`inventory_health_daily`、`sales_velocity_monthly`、`operations_review_candidates` のような継続的な業務向け出力は、複雑なロジック、安定した grain、繰り返し利用、明確なテストカバレッジが必要であれば、dbt側で直接モデル化する方が適している場合があります。

### 3.1 アナリティクスエンジニアリングの責任範囲とステークホルダー側のオーナーシップ

アナリティクスエンジニアリングは、信頼できる意思決定支援モデルを提供できます。ただし、価格設定、サポート、カスタマーサクセス、リスク、ファイナンス、プロダクト、業務上の判断に関する事業側のオーナーシップを置き換えるべきではありません。

アナリティクスエンジニアリングが担う、または強く関与するべき領域は次の通りです。

- ステークホルダーの問いを、モデル化可能な業務概念に翻訳すること
- 指標定義と model grain を明確にすること
- 必要なソースデータと source contracts（ソース契約）を特定すること
- 再利用可能なファクト、ディメンション、intermediate models、marts を設計すること
- 構造上・意味上の前提に対する dbtテストを実装すること
- 前提、制約、解釈上の注意をドキュメント化すること
- BIツール、semantic layer、ダッシュボード、レポート、レビュー workflow に安定したモデルを公開すること
- 変換ロジックの再現性、lineage、追跡可能性を維持すること

一方で、価格変更、キャンペーン対象条件、サポートポリシー変更、カスタマーサクセスの優先順位付け、リスク対応、ファイナンスレビュー、プロダクト戦略のような判断は、事業側のステークホルダーが担うべきです。

アナリティクスエンジニアリングの役割は、業務状態を測定可能で、説明可能で、再利用可能で、議論しやすい形にすることです。意思決定を支援するものであり、静かに置き換えるものではありません。

そのため、指標定義、model grain、レビューシグナルの意味、想定される利用方法について、ステークホルダーと積極的にコミュニケーションを取り、認識を合わせることが重要です。

---

## 4. ケーススタディ: 顧客ライフサイクルと収益化分析

顧客ライフサイクルと収益化分析は、多くの事業モデルに現れるため、再利用可能なケーススタディとして有用です。業界によって用語は異なりますが、多くの事業では、顧客、アカウント、ユーザー、購入者、出品者、組織が、獲得、有効化、利用、収益化、継続、フォローアップへどのように進むのかを理解する必要があります。

この領域における中心的なステークホルダーの問いは、例えば次のように表せます。

> どの顧客セグメント、アカウント、プロダクト、プラン、チャネルが、有効化、利用、収益、継続を生み出しているのか。また、どのライフサイクル状態にフォローアップが必要なのか。

最初のモデリングステップは、関連する業務プロセスを特定することです。この領域では、signup / registration、onboarding / activation、product usage、transactions or orders、subscription lifecycle、billing or invoicing、support interactions、lifecycle review signals などが候補になります。

ソースデータは企業によって異なりますが、顧客またはアカウントのマスターデータ、ユーザーまたはメンバーのデータ、獲得データ、プロダクトまたはサービスカタログ、signup や onboarding event、利用や行動イベント、注文または取引、サブスクリプションまたは契約データ、請求または収益データ、サポートまたはオペレーションデータなどが候補になります。

再利用可能なモデル構造には、例えば `dim_customer`、`dim_account`、`dim_user`、`dim_product`、`dim_plan`、`dim_channel`、`dim_date` のようなディメンション、`fct_signup`、`fct_activation_event`、`fct_usage_daily`、`fct_transaction`、`fct_order`、`fct_subscription_snapshot_monthly`、`fct_invoice`、`fct_payment`、`fct_support_case` のようなファクト、さらに月次利用集計、顧客収益集計、月末時点のアカウント状態、直近アクティビティ分類、ライフサイクルレビューシグナル準備のための intermediate models が含まれます。

正確なモデル名そのものよりも重要なのは、モデリング原則です。公開される各ファクト、ディメンション、mart には、明確な責任、ドキュメント化された grain、テスト可能な前提が必要です。

例えば、`fct_usage_daily` は、顧客、アカウント、またはユーザー、プロダクト、日付ごとに1行を公開する可能性があります。月次ライフサイクル mart は、レポート月、セグメント、ライフサイクル状態ごとに1行を公開する可能性があります。現在のアカウントヘルスを表す出力は、現在のアカウントごとに1行を公開する可能性があります。これらのモデルは異なる問いに答えるものであり、grain を理解せずに安易に混在させるべきではありません。

この領域では、flow metrics と stock metrics も組み合わされます。flow metrics には、ある期間中の signup、activation、usage、revenue、support cases などが含まれます。stock metrics には、月末時点の active accounts、active subscriptions、unresolved support cases、dormant accounts、at-risk accounts などが含まれます。これらの指標を1つの mart で組み合わせる場合は、どの指標が期間中の flow で、どの指標が特定時点の stock なのかをドキュメントで説明する必要があります。

この領域には、データとして妥当な業務レビューシグナルも含まれます。例えば、signup はあるが activation がない、activated account だが recent usage がない、usage は高いが monetization が弱い、paying account だが recent usage がない、active usage はあるが想定される billing がない、renewal 前に usage が低下している、過去に高い価値があった dormant customer などです。これらのシグナルは、意思決定支援のための出力としてモデル化するべきです。必ずしもデータ品質上の失敗を意味するものではなく、ステークホルダーとの認識合わせなしに自動的な業務アクションを起動するべきでもありません。

この領域における本番運用を意識した公開パターンでは、テスト済みのファクトとディメンションを dbt側に置き、その上で BI / semantic layer から管理されたリレーションシップと measure を公開できます。管理された measure の例には、activated customers、active accounts、monthly active users、revenue amount、churned accounts、retention rate、average revenue per account などがあります。特に revenue 関連の measure では、billed revenue、collected payment amount、recognized revenue、transaction fees、gross revenue、net revenue、gross margin、またはその他の事業固有の収益概念のどれを表すのかを明確にする必要があります。

安定して繰り返し使われるロジックやレビュー向けロジックについては、レポーティングマートも有用です。例えば、`customer_lifecycle_monthly`、`activation_funnel_weekly`、`usage_engagement_monthly`、`revenue_retention_monthly`、`account_health_current`、`churn_risk_candidates`、`monetization_review_candidates`、`operational_review_candidates` などが考えられます。これらの marts の目的は、BI / semantic layer を置き換えることではありません。明確な価値がある場合に、安定し、再利用可能で、テスト可能で、追跡可能な業務ロジックを materialize することです。

---

## 5. ポートフォリオによる実装証拠

この方針の主な実装証拠は、エンタープライズ向け AI ツールアクセスガバナンスを題材にした、dbt + DuckDB + BigQuery のアナリティクスエンジニアリングポートフォリオプロジェクトである `access-governance-warehouse` です。

このプロジェクトでは、決定論的な synthetic raw data、dbt source contracts とテスト、明示的な model grain、staging / core / intermediate / marts のレイヤー、再利用可能なファクトとディメンション、intermediate models に分離した re-graining と stock logic、レポーティング用途の marts、変換処理の失敗と業務レビューシグナルの分離、dbtドキュメンテーションと lineage、Looker Studio dashboard artifacts、static report outputs、再現可能なローカル実行パス、BigQuery を使った任意の cloud execution path を示しています。

実装済みポートフォリオの構造は次の通りです。

```text
raw sources
  ↓
staging
  ↓
core
  ↓
intermediate
  ↓
marts
  ↓
Looker Studio / static report artifacts
```

この構成は、完全な本番ETL取り込み基盤というよりも、ELT寄りの analytics engineering workflow に近いものです。raw data が分析環境にロードされた後の、データウェアハウス側での dbt modeling、testing、documentation、lineage、BI で利用しやすい出力の作成に焦点を当てています。

本番規模でのロード前変換、streaming ingestion、Dataflow / Apache Beam 型の ETL 処理は、現在のポートフォリオの範囲外です。ただし、それらは data engineering における重要な隣接領域であり、必要に応じて学び、貢献していく対象として認識しています。

これは、再利用可能な `dim_*` や `fct_*` モデルを `marts` 配下に直接配置し、Looker、Tableau、または semantic layer から利用するような、本番運用を意識した構造とは異なる場合があります。このポートフォリオでは、再利用可能なファクトとディメンションのために `core` を使い、`marts` はレポーティング用途で、ステークホルダー向けの出力に限定しています。この構成は、Looker Studio を軽量な可視化レイヤーとして使い、再利用可能な業務ロジックを BI層のカスタムロジックではなく dbt側に置くというポートフォリオのスコープに合っています。

応用可能な点は、正確なフォルダ構造ではありません。応用可能なのは、次のようなモデリング原則です。明示的な grain を定義すること、source contracts を保つこと、ソースデータの整形と業務ロジックを分離すること、再利用可能なファクトとディメンションを作ること、re-graining ロジックを分離すること、事業側に向けた分析出力を公開すること、構造上の前提をテストすること、制約と解釈ルールをドキュメント化することです。

## 6. 入社後30 / 60 / 90日の初期貢献方針

アナリティクスエンジニアリングまたはデータエンジニアリングのチームに参加した最初の30日間は、既存の業務プロセス、ソースシステム、dbt project structure、model grain、指標定義、BIの利用状況、チーム固有のツールや workflow、現在の課題を理解することに注力します。

60日頃までには、ドキュメンテーション更新、dbtテスト追加、モデルレビュー、lineage確認、既存のレポーティングマートやダッシュボードの支援など、小さく範囲を絞った改善から貢献機会を探します。

90日頃までには、ポートフォリオで実践した原則、特に明示的な grain、source contracts、テストされた前提、ドキュメント化された制約、変換処理の失敗と業務レビューシグナルの明確な分離を活用し、再利用可能で意思決定に使いやすい分析モデルを支援することを目指します。

目的は、既存のデータ基盤をすぐに再設計することではありません。チームのアナリティクスエンジニアリング業務に慣れ、現在の環境を理解し、適切な問いを立て、既存 workflow に安全に貢献しながら、より信頼性が高く再利用可能なアナリティクスエンジニアリングの実践を徐々に支援することです。

---

## 参考文献と公開情報

この資料は、ポートフォリオ実装での経験と公開情報に基づいています。以下の情報源は、dbtモデリング原則、dimensional modeling、アナリティクスエンジニアリングの実践、ドキュメンテーション、semantic layer に関する考え方、データチームのワークフロー、ETL / ELT ワークフロー境界, データパイプライン設計に関する公開事例の参考として使用しました。

- dbt Labs. “What is dbt?” dbt Developer Hub, version 1.11.  
  https://docs.getdbt.com/docs/introduction?version=1.11

- dbt Labs. “Best Practice Guides.” dbt Developer Hub, version 1.11.  
  https://docs.getdbt.com/best-practices?version=1.11

- dbt Labs. “Building a Kimball dimensional model with dbt.” dbt Developer Blog, April 20, 2023.  
  https://docs.getdbt.com/blog/kimball-dimensional-model

- Kimball Group. “Dimensional Modeling Techniques.” Kimball Group.  
  https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/

- GitLab. “Data Team - How We Work” and “dbt Change Workflow.” The GitLab Handbook.  
  https://handbook.gitlab.com/handbook/enterprise-data/how-we-work/  
  https://handbook.gitlab.com/handbook/enterprise-data/how-we-work/dbt-change-workflow/

- dbt Labs. “ETL vs ELT: What's the difference?” dbt Blog.  
  https://www.getdbt.com/blog/etl-vs-elt

- dbt Labs. “Data pipelines: Critical components and best practices.” dbt Blog.  
  https://www.getdbt.com/blog/data-pipelines
