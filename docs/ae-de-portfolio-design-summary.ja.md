# AE/DEポートフォリオ構成意図

> English ver.: [ae-de-portfolio-design-summary.md](ae-de-portfolio-design-summary.md)

## なぜこのポートフォリオなのか：着想から設計思想まで

## 1. 目的

このポートフォリオ群は、Analytics Engineering(AE:分析エンジニアリング) / Data Engineering(DE:データエンジニアリング) 領域に対して、実務未経験者として何を示せるかを考えたうえで設計しました。

単発の技術練習ではなく、業務アプリで発生するデータ、利用イベントの取り込み、raw data、warehouse modeling、data quality checks、Business Intelligence(BI:ビジネスインテリジェンス) dashboard artifacts までを、小規模な一貫した業務ドメイン上で再現することを目的にしています。

題材には、企業におけるAIツール利用ガバナンスを設定しました。AIツールの利用申請、承認、利用状況、コスト、ガバナンス例外を扱うことで、業務データがどのように発生し、どのように分析可能な形へ変換され、最終的に意思決定材料として提示されるかを示す構成にしています。

この取り組みは、実務経験そのものを代替するものではありません。一方で、実務で求められる責務分離、データモデリング、検証観点、ドキュメント化、レビューしやすい成果物づくりを、ポートフォリオとして確認できる形にすることを重視しました。

---

## 2. 研究経験との接続

大学院では、数値計算を用いて現実の物理過程をモデル化し、計算機上で模擬する研究に取り組みました。数値計算では、支配方程式、初期条件、境界条件、計算条件を明示し、結果だけでなく、結果に至る過程を再現・比較・検証できる状態に保つことが重要でした。

この経験は、今回のAE/DEポートフォリオ群の設計にもつながっています。実務経験や実際の業務データがない中で、到達可能な情報をもとに業務フローを仮定し、AIツール利用ガバナンスという仮想ドメイン上で、業務アプリ、イベント取り込み、synthetic raw data、dbt warehouse、BI dashboard artifacts までを小規模に模擬しました。

研究で物理過程をモデル化したように、このポートフォリオでは業務データの発生・取り込み・変換・検証・可視化の流れをモデル化しています。source contracts、model grain、data tests、reconciliation checks、business review signals、BI presentation の責務を明示することで、成果物だけでなく、設計判断と検証過程をレビューできる構成を重視しました。

この取り組みは、実務経験そのものの代替ではありません。一方で、実務未経験という制約の中で、AE/DE領域に必要なデータの流れ、責務分離、検証可能性、意思決定への接続を理解しようとした過程を示すものです。

---

## 3. 着想と設計方針

出発点は、「AE / DE求人に対して、実務未経験者としてどう答えるか」でした。

まず、AE実務で扱われる業務を調査し、dbtによるwarehouse modelingやdata testsだけでなく、その外側にあるstakeholder-facingなBIレポートまで含めて示す必要があると考えました。

一方で、個人開発では実務上の業務データを持っていません。そのため、自作のsynthetic dataを作成し、それをdbtや分析APIで扱う方針にしました。ただし、単にランダムなデータを作るのではなく、業務アプリで発生しうる申請・承認・利用・コスト・例外のデータとして設計し、下流の分析基盤で意味のあるモデルや指標に変換できることを重視しました。

この方針に基づき、最終的に次の4つのポートフォリオを、同じ問題意識のもとで段階的に構築しました。

- `analytics-metrics-api`
- `ai-tool-access-requests`
- `go-ingestion-api`
- `access-governance-warehouse`

---

## 4. 実装順序と試行錯誤

このポートフォリオ群は、最初から完成形を決めて作ったものではなく、AE/DE領域への理解を深めながら段階的に拡張しました。

### 1. `analytics-metrics-api`

最初に、AE領域に入る前段として、蓄積型データ、Key Performance Indicator(KPI:重要業績評価指標) 定義、read-only API(application programming interface:アプリケーションプログラミングインターフェース)、Parquet + DuckDBによるローカル集計を扱うプロジェクトを実装しました。

synthetic SaaS-like event data と job-run data を用い、APIから分析用データにアクセスする構成を作ることで、バックエンドAPIと分析データストアの接点を理解することを目的にしました。KPI定義、resource-oriented HTTP design、golden-output testing、offline-first testingを通じて、データを安定した契約として提供する考え方を確認しました。

### 2. `ai-tool-access-requests`

次に、`analytics-metrics-api` の経験を踏まえ、AE領域で扱える仮想的な業務データの発生源として、AIツール利用申請・承認を扱うDjango + PostgreSQLアプリを実装しました。

このプロジェクトでは、requester / reviewer / admin の責務分離、Authentication and Authorization、Role-Based Access Control(RBAC:ロールベースアクセス制御)、フォームバリデーション、業務ルールを担保する自動テストを扱いました。後続のwarehouseで扱う申請・承認データの上流にある業務アプリ側を表現する位置づけです。

### 3. `access-governance-warehouse` v0.1.0

その後、`ai-tool-access-requests` のような業務アプリから発生しうるデータを想定し、synthetic raw dataとして設計したうえで、dbt + DuckDB + PythonによるAE中核部分を実装しました。

raw sources / staging / core / intermediate / marts のdbt layerに分け、アクセス申請、承認状況、利用実績、コスト、ガバナンス例外を分析できるwarehouseとして設計しました。ここでは、source contracts、model grain、dbt data tests、mart設計、static governance reportを重視しました。

### 4. `access-governance-warehouse` v0.2.0 / v0.2.1

v0.2.0では、ローカルDuckDB pathをprimary review pathとして維持しつつ、同じdbt source contract、model tree、marts、data testsをBigQuery上でも実行できるcloud warehouse execution pathを追加しました。

v0.2.1では、BigQuery martsに接続したLooker Studio dashboard artifactsを追加しました。AE領域とstakeholder-facing reportingの接点を示すため、Executive Overview、Tool Adoption and Usage、Governance Exceptions and Review Signalsの3ページをBI artifactsとして可視化しました。

このとき、business logicやreview classificationsはdbt marts側に保持し、Looker Studioはpresentation / filtering / chartingに集中させる方針を取りました。

### 5. `go-ingestion-api`

最後に、ポートフォリオ群全体を補完する形で、AE領域の上流からデータが流入する境界として、AIツール利用イベントを受け取るGo HTTP ingestion APIを実装しました。

1 requestにつき1 eventを受け取り、Content-Type enforcement、request body size limit、strict JSON decoding、unknown field rejection、event model validation、user / tool reference validationを行ったうえで、受理したeventをappend-only JSONL raw storageへ保存します。

このプロジェクトでは、分析や変換処理そのものではなく、warehouseに入る前段のingestion boundaryを明示的に扱いました。これにより、AE領域の前段にあるDE寄りの責務、つまり入力契約、validation、raw storage、downstreamとの責務分離を示すことを意図しました。

---

## 5. プロジェクト全体像

このポートフォリオ群は、以下のようなデータフローを小さく再現することを意図しています。

```text
業務アプリ / 利用イベント
  -> ingestion boundary
  -> raw data
  -> warehouse modeling
  -> marts
  -> static report / BI dashboard artifacts
  -> stakeholder decision-making
```

各リポジトリの役割は次の通りです。

| Repository | Role | Main focus |
|---|---|---|
| `analytics-metrics-api` | 分析データ参照API層 | KPI定義、read-only API、Parquet + DuckDBによるローカル集計、golden-output testing |
| `ai-tool-access-requests` | 業務アプリ層 | 申請・承認、権限分離、RBAC、業務ルール、Django + PostgreSQL |
| `go-ingestion-api` | 利用イベント取り込み層 | HTTP request contract、strict validation、append-only JSONL raw storage |
| `access-governance-warehouse` | 分析基盤 / BI層 | dbt modeling、data tests、BigQuery、Looker Studio、marts、BI artifacts |

### ポートフォリオ群の関係概念図

以下は、実際に接続されたproduction pipelineではなく、各リポジトリの役割を示すための模擬的な業務データフロー概念図です。  
エンタープライズ向けAIツール利用ガバナンスを題材に、業務アプリ、利用イベント取り込み、分析基盤、BI artifacts をそれぞれレビューしやすい小さなレイヤーとして表現しています。

```text
各リポジトリの関係を示す仮想業務フロー概念図

+--------------------------+        +--------------------------+
| ai-tool-access-requests  |        | go-ingestion-api         |
| Django business app      |        | Go ingestion boundary    |
| request / approval flow  |        | usage event intake       |
+------------+-------------+        +------------+-------------+
             |                                   |
             | 想定される業務データ                 | 概念上の利用イベントフロー
             +-------------------+---------------+
                                 |
                                 v
+-------------------------------------------------------------+
| access-governance-warehouse                                 |
| Analytics engineering warehouse                             |
| synthetic raw data -> dbt -> marts -> DuckDB / BigQuery     |
| data tests / governance report / Looker Studio artifacts    |
+-----------------------------+-------------------------------+
                              |
                              v
+--------------------------------------------------------------+
| static governance report / Looker Studio dashboard artifacts |
| stakeholder-facing analytical outputs                        |
+--------------------------------------------------------------+

+--------------------------+
| analytics-metrics-api    |
| FastAPI read-only API    |
| KPI design / analytics   |
| backend + data interface |
+--------------------------+
  KPI / API / 分析データアクセス設計を確認するための先行プロジェクト
```

`analytics-metrics-api` は、この仮想業務フローに直接接続された実装というより、KPI定義、read-only API、分析データアクセスの基礎を確認するための先行プロジェクトとして位置づけています。

---

## 6. 設計判断と実装上の証拠

このポートフォリオ群では、実装量そのものよりも、どのような設計判断を行い、それをどの成果物で確認できるかを重視しました。

| 設計判断 | 実装での表現 |
|---|---|
| 結果だけでなく変換過程を検証する | dbt data tests、source contracts、model grain、reconciliation checks |
| Data QualityとBusiness Logicを分離する | transformation failuresはdbt testsで検出し、business review signalsはmarts / BIに出力 |
| BI側にロジックを寄せすぎない | review classificationsやbusiness logicをdbt marts側に保持 |
| 上流データの発生源を意識する | Django業務アプリとGo ingestion APIを別レイヤーとして実装 |
| raw dataと下流変換を分ける | append-only JSONL、Parquet raw sources、staging / core / marts構成 |
| 小さな実装でも入力契約を明示する | strict JSON decoding、unknown field rejection、Content-Type enforcement |
| レビュー可能性を高める | README、CI、release、evidence artifacts、dashboard documentationを整備 |
| 実行環境の再現性を高める | Docker multi-stage build、committed fixtures、validation commands、CI |

---

## 7. 実装過程で重視したこと

### 再現性と検証可能性

各プロジェクトでは、実行手順、入力、出力、テスト、Continuous Integration(CI:継続的インテグレーション) を明示し、第三者がレビューしやすい状態を重視しました。

特にデータ系では、deterministic synthetic data、committed golden outputs、source contracts、model grain、reconciliation checks、dbt data testsを扱い、処理結果だけでなく変換過程も検証できるようにしています。

### Data Quality と Business Logic の分離

`access-governance-warehouse` では、データ変換処理の不整合と、業務上レビューすべきシグナルを分けて扱いました。

たとえば、重複行、参照不整合、grain違反、invalid enum、negative metrics、集計不一致のような構造的問題は、warehouseの信頼性を損なうtransformation failureとしてdbt testsで検出します。

一方で、usage without approval、approved but inactive、spend without usageのような状態は、必ずしもデータ破損ではありません。これらは、業務上レビューすべきbusiness review signalsとしてmartsやBI dashboardに出力します。

これにより、data quality、business logic、stakeholder-facing presentationの責務を分離する設計を意識しました。

### 小規模でも責務を明示すること

各リポジトリは意図的に小さく作っています。ただし、小さい実装であっても、責務の境界は曖昧にしないようにしました。

業務アプリは申請・承認・権限分離を扱い、ingestion APIはイベント受け取りとraw storageへの保存を扱い、warehouseはデータ変換・集計・mart定義を扱い、BI dashboard artifactsはstakeholder-facing presentationを扱います。

このように、1つの巨大なアプリを作るのではなく、複数の小さなリポジトリに分けることで、それぞれが実務上どのレイヤーに対応するのかを示しやすくしました。

---

## 8. このポートフォリオ群で示せること

このポートフォリオ群を通じて、以下を示すことを意図しています。

- 業務データが発生してから分析・可視化されるまでの全体像を意識できること
- AE領域におけるdbt modeling、model grain、source contracts、data tests、marts設計を扱えること
- DE領域に近いingestion boundary、入力契約、validation、raw storageへの理解があること
- BI dashboardを単なる可視化ではなく、dbt martsで定義されたbusiness logicを提示するpresentation layerとして扱えること
- Data quality errorとbusiness review signalを区別して設計できること
- Python、SQL、dbt、DuckDB、BigQuery、Django、FastAPI、Go、Docker、CIを、目的に応じて使い分けていること
- 実装だけでなく、README、開発サマリ、実行証跡、静的レポート、dashboard documentationまで整備していること
- 実務未経験という制約の中でも、調査、仮説設定、実装、検証、説明可能な成果物づくりを自走して行えること

---

## 9. ポートフォリオの限界と、実務で想定される運用領域

このポートフォリオ群は、production systemそのものではありません。synthetic dataを用いた小規模なreviewable projectであり、実際の企業データ、ユーザー数、セキュリティ要件、運用監視、権限管理、コスト制御、データ契約管理、障害対応等までは扱っていません。

一方で、実務で重要になる責務分離、検証可能性、データ品質、business logic、BI presentationの分離を、小さな構成で確認できるように設計しています。

個人ポートフォリオでは扱いきれていないものの、実際の企業環境では以下のような運用領域が必要になると認識しています。

| 実務で想定される領域 | 内容 |
|---|---|
| Data contract / schema governance | 上流システムと下流分析基盤の間で、schema、更新頻度、欠損・遅延時の扱い、breaking changeの扱いを合意・管理する |
| Access control / security | 実データを扱う場合の権限設計、認証・認可、least privilege、個人情報や機密情報の取り扱いを設計する |
| Production ingestion | JSONL保存だけでなく、message queue、object storage、streaming ingestion、batch loadなどの本番向け取り込み方式を検討する |
| Workflow orchestration | データ生成・取り込み・変換・テスト・レポート生成を定期実行し、依存関係や失敗時の再実行を管理する |
| Data freshness monitoring | データが期待時刻までに到着しているか、特定テーブルやmartが古くなっていないかを監視する |
| Data quality monitoring | dbt testsだけでなく、異常値、急激な件数変動、分布変化、NULL増加などを継続的に監視する |
| CI/CD for data pipelines | コード変更時にSQL、dbt models、tests、docs、sample buildを検証し、安全に本番反映する仕組みを整える |
| Audit logging | 誰が、いつ、どのデータやdashboardにアクセスしたか、どの処理が実行されたかを追跡できる状態にする |
| Incident response | pipeline failure、data delay、誤集計、dashboard不整合などが起きた際の検知、切り分け、復旧、関係者への通知を行う |
| Cost management | BigQueryなどのcloud data warehouseにおけるクエリコスト、保存コスト、実行頻度、partition / clustering設計を管理する |
| Stakeholder operation | dashboardや指標の定義変更、利用部門との合意形成、定例レビュー、問い合わせ対応を運用に組み込む |
| Applied analytics / ML connection | martsや指標定義を、将来的な分析、実験設計、特徴量設計、Machine Learning(ML:機械学習) pipelineに接続する |

これらは現時点のポートフォリオで完全に実装した領域ではなく、今後実務経験を通じて補うべき領域として認識しています。今回のポートフォリオ群では、その前段として、データの発生、取り込み、変換、検証、可視化までの責務分離と基本的な設計観点を示すことを重視しました。

---

## 10. まとめ

このポートフォリオ群は、実務未経験者としてAE / DE求人に対して何を示せるか、という問いから出発しました。

数値計算研究で培った「現実をモデル化し、検証可能な仮想実験として扱う」という考え方を、今回は業務データの流れに適用しました。実務データがないという制約に対して、AIツール利用ガバナンスという仮想業務ドメインを設定し、業務アプリ、利用イベント取り込み、synthetic raw data、dbt warehouse modeling、BigQuery execution path、Looker Studio dashboard artifactsまでを段階的に構築しました。

この取り組みを通じて、個別技術の習得だけでなく、業務データを意思決定に使える形へ変換する流れを、小規模ながら一貫して設計・実装・検証したことを示す資料としてまとめています。
