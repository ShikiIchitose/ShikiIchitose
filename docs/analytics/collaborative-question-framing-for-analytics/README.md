# Collaborative Question Framing for Analytics

このディレクトリは、データ利活用に関する着想や、問い、指標、必要なデータ、利用場面、検証方法などに不確実性が残る場合に、必要な専門知を早い段階から持ち寄り、分析のための問いと検証方法を共同で形成する進め方を整理した資料です。

この進め方を、Collaborative Question Framing for Analytics（CQFA：仮称）と呼びます。

CQFAでは、問いと検証方法を共同で形成した後、成果が実際の判断や行動に役立つかを実利用から確かめる必要がある場合に、小さなデータ成果を作成して利用します。そこから得た分析結果や利用結果を、問い、指標、データ、実装、利用方法の見直しへ戻します。

## 読み方

要点をまとめたConcept Proposalと、背景、中心モデル、役割関係、試行方法を説明する詳細版を用意しています。

- まず提案の要点を確認する場合: [Concept Proposal](concept-proposal.ja.md)
- 背景や提案内容を通読する場合: [詳細版](detailed/full.ja.md)
- 詳細版の特定の章だけ読む場合: [章別資料](detailed/chapters/)
- 参考文献・参照情報を見る場合: [参考文献](appendices/references.ja.md)
- Analytics関連資料の一覧へ戻る場合: [Analytics資料一覧](../README.md)
- CQFAに関連する補助ツール構想を見る場合: [DWHで答えられるかを見立てる補助ツール構想](assets/cqfa-dwh-answerability-support-tool-concept.ja.pdf)

最初に [Concept Proposal](concept-proposal.ja.md) を読み、必要に応じて各章末のリンクから詳細版の該当箇所を参照することを想定しています。

## 資料の構成

### Concept Proposal

[concept-proposal.ja.md](concept-proposal.ja.md) は、CQFAの要点をまとめた公開用の提案資料です。

主に次を扱います。

- CQFAを検討する背景と問題
- 分析のための問いと検証方法を共同で形成する進め方
- 小さなデータ成果を実際に利用し、フィードバックから学ぶ流れ
- 必要な専門知と役割の境界
- 限定的な試行の条件
- 期待する価値と、追加負担・リスク
- 試行後の継続、修正、保留、停止の判断

恒久導入や全社展開を求める資料ではありません。現在のデータ利活用にCQFAを試す理由があるかを確認し、理由がある場合に、適切な一件を対象とした限定的な試行を検討するための判断材料として作成しています。

### 詳細版

[detailed/full.ja.md](detailed/full.ja.md) は、CQFAの着想、定義、適用範囲、役割関係、具体例、試行方法、評価方法を説明する詳細資料です。

章別の資料は [detailed/chapters/](detailed/chapters/) に配置しています。

- [本Proposalの位置づけ](detailed/chapters/00-positioning.ja.md)
- [問題と共同発見](detailed/chapters/01-problem-and-joint-discovery.ja.md)
- [CQFAの中心モデル](detailed/chapters/02-cqfa-model.ja.md)
- [小さな試行と適応](detailed/chapters/03-small-trial-and-adaptation.ja.md)
- [まとめ](detailed/chapters/04-summary.ja.md)
- [おわりに](detailed/chapters/99-closing.ja.md)

### 関連する補助ツール構想

CQFAにおける共同検討を補助する着想として、既存DWHで問いに答えられそうか、DWH内の変更やSource側の確認が必要かを見立て、次の調査方向を提示する補助ツール構想を整理しています。

このツールはCQFAを成立させるための必須要素ではありません。dbt環境に接続されたMCP Server、LLM、Pythonによる固定的な処理制御を組み合わせる技術構成を想定していますが、現時点では未実装・未検証です。

- [DWHで答えられるかを見立てる補助ツール構想（PDF）](assets/cqfa-dwh-answerability-support-tool-concept.ja.pdf)

## 想定する読み手

- データ・分析チームの責任者またはリード
- データ利活用に関わる業務領域の責任者
- Analytics Engineer（アナリティクスエンジニア）
- Data Analyst（データアナリスト）
- Data Engineer（データエンジニア）
- データ・分析領域の協働方法や職能設計に関心がある人
- QA、アジャイル、Analytics Engineeringの接点に関心がある人

## 資料の位置づけ

CQFAは、完成した標準手法や、実務で有効性が検証された運用モデルではありません。

Quality Assistance / Quality Enablementと、アジャイルにおける小さな成果、短いフィードバックループ、継続的な適応から得た示唆を、Analytics Engineeringの視点から組織的なデータ利活用へ応用した提案仮説です。

すべてのデータ利活用へ適用することや、既存の分担型フローを置き換えることは想定していません。また、Analytics Engineerが業務上の判断、指標の承認、相談の受付、職種間の調整を一元的に担うことを提案するものでもありません。

この資料は、学習、既存概念の整理、個人ポートフォリオでの実装経験をもとに作成しています。CQFAによる組織的な協働、参加者の負担軽減、データ利活用の定着、事業成果などが実務で実証されたことを示す資料ではありません。

詳細な前提と扱う範囲は、[本Proposalの位置づけ](detailed/chapters/00-positioning.ja.md)を参照してください。

## 関連する着想

CQFAは、主に次の考え方から着想を得ています。

- Quality Assurance（QA：品質保証）における早期関与とShift Left
- Quality Assistance / Quality Enablement
- 異なる専門知を持ち寄る協働
- アジャイルにおける小さな成果と短いフィードバックループ
- 実際の利用結果を次の問いや実装へ戻す検査と適応
- 成果だけでなく、参加者の負担と持続可能性も評価すること

参考にした資料や外部情報は、[参考文献](appendices/references.ja.md)にまとめています。

## 著作権について

この資料は閲覧および個人参照目的で公開しています。無断転載、再配布、改変、翻訳、商用利用、学習データセット等への利用は許可していません。

詳細は [COPYRIGHT.md](COPYRIGHT.md) を参照してください。

## 管理方針

Concept Proposalは [concept-proposal.ja.md](concept-proposal.ja.md) を公開用の提案資料として管理します。

詳細版の編集・更新は、主に [detailed/chapters/](detailed/chapters/) 配下の章別ファイルを対象とし、[detailed/full.ja.md](detailed/full.ja.md) は通読用の統合版として扱います。

Concept Proposalは詳細版の単純な要約ではなく、読み手が限定的な試行を検討するための判断材料として独立して構成します。
