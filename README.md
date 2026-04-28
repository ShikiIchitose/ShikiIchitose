# Python | Data & Analytics Engineering Portfolio

I’m primarily seeking **data engineering / analytics engineering** roles, with strong interest in backend-oriented data applications and a longer-term interest in **data science and artificial intelligence / machine learning (AI / ML)**.

**Proof points:** Tested • continuous integration (CI)-gated • Reproducible outputs • Reviewable documentation

## Current focus (near-term)
- Production-minded Python: **command-line interface (CLI) design, backend APIs, Django applications, testing, CI, reproducible runs**
- Data and analytics engineering workflows: **fetch → validate → preprocess → model → analyze → report**
- Local analytical systems: **DuckDB, Parquet, dbt, SQL, reproducible data artifacts**

## Featured projects

- **[url-monitor](https://github.com/ShikiIchitose/url-monitor)** — Python CLI to check URL availability / latency and generate a **Markdown report + JSON results**
  - Links: [README](https://github.com/ShikiIchitose/url-monitor#readme) · [CI](https://github.com/ShikiIchitose/url-monitor/actions) · [Releases](https://github.com/ShikiIchitose/url-monitor/releases)
  - Highlights:
    - Reproducible runs and clear, reviewable outputs
    - CI quality gates (**ruff / pytest**)
    - Responsible network-use guidance with portfolio-friendly defaults

- **[Exoplanet catalog analysis](https://github.com/ShikiIchitose/exoplanet-analysis-report)** — Reproducible analytics pipeline using **NASA Exoplanet Archive TAP (Table Access Protocol)** data  
  *(fetch → validate → preprocess → analyze → report & plots)* with **DuckDB** as the local warehouse
  - Links: [README](https://github.com/ShikiIchitose/exoplanet-analysis-report#readme) · [CI](https://github.com/ShikiIchitose/exoplanet-analysis-report/actions) · [Releases](https://github.com/ShikiIchitose/exoplanet-analysis-report/releases)
  - Highlights:
    - Reproducibility and auditability: seeded bootstrap, snapshot / schema hashes, and locked dependencies captured per run in [`run.json`](https://github.com/ShikiIchitose/exoplanet-analysis-report/blob/main/sample-artifacts/run.json)
    - Output artifacts are generated deterministically under a fixed directory structure
    - Designed for real-world data issues: schema drift, missing values, outliers, and automated reporting
    - Reusable domain-agnostic pipeline scaffolding with DuckDB as a local analytical store

- **[analytics-metrics-api](https://github.com/ShikiIchitose/analytics-metrics-api)** — Read-only analytics / metrics API built with **FastAPI + DuckDB + Parquet** for synthetic SaaS-like event and job-run data
  - Links: [README](https://github.com/ShikiIchitose/analytics-metrics-api#readme) · [CI](https://github.com/ShikiIchitose/analytics-metrics-api/actions) · [Releases](https://github.com/ShikiIchitose/analytics-metrics-api/releases)
  - Highlights:
    - RESTful, resource-oriented API design with explicit HTTP semantics (**200 / 404 / 422**)
    - Stable metric contracts and reproducible local testing with committed golden outputs
    - Offline-first backend / analytics engineering setup using DuckDB queries over Parquet-backed local data
    - Deterministic synthetic data generation to demonstrate backend and analytics engineering fundamentals in a small, reviewable project
  - [Demo](https://analytics-metrics-api.onrender.com/)

- **[ai-tool-access-requests](https://github.com/ShikiIchitose/ai-tool-access-requests)** — Internal workflow app built with **Django + PostgreSQL** for enterprise AI tool access requests and approvals
  - Links: [README](https://github.com/ShikiIchitose/ai-tool-access-requests#readme) · [CI](https://github.com/ShikiIchitose/ai-tool-access-requests/actions) · [Releases](https://github.com/ShikiIchitose/ai-tool-access-requests/releases)
  - Highlights:
    - Authentication and authorization with clear requester / reviewer / admin boundaries
    - Role-based access control (RBAC) and form validation for a minimal but realistic business workflow
    - Inspection-only Django admin customization and management commands for demo-state reset
    - CI-gated tests covering approval flow, permissions, and core business rules
  - [Demo](https://ai-tool-access-requests.onrender.com/)

- **[access-governance-warehouse](https://github.com/ShikiIchitose/access-governance-warehouse)** — Local analytics engineering warehouse built with **dbt + DuckDB + Python** for enterprise AI tool access governance
  - Links: [README](https://github.com/ShikiIchitose/access-governance-warehouse#readme) · [CI](https://github.com/ShikiIchitose/access-governance-warehouse/actions) · [Releases](https://github.com/ShikiIchitose/access-governance-warehouse/releases)
  - Highlights:
    - Deterministic synthetic raw Parquet data generation for reproducible source fixtures
    - Layered dbt modeling: **sources → staging → core → intermediate → marts**
    - Business-facing marts for access requests, tool adoption, spend alignment, review candidates, and governance exceptions
    - 315 dbt data tests covering source contracts, model grain, reconciliation, and mart logic
    - dbt documentation, lineage graph, domain assumptions, testing strategy, and generated static governance report
    - Clear separation between transformation failures and business review signals

## Growth direction

In the near term, I’m focusing on **data engineering**, **analytics engineering**, and backend-oriented data applications.

My current priority is to strengthen practical fundamentals in data modeling, schema design, data quality management, batch processing, reproducible data pipelines, dbt-based analytics engineering, tested data transformations, and read-only application programming interface (API) design for analytical data access.

Technologies I’m currently focusing on include **Python, structured query language (SQL), dbt, DuckDB, FastAPI, Parquet, and PostgreSQL**.

In the medium term, I want to broaden toward **applied data science** and decision-oriented analytics, including metric design, business logic documentation, automated reporting, statistical estimation, uncertainty evaluation, experiment design, and connecting analytical results to business decisions.

In the long term, I’m interested in connecting this foundation to **machine learning (ML)** and **artificial intelligence (AI)** systems, including feature engineering, machine learning pipelines, model evaluation, monitoring, deployment, retraining workflows, Docker, and machine learning operations (MLOps).

## Background
M.E. in Aerospace Engineering

In graduate school, I specialized in computational fluid dynamics (CFD), working on numerical simulations of scramjet engines, supersonic combustion flows, and black hole accretion disks.

Using FORTRAN as my primary language, I was responsible for simulation condition design, grid generation, implementation, analysis, and visualization on supercomputing environments.

In my master’s program, I collaborated with a JAXA research laboratory on the optimization of scramjet engine inlet geometry and earned a master’s degree.

## Contact
Please use the links on my GitHub profile.

---

### Notes
This profile emphasizes engineering practices and reproducible deliverables over domain-specific research claims.

<details>
<summary>日本語版 / Japanese</summary>

現在、直近では **データエンジニアリング / Analytics Engineering(AE:分析エンジニアリング) / バックエンド寄りのデータアプリケーション実装** を主軸に仕事を探しています。  
長期的には **Data Science / Artificial Intelligence(AI:人工知能) / Machine Learning(ML:機械学習)** にも段階的に接続していきたいと考えています。

再現性（reproducibility）、検証可能性（verifiability）、保守しやすさ（maintainability）を重視し、CLI(command-line interface:コマンドラインインターフェース)、分析パイプライン、read-only API(application programming interface:アプリケーションプログラミングインターフェース)、Django 業務アプリ、dbt + DuckDB による分析基盤を GitHub ポートフォリオとして継続開発しています。

**要点:** テスト済み / CI(continuous integration:継続的インテグレーション) による品質ゲート / 再現可能な成果物 / レビューしやすいドキュメント

## 現時点の注力

- **実装衛生（engineering hygiene）**：CLI 設計、API 設計、Django アプリケーション、テスト、静的解析、CI による品質管理
- **データ処理 / 分析ワークフロー**：取得（fetch）→ 検証（validate）→ 前処理（preprocess）→ モデリング（model）→ 分析（analyze）→ レポート生成（report）
- **Analytics Engineering**：dbt による source / staging / core / intermediate / marts の設計、data tests、dbt docs、lineage、model grain（モデル粒度）の明示
- **実行の監査可能性（auditability）**：実行条件・入力・依存関係・出力をメタデータや成果物として残し、後から再現 / 検証できる設計

## 代表プロジェクト

- **[url-monitor](https://github.com/ShikiIchitose/url-monitor)**  
  URL の疎通 / 遅延を計測し、Markdown レポートと JSON 結果を生成する Python CLI です。テスト、CI、再現可能な実行結果を重視し、ネットワーク利用に配慮したデフォルト動作を明記しています。

- **[Exoplanet catalog analysis](https://github.com/ShikiIchitose/exoplanet-analysis-report)**  
  NASA 公開の系外惑星カタログ TAP(Table Access Protocol) データを用いた end-to-end 分析パイプラインです。DuckDB をローカルの分析用データストアとして利用し、取得、検証、前処理、分析、レポート / 図表生成までを一貫して実装しています。  
  - **再現性（reproducibility）**：ブートストラップ（bootstrap）の seed、スナップショット / スキーマのハッシュ、依存関係ロック等を `run.json` に記録
  - **実データ対応**：スキーマ変化、欠損値、外れ値を前提にした検証と分析
  - **再利用性（reusability）**：ドメイン非依存のパイプライン骨格、監査可能な実行メタデータ、DuckDB ローカル集計という構成

- **[analytics-metrics-api](https://github.com/ShikiIchitose/analytics-metrics-api)**  
  synthetic SaaS-like event data と job-run data を対象に、FastAPI + DuckDB + Parquet で実装した read-only の分析 / メトリクス API です。RESTful を意識したリソース指向の設計、安定した KPI(key performance indicator:重要業績評価指標) 定義、golden output を用いた再現可能なテストを通じて、バックエンドとデータ処理基盤の基礎力を示す小規模ポートフォリオです。  
  - **API 設計**：`/metrics`、`/metrics/{name}`、`/users/{user_id}` などのリソース指向 path と、`200 / 404 / 422` の明示的な HTTP ステータス運用
  - **再現性（reproducibility）**：deterministic な synthetic data 生成、committed golden JSON、offline-first テストによる安定した検証
  - **実装の狙い**：FastAPI による API 実装、DuckDB によるローカル集計、Parquet ベースのデータ管理を小さくレビューしやすい形で整理
  - [metrics の定義や KPI の扱い](https://github.com/ShikiIchitose/analytics-metrics-api/blob/main/METRICS.ja.md)
  - [開発サマリ](https://github.com/ShikiIchitose/analytics-metrics-api/blob/main/docs/development-highlights.ja.md)
  - [公開デモ](https://analytics-metrics-api.onrender.com/)

- **[ai-tool-access-requests](https://github.com/ShikiIchitose/ai-tool-access-requests)**  
  Django + PostgreSQL で実装した、エンタープライズ向け AI ツール利用申請・承認のための内部業務アプリです。認証・認可、requester / reviewer / admin の権限分離、フォームバリデーション、業務ルールを踏まえたレビュー導線、自動テストを通じて、実務寄りのバックエンド基礎力を示す小規模ポートフォリオです。  
  - **権限設計**：requester / reviewer / admin の責務を分離し、review workflow を通常 UI 側で実行する構成
  - **業務ルール**：self-review prohibition、pending-only review、inactive tool handling などを明示的に扱う設計
  - **運用と品質**：inspection-only admin、management commands、CI により、最小構成でも保守しやすい内部業務アプリとして整理
  - [開発サマリ](https://github.com/ShikiIchitose/ai-tool-access-requests/blob/main/docs/development-highlights.ja.md)
  - [公開デモ](https://ai-tool-access-requests.onrender.com/)

- **[access-governance-warehouse](https://github.com/ShikiIchitose/access-governance-warehouse)**  
  dbt + DuckDB + Python を用いた、エンタープライズ向け AI ツール利用ガバナンスを題材にした分析基盤ポートフォリオです。決定論的な synthetic raw data generator により Parquet files を生成し、raw sources / staging / core / intermediate / marts の dbt layer に分けて、アクセス申請、承認状況、利用実績、コスト、ガバナンス例外を分析できる warehouse を構築しています。  
  - **Analytics Engineering**：sources → staging → core → intermediate → marts の layered dbt modeling
  - **検証設計**：315 件の dbt data tests により、source contracts、model grain、reconciliation、mart logic を検証
  - **分析出力**：access requests、tool adoption、spend alignment、review candidates、governance exceptions を business-facing marts として整理
  - **ドキュメント**：dbt docs、lineage graph、domain assumptions、testing strategy、static governance report を整備
  - **設計方針**：変換処理の不整合（transformation failure）と業務レビューシグナル（business review signal）を分離

## Growth direction / 今後伸ばしたい領域

直近では、**データエンジニアリング、Analytics Engineering、バックエンド寄りのデータアプリケーション** を中心に、実務に近い形で設計・実装・検証の経験を積みたいと考えています。

特に強化したい領域は以下です。

- データモデリング、スキーマ設計、データ品質管理
- バッチ処理・再現可能なデータパイプライン
- dbt を用いた source definitions、staging models、marts、data tests、documentation
- model grain、source contracts、reconciliation checks を意識したデータ変換処理
- テスト可能で監査しやすいデータ処理基盤
- 分析データを提供する read-only API の設計
- Python、SQL(structured query language:構造化問い合わせ言語)、dbt、DuckDB、FastAPI、Parquet、PostgreSQL

中期的には、**Applied Data Science** と意思決定につながる分析にも広げていきたいです。

- 分析しやすいデータモデルの設計
- 指標定義やビジネスロジックの明文化
- 分析 / レポーティングの自動化
- 統計的推定、不確実性評価、実験設計
- 仮説検証、分析結果の解釈、意思決定への接続

長期的には、これらの基礎を **Machine Learning / AI システム** に接続していきたいと考えています。

- 特徴量設計、継続的な分析高度化
- 機械学習パイプラインの設計
- モデル評価、検証、監視のための基盤
- デプロイ、運用、再学習を含む ML システム
- Docker、MLOps、モデル運用基盤

## 学位
工学修士(航空宇宙工学)

大学院では数値流体力学（CFD）を専門とし、スクラムジェットエンジン、超音速燃焼流、ブラックホール降着円盤の数値シミュレーションに取り組みました。  
FORTRAN を主に用い、スーパーコンピュータでの計算条件設計、格子作成、実装、解析・可視化までを担当しました。  
JAXA 研究室と連携し、スクラムジェットエンジン・インレット形状の最適化研究を行い修士号を取得しました。

</details>
