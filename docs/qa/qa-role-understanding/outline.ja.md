# QA職能入門：Outline

このページは、QA職能理解資料の全体構成を示す章案内です。  
各章の概要を確認し、関心のある章へ移動するためのナビゲーションとして用意しています。

## Reading Guide

- 資料の入口に戻る: [README.md](README.md)
- 通読する場合: [full.ja.md](full.ja.md)
- 章ごとに読む場合: [chapters/](chapters/)
- 参考文献・参照情報を見る場合: [appendices/references.ja.md](appendices/references.ja.md)

## Chapters

### [0. この資料の位置づけ](chapters/00-positioning.ja.md)

資料の目的、想定読者、扱う範囲、扱わない範囲を整理する。

### [1. QAを学ぶときに最初に押さえること](chapters/01-qa-basics.ja.md)

QAをTesting(テスト)だけに限定せず、不具合検出と不具合予防、品質を扱える状態づくりとして理解する。

### [2. QA / QC / Testing / Quality Engineeringの違い](chapters/02-qa-qc-testing-quality-engineering.ja.md)

QA、QC(Quality Control:品質管理)、Testing、Quality Engineering、Quality Assistance / Quality Enablementの関係を整理する。

### [3. ソフトウェア品質を分解して考える](chapters/03-software-quality-perspectives.ja.md)

機能品質、ドメイン品質、データ品質、セキュリティ・権限品質、UX品質、運用品質、開発プロセス品質に分けて品質を捉える。

### [4. QAはいつ関与するのか](chapters/04-qa-in-development-lifecycle.ja.md)

要求、仕様、設計、実装、レビュー、テスト、リリース、運用後フィードバックにおけるQA関与を整理する。Shift Left TestingやAgile / DevOpsの文脈も踏まえ、品質観点を開発ライフサイクル全体に組み込む考え方を扱う。

### [5. 要求をテスト可能な形にする](chapters/05-testable-requirements.ja.md)

Acceptance Criteria(受け入れ条件)、具体例、Example Mapping、Specification by Example、Three Amigos、ATDD、BDD、TDDを扱う。

### [6. テスト設計の基本](chapters/06-test-design-basics.ja.md)

リスクベースドテスト、同値分割、境界値分析、デシジョンテーブル、状態遷移テスト、探索的テストなどを整理する。

### [7. テスト設計から自動化・品質ゲートへ](chapters/07-test-automation-quality-gates.ja.md)

テスト条件をUnit / API / Integration / E2Eなどに割り当て、自動化とCI/CD品質ゲートへ接続する考え方を整理する。

### [8. 品質メトリクスとデータ活用](chapters/08-quality-metrics.ja.md)

テスト結果、欠陥、インシデント、問い合わせ、PR・レビュー・リリースデータなどを品質改善へ戻す考え方を扱う。

### [9. DQA: データ品質をQAの観点から理解する](chapters/09-data-quality-assurance.ja.md)

データそのものを品質対象として捉え、業務、分析、意思決定、AI評価との接続を整理する。

### [10. 他職種連携と品質の言語化](chapters/10-cross-functional-quality-language.ja.md)

PM、開発者、デザイナー、CS / サポート、SRE / 運用、データ / 分析などの品質情報を共通言語へ変換する考え方を扱う。

### [11. AI時代のQA](chapters/11-qa-in-ai-era.ja.md)

AI-assisted QAとAI Product QAの両面から、AI時代におけるQA活動、評価、品質制御、DQAとの接続を整理する。

### [12. QAが新しいプロジェクトへ参加するときの着眼点と進め方](chapters/12-joining-new-projects-as-qa.ja.md)

QAが新しいプロジェクトに参加するとき、何を観察し、どこから品質活動へ接続するかを整理する。

### [13. まとめ：QAを「品質を扱える状態を作る活動」として捉える](chapters/13-summary.ja.md)

資料全体を振り返り、QAを品質を作り、判断し、改善へ戻すための職能として総括する。

### [おわりに](chapters/99-closing.ja.md)

資料全体の立ち位置、想定される使われ方、品質を考えるための手がかりとしての位置づけを述べる。

## Appendices

### [Appendix A. 参考文献・参照情報](appendices/references.ja.md)

本文作成時に参照した公開情報、公式ドキュメント、標準規格の概要ページ、実務者向け記事、技術ブログ、業界レポート、書籍情報を整理する。
