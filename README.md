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
- Collaborative analytics problem framing: **connecting business questions, metrics, data requirements, intended use, and validation through early cross-functional dialogue**
- Supporting quality and delivery perspective: **testability, data quality, quality gates, reproducibility, and Agile / DevOps feedback loops**

## Supporting materials

### Analytics engineering modeling

- **[Analytics Engineering Modeling and Operating Approach](docs/analytics-engineering-modeling/analytics_engineering_modeling_operating_approach_overview.md)**  
  A concise overview of my analytics engineering modeling and operating approach, including dbt modeling, model grain, data tests, BI / semantic layer consumption, stakeholder alignment, business review signals, portfolio evidence, and an initial 30 / 60 / 90-day contribution approach.

- **[Full material set: Analytics Engineering Modeling Perspectives](docs/analytics-engineering-modeling/00_analytics_engineering_modeling_index.md)**  
  A detailed material set covering common dbt modeling principles and a reusable customer lifecycle / monetization analytics case study.

### Collaborative question framing for analytics

- **[Collaborative Question Framing for Analytics (CQFA)](docs/analytics/collaborative-question-framing-for-analytics/README.md)**  
  A concept proposal for collaboratively turning uncertain business ideas into analyzable questions by connecting intended decisions, metrics, required data, analytical outputs, and validation methods.

  CQFA draws on ideas from Analytics Engineering, Quality Assistance / Quality Enablement, and iterative Agile feedback. It proposes starting with a limited case, producing a small usable data outcome, and evaluating both its practical value and the additional coordination burden before considering broader adoption.

  The current proposal materials are available in Japanese.  
  Links: [Concept Proposal](docs/analytics/collaborative-question-framing-for-analytics/concept-proposal.ja.md) · [Detailed version](docs/analytics/collaborative-question-framing-for-analytics/detailed/full.ja.md)

### Quality engineering and Agile development

The following materials are **currently available in Japanese**.

- **[QA Role Foundations: Building, Evaluating, and Improving Quality](docs/qa/qa-role-understanding/README.md)**  
  A learning and reference material that examines QA beyond test execution, covering testable requirements, test design, automation, quality gates, quality metrics, Data Quality Assurance, cross-functional quality practices, and QA in the AI era.  
  Links: [Entry](docs/qa/qa-role-understanding/README.md) · [Outline](docs/qa/qa-role-understanding/outline.ja.md)

- **[Reinterpreting the AE / DE Portfolio from a Quality Perspective — Overview](docs/qa/qa-perspective-portfolio-overview.ja.md)** · **[Detailed version](docs/qa/qa-perspective-portfolio-detailed.ja.md)**  
  A supplementary material that reinterprets the AE / DE portfolio through QA, Data Quality Assurance, Quality Engineering, and operational-quality perspectives. It connects implementation evidence such as validation, authorization boundaries, data contracts, automated tests, reproducibility, monitoring, and reviewable documentation to broader quality activities.

- **[Agile Development Foundations: Delivering Value in Short Cycles and Treating Quality Continuously](docs/agile/agile-development-foundations/README.md)**  
  A learning and reference material covering the Agile Manifesto, Scrum, Kanban, development flow, quality activities within short cycles, DevOps feedback loops, and AI-assisted software development.  
  Links: [Entry](docs/agile/agile-development-foundations/README.md) · [Outline](docs/agile/agile-development-foundations/outline.ja.md)

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

直近では、QA(Quality Assurance / Quality Assistance: 品質保証 / 品質支援) / Quality Engineeringの視点から、自身のAE / DEポートフォリオ群を再解釈し、品質・検証・再現性・テスト容易性に加え、チームで品質を作り込む仕組みへどう接続できるかを整理しています。Quality Assistance / Quality Enablementの考え方にも関心があり、品質活動をチーム全体で支えられる状態にすることを重視しています。

あわせて、こうした品質活動が実際の開発プロセスの中でどのように位置づけられるかを理解するため、現在多くのソフトウェア開発現場で用いられているアジャイル開発についても、Agile Manifesto、Scrum、Kanban、短いサイクルにおける品質活動、DevOps、AI支援との接続まで、基礎から調査・整理しています。

これらの学習とポートフォリオ開発をもとに、業務上の着想や不確実な問題を、分析可能な問い、指標、必要なデータ、利用場面、検証方法へ関係者と共同で具体化する進め方を、Collaborative Question Framing for Analytics（CQFA：仮称）として整理しました。

CQFAは完成した標準手法や実務で有効性を検証済みの運用モデルではなく、限定した一件で小さなデータ成果を実利用し、得られる価値と参加者の負担の両方を確認するためのConcept Proposalです。

## 関連資料

**[ポートフォリオ設計の考え方](docs/ae-de-portfolio-design-summary.md)**  
主要プロジェクトを、operational data から decision-ready analytics までの小さな AE / DE workflow としてどのように接続しているかを説明した資料です。

### Analytics engineering modeling materials

- **[Analytics Engineering におけるモデリングと運用方針](docs/analytics-engineering-modeling/analytics_engineering_modeling_operating_approach_overview.ja.md)**  
  dbt modeling、model grain、data tests、BI / semantic layer、stakeholder alignment、business review signals、ポートフォリオによる実装証拠、入社後30 / 60 / 90日の初期貢献方針を整理した概要版です。

- **[詳細版: Analytics Engineering Modeling Perspectives](docs/analytics-engineering-modeling/00_analytics_engineering_modeling_index.md)**  
  共通の dbt modeling principles と、customer lifecycle / monetization analytics のケーススタディを含む詳細版です。

### Collaborative Question Framing for Analytics（CQFA）

- **[Collaborative Question Framing for Analytics（CQFA）](docs/analytics/collaborative-question-framing-for-analytics/README.md)**  
  業務上の着想や不確実な問題を、分析可能な問い、指標、必要なデータ、利用場面、検証方法へ関係者と共同で具体化する進め方を整理したConcept Proposalです。

  Analytics Engineering、Quality Assistance / Quality Enablement、アジャイルにおける小さな成果と短いフィードバックループから得た示唆を接続しています。限定した一件で小さなデータ成果を実利用し、成果の有用性だけでなく、対話、調整、参加、記録などの追加負担も評価したうえで、適用継続を判断する構成としています。

  [Concept Proposal](docs/analytics/collaborative-question-framing-for-analytics/concept-proposal.ja.md) · [詳細版](docs/analytics/collaborative-question-framing-for-analytics/detailed/full.ja.md)

### QA / Quality Engineering / Agile関連資料

- **[QA職能入門：品質を作り、判断し、改善へ戻す仕組み](docs/qa/qa-role-understanding/README.md)**  
  QAについての基礎を学ぶために、公開情報をもとに調査・分析・整理した学習・参照用資料です。テスト実行だけでなく、要求整理、テスト設計、自動化、品質ゲート、品質メトリクス、DQA、他職種連携、AI時代のQAまでを扱っています。  
  [資料入口](docs/qa/qa-role-understanding/README.md) · [まず全体像を見る（目次）](docs/qa/qa-role-understanding/outline.ja.md)

- **[AE / DEポートフォリオを品質の観点から読み直す — 概要版](docs/qa/qa-perspective-portfolio-overview.ja.md)** · **[詳細版](docs/qa/qa-perspective-portfolio-detailed.ja.md)**  
  AE / DEポートフォリオ群を、QA / DQA / Quality Engineering / 運用品質の観点から再解釈した補助資料です。バリデーション、権限境界、データ契約、自動テスト、再現性、モニタリング、ドキュメントなどの実装を、品質活動としてどのように捉えられるか整理しています。

- **[アジャイル開発の基礎理解：短いサイクルで価値を届け、品質を継続的に扱う仕組み](docs/agile/agile-development-foundations/README.md)**  
  Agile Manifesto、Scrum、Kanban、開発の流れ、短いサイクルにおける品質活動、DevOps、AI支援を取り入れた開発までを整理した学習・参照用資料です。  
  [資料入口](docs/agile/agile-development-foundations/README.md) · [まず全体像を見る（目次）](docs/agile/agile-development-foundations/outline.ja.md)

## 主要ポートフォリオ

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
