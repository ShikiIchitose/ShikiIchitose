# Analytics Engineering Portfolio | Data Engineering / Python / Go

I’m primarily seeking **analytics engineering** roles, with strong interest in **data engineering** and supporting backend-oriented data applications. My portfolio focuses on turning raw operational data into tested, documented, and decision-ready analytical outputs, with longer-term interest in **artificial intelligence / machine learning (AI / ML)**.

**Proof points:** Tested • continuous integration (CI)-gated • Reproducible outputs • Reviewable documentation • Released portfolio projects

**Portfolio rationale:** [Why this portfolio: from motivation to design rationale](docs/ae-de-portfolio-design-summary.md)  
A short rationale document explaining how the projects connect as a simulated AE/DE workflow from operational data to decision-ready analytics.  

## Current focus

- Data and analytics engineering workflows: **generate → validate → model → test → report → visualize**
- Backend-oriented data applications: **application programming interface (API) design, ingestion boundaries, Django applications, FastAPI services, Go HTTP services**
- Analytical systems: **dbt, DuckDB, BigQuery, Parquet, Structured Query Language (SQL), Looker Studio dashboard artifacts**
- Production-minded implementation: **testing, CI, Docker, documentation, reproducible runs, reviewable artifacts**

## Analytics engineering modeling materials

- **[Analytics Engineering Modeling Perspectives](docs/analytics-engineering-modeling/00_analytics_engineering_modeling_index.md)**  
  A portfolio-based material set on dbt modeling, model grain, tests, documentation, BI / semantic layer consumption, stakeholder alignment, decision-support outputs, and an initial 90-day ramp-up and contribution approach.

## Featured projects

- **[access-governance-warehouse](https://github.com/ShikiIchitose/access-governance-warehouse)** — Analytics engineering warehouse built with **dbt + DuckDB + BigQuery + Looker Studio + Python** for enterprise AI tool access governance
  - Links: [README](https://github.com/ShikiIchitose/access-governance-warehouse#readme) · [Dashboard docs](https://github.com/ShikiIchitose/access-governance-warehouse/blob/main/docs/looker-studio-dashboard.md) · [CI](https://github.com/ShikiIchitose/access-governance-warehouse/actions) · [Releases](https://github.com/ShikiIchitose/access-governance-warehouse/releases)
  - Highlights:
    - Deterministic synthetic raw Parquet data generation for reproducible source fixtures
    - Layered dbt modeling: **sources → staging → core → intermediate → marts**
    - Local DuckDB path preserved as the primary clone-and-run review workflow
    - Optional BigQuery execution path using the same dbt source contract, model tree, marts, and data tests
    - Looker Studio dashboard artifacts connected to BigQuery marts
    - 315 dbt data tests covering source contracts, model grain, reconciliation, and mart logic
    - Clear separation between transformation failures, business review signals, and business intelligence (BI) presentation logic

- **[ai-tool-access-requests](https://github.com/ShikiIchitose/ai-tool-access-requests)** — Internal workflow app built with **Django + PostgreSQL** for enterprise AI tool access requests and approvals
  - Links: [README](https://github.com/ShikiIchitose/ai-tool-access-requests#readme) · [CI](https://github.com/ShikiIchitose/ai-tool-access-requests/actions) · [Releases](https://github.com/ShikiIchitose/ai-tool-access-requests/releases)
  - Highlights:
    - Authentication and authorization with clear requester / reviewer / admin boundaries
    - Role-based access control (RBAC) and form validation for a minimal but realistic business workflow
    - Inspection-only Django admin customization and management commands for demo-state reset
    - CI-gated tests covering approval flow, permissions, and core business rules
  - [Demo](https://ai-tool-access-requests.onrender.com/)

- **[go-ingestion-api](https://github.com/ShikiIchitose/go-ingestion-api)** — Minimal Go HTTP ingestion API for strict AI tool usage event ingestion
  - Links: [README](https://github.com/ShikiIchitose/go-ingestion-api#readme) · [CI](https://github.com/ShikiIchitose/go-ingestion-api/actions) · [Releases](https://github.com/ShikiIchitose/go-ingestion-api/releases)
  - Highlights:
    - One JSON event per HTTP request with a strict request contract
    - Content-Type enforcement, request body size limit, strict JSON decoding, and unknown field rejection
    - Event model validation and compact user / tool reference validation
    - Accepted events persisted as append-only JSONL raw storage
    - Docker multi-stage build and GitHub Actions CI
    - Positioned as an upstream ingestion boundary for downstream warehouse and BI workflows

- **[analytics-metrics-api](https://github.com/ShikiIchitose/analytics-metrics-api)** — Read-only analytics API built with **FastAPI + DuckDB + Parquet** for synthetic SaaS-like event and job-run data
  - Links: [README](https://github.com/ShikiIchitose/analytics-metrics-api#readme) · [CI](https://github.com/ShikiIchitose/analytics-metrics-api/actions) · [Releases](https://github.com/ShikiIchitose/analytics-metrics-api/releases)
  - Highlights:
    - Resource-oriented API design with explicit HTTP semantics
    - Stable metric contracts and reproducible local testing with committed golden outputs
    - Offline-first backend / analytics engineering setup using DuckDB queries over Parquet-backed local data
    - Deterministic synthetic data generation for a small, reviewable analytics API project
  - [Demo](https://analytics-metrics-api.onrender.com/)

- **[Exoplanet catalog analysis](https://github.com/ShikiIchitose/exoplanet-analysis-report)** — Reproducible analytics pipeline using **NASA Exoplanet Archive TAP (Table Access Protocol)** data with DuckDB as the local analytical store
  - Links: [README](https://github.com/ShikiIchitose/exoplanet-analysis-report#readme) · [CI](https://github.com/ShikiIchitose/exoplanet-analysis-report/actions) · [Releases](https://github.com/ShikiIchitose/exoplanet-analysis-report/releases)
  - Highlights:
    - Fetch → validate → preprocess → analyze → report workflow
    - Reproducibility and auditability through seeded bootstrap, schema snapshots, and locked dependencies
    - Designed for real-world data issues such as schema drift, missing values, outliers, and automated reporting
    - Domain-agnostic pipeline scaffolding with DuckDB as a local analytical store

- **[url-monitor](https://github.com/ShikiIchitose/url-monitor)** — Python command-line interface (CLI) to check URL availability and latency, then generate a Markdown report and JSON results
  - Links: [README](https://github.com/ShikiIchitose/url-monitor#readme) · [CI](https://github.com/ShikiIchitose/url-monitor/actions) · [Releases](https://github.com/ShikiIchitose/url-monitor/releases)
  - Highlights:
    - Reproducible runs and clear, reviewable outputs
    - CI quality gates with Ruff and pytest
    - Compact project covering CLI design, HTTP request handling, validation, test isolation, and report output

## Growth direction

In the near term, I’m focusing primarily on **analytics engineering**, with strong interest in **data engineering** and supporting backend-oriented data applications.

My current priority is to strengthen practical fundamentals in data modeling, schema design, data quality management, dbt-based analytics engineering, tested data transformations, reproducible data pipelines, batch processing, and decision-ready reporting.

Technologies I’m currently focusing on include **Python, Structured Query Language (SQL), dbt, DuckDB, BigQuery, Parquet, PostgreSQL, Looker Studio dashboard artifacts, Go, FastAPI, and Django**.

I’m especially interested in building analytical systems that turn raw operational data into trusted marts, documented metrics, automated quality checks, static reports, and business intelligence (BI)-facing artifacts.

Backend-oriented work is currently positioned as a supporting skill for data products, including read-only application programming interface (API) design, ingestion boundaries, validation, and internal workflow applications that produce or expose analytical data.

In the medium term, I want to broaden toward **applied data science** and decision-oriented analytics, including metric design, business logic documentation, automated reporting, statistical estimation, uncertainty evaluation, experiment design, and connecting analytical results to business decisions.

In the long term, I’m interested in connecting this foundation to **machine learning (ML)** and **artificial intelligence (AI)** systems, including feature engineering, machine learning pipelines, model evaluation, monitoring, deployment, retraining workflows, and machine learning operations (MLOps).

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
<summary>日本語要約版 / Japanese</summary>

## 概要

Analytics Engineering を主軸に、Data Engineering とデータプロダクトを支えるバックエンド実装にも関心があります。

現在は、AIツール利用ガバナンスを題材にしたポートフォリオを中心に、申請・承認アプリ、利用イベント取り込みAPI、dbtによる分析基盤、BigQuery実行、Looker Studio dashboard artifacts までを小規模に実装しています。

重視している点は、再現性、検証可能性、テスト、CI、ドキュメント、レビューしやすい成果物です。

## 主なポートフォリオ

主要ポートフォリオ群をまとめたドキュメントを用意しています。
[AE/DEポートフォリオ構成意図](docs/ae-de-portfolio-design-summary.ja.md)

- access-governance-warehouse  
  dbt、DuckDB、BigQuery、Looker Studio、Pythonを用いたAnalytics Engineeringポートフォリオです。AIツール利用の申請・承認・利用・コスト・例外を分析できるwarehouseとBI artifactsを構築しています。

- ai-tool-access-requests  
  Django + PostgreSQLで実装したAIツール利用申請・承認アプリです。requester / reviewer / admin の権限分離、RBAC、フォームバリデーション、業務ルールのテストを扱っています。

- go-ingestion-api  
  Goで実装したAIツール利用イベント向けのHTTP ingestion APIです。strict JSON validation、reference validation、append-only JSONL storage、Docker、CIを扱っています。

- analytics-metrics-api  
  FastAPI + DuckDB + Parquetで実装したread-only analytics APIです。KPI定義、resource-oriented API design、golden-output testingを扱っています。

## 今後の方向性

直近ではAnalytics Engineeringを主軸に、dbt、SQL、DuckDB、BigQuery、Pythonを用いたデータモデリング、data quality、mart設計、BI-facing reportingを強化したいと考えています。

中期的には、指標設計、ビジネスロジックの明文化、統計的推定、意思決定につながる分析へ広げ、長期的にはMachine Learning / AI systemsにも接続していきたいです。

## 学位
工学修士(航空宇宙工学)

大学院では数値流体力学（CFD）を専門とし、スクラムジェットエンジン、超音速燃焼流、ブラックホール降着円盤の数値シミュレーションに取り組みました。  
FORTRAN を主に用い、スーパーコンピュータでの計算条件設計、格子作成、実装、解析・可視化までを担当しました。  
JAXA 研究室と連携し、スクラムジェットエンジン・インレット形状の最適化研究を行い修士号を取得しました。

</details>
