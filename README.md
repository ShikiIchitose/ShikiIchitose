# Python | Data & Analytics Portfolio

I’m primarily seeking **data engineering / analytics engineering** roles, with a longer-term interest in **data science and AI / machine learning (ML)**.

**Proof points:** Tested • continuous integration (CI)-gated • Reproducible outputs

## Current focus (near-term)
- Production-minded Python: **command-line interface (CLI) design, testing, CI, reproducible runs**
- End-to-end analytics workflows: **fetch → validate → preprocess → analyze → report & plots**

## Featured projects
- **[url-monitor](https://github.com/ShikiIchitose/url-monitor)** — Python CLI to check URL availability/latency and generate a **Markdown report + JSON results**
  - Links: [README](https://github.com/ShikiIchitose/url-monitor#readme) · [CI](https://github.com/ShikiIchitose/url-monitor/actions) · [Releases](https://github.com/ShikiIchitose/url-monitor/releases)
  - Highlights:
    - Reproducible runs and clear, reviewable outputs
    - CI quality gates (**ruff / pytest**)
    - Responsible network-use guidance (portfolio-friendly defaults)

- **[Exoplanet catalog analysis](https://github.com/ShikiIchitose/exoplanet-analysis-report)** — Reproducible analytics pipeline using **NASA Exoplanet Archive (TAP: Table Access Protocol)** data  
  *(fetch → validate → preprocess → analyze → report & plots)* with **DuckDB** as the local warehouse
  - Links: [README](https://github.com/ShikiIchitose/exoplanet-analysis-report#readme) · [CI](https://github.com/ShikiIchitose/exoplanet-analysis-report/actions) · [Releases](https://github.com/ShikiIchitose/exoplanet-analysis-report/releases)
  - Highlights:
    - Reproducibility & auditability: seeded bootstrap + snapshot/schema hashes + locked deps captured per run in [`run.json`](https://github.com/ShikiIchitose/exoplanet-analysis-report/blob/main/sample-artifacts/run.json)
    - Output artifacts are generated deterministically (reports/plots under a fixed directory)
    - Designed for real-world data issues: schema drift, missing values, outliers, automated reporting
    - Reusability: domain-agnostic pipeline scaffolding (fetch → validate → preprocess → analyze → report & plots) with DuckDB as the local warehouse

- **[analytics-metrics-api](https://github.com/ShikiIchitose/analytics-metrics-api)** — Read-only analytics / metrics API built with **FastAPI + DuckDB + Parquet** for synthetic SaaS event data
  - Links: [README](https://github.com/ShikiIchitose/analytics-metrics-api#readme) · [CI](https://github.com/ShikiIchitose/analytics-metrics-api/actions) · [Releases](https://github.com/ShikiIchitose/analytics-metrics-api/releases)
  - Highlights:
    - RESTful, resource-oriented API design with explicit HTTP semantics (**200 / 404 / 422**)
    - Stable metric contracts and reproducible local testing with committed golden outputs
    - Offline-first backend / analytics engineering setup using DuckDB queries over Parquet-backed local data
    - Deterministic synthetic data generation to demonstrate backend fundamentals and data / analytics engineering fundamentals in a small, reviewable project
  - [Demo](https://analytics-metrics-api.onrender.com/)

- **[ai-tool-access-requests](https://github.com/ShikiIchitose/ai-tool-access-requests)** — Internal workflow app built with **Django + PostgreSQL** for enterprise AI tool access requests and approvals
  - Links: [README](https://github.com/ShikiIchitose/ai-tool-access-requests#readme) · [CI](https://github.com/ShikiIchitose/ai-tool-access-requests/actions) · [Releases](https://github.com/ShikiIchitose/ai-tool-access-requests/releases)
  - Highlights:
    - Authentication / authorization with clear requester / reviewer / admin boundaries
    - Form validation and role-based workflow design for a minimal but realistic business application
    - Inspection-only admin and CI-gated tests covering approval flow and core business rules
  - [Demo](https://ai-tool-access-requests.onrender.com/)

## Longer-term direction
- I’m interested in **ML / AI** in the longer term, but I’m currently prioritizing analytics and data engineering fundamentals.

## Background
- M.S. in Aerospace Engineering
- Scientific / numerical computing with **FORTRAN** (plus C / Python), including runs on **high-performance computing (HPC)** systems

## Contact
Please use the links on my GitHub profile.

---

### Notes
This profile emphasizes engineering practices and reproducible deliverables over domain-specific research claims.

<details>
<summary><strong>日本語（概要）</strong></summary>

現在、直近は **データエンジニア／アナリティクスエンジニア（分析基盤寄り）** を主軸に仕事を探しています。あわせて、長期的には **データサイエンス／AI・ML** にも段階的に取り組む方針です。再現性（reproducibility）とデータ品質（data quality）を重視したパイプライン実装を、成果物として提示しています。

（要点）テスト済み／CI（continuous integration: 継続的インテグレーション）で品質ゲート／再現可能な成果物

## 現時点の注力
- **実装衛生（engineering hygiene）**：CLI設計、テスト（pytest）、静的解析（ruff）、CIによる品質管理
- **分析パイプライン（analytics pipeline）**：取得（fetch）→検証（validate）→前処理（preprocess）→分析（analyze）→レポート／図表生成（report & plots）
- **実行の監査可能性（auditability）**：実行条件・入力・依存関係をメタデータとして残し、後から再現／検証できる設計

## 代表プロジェクト
- **[url-monitor](https://github.com/ShikiIchitose/url-monitor)**  
  URLの疎通／遅延を計測し、MarkdownレポートとJSON結果を生成するPython CLIです。テスト／CI／再現可能な実行を重視し、ネットワーク利用に配慮したデフォルト動作を明記しています。

- **[Exoplanet catalog analysis](https://github.com/ShikiIchitose/exoplanet-analysis-report)**  
  NASA公開の系外惑星カタログ（TAP: Table Access Protocol）を用いた end-to-end 分析パイプラインです。DuckDBをローカルのデータウェアハウスとして利用しています。  
  - **再現性（reproducibility）**：ブートストラップ（bootstrap）の seed、スナップショット／スキーマのハッシュ、依存関係ロック等を `run.json` に記録
  - **再利用性（reusability）**：ドメイン非依存のパイプライン骨格＋監査可能な実行メタデータ＋DuckDBローカル集計という構成

- **[analytics-metrics-api](https://github.com/ShikiIchitose/analytics-metrics-api)**  
  synthetic SaaS event data を対象に、FastAPI + DuckDB + Parquet で実装した read-only の分析 / メトリクス API です。RESTful を意識したリソース指向の設計、安定したメトリクス定義、golden output を用いた再現可能なテストを通じて、バックエンド基礎力とデータ / 分析基盤の基礎力を示す小規模ポートフォリオです。  
  - **RESTful 設計**：`/metrics`、`/metrics/{name}`、`/users/{user_id}` などのリソース指向 path と、`200 / 404 / 422` の明示的な HTTP ステータス運用
  - **再現性（reproducibility）**：deterministic な synthetic data 生成、committed golden JSON、offline-first テストによる安定した検証
  - **実装の狙い**：FastAPI による API 実装、DuckDB によるローカル集計、Parquet ベースのデータ管理を小さくレビューしやすい形でまとめ、バックエンドとデータ処理基盤の基礎力を示す構成
  - [**metrics の定義や KPI の扱い**](https://github.com/ShikiIchitose/analytics-metrics-api/blob/main/METRICS.ja.md)
  - [**開発サマリ**](https://github.com/ShikiIchitose/analytics-metrics-api/blob/main/docs/development-highlights.ja.md)  
  - [**公開Demo**](https://analytics-metrics-api.onrender.com/)

- **[ai-tool-access-requests](https://github.com/ShikiIchitose/ai-tool-access-requests)**  
  Django + PostgreSQL で実装した、AIツール利用申請・承認のための内部業務アプリです。認証・認可、申請者／レビュワー／admin の権限分離、フォームバリデーション、業務ルールを踏まえたレビュー導線、自動テストを通じて、実務寄りのバックエンド基礎力を示す小規模ポートフォリオです。  
  - **権限設計**：requester / reviewer / admin の責務を分離し、review workflow を通常UI側で実行する構成
  - **業務ルール**：self-review prohibition、pending-only review、inactive tool handling などを明示的に扱う設計
  - **運用と品質**：inspection-only admin と CI により、最小構成でも保守しやすい内部業務アプリとして整理
  - [**開発サマリ**](https://github.com/ShikiIchitose/ai-tool-access-requests/blob/main/docs/development-highlights.ja.md)
  - [**公開Demo**](https://ai-tool-access-requests.onrender.com/)

## 背景
航空宇宙工学修士。FORTRAN中心の数値計算（C / Python併用）と、HPC（high-performance computing: 高性能計算）環境での計算実行経験があります。  
機械学習／AIは将来の方向性として関心がありますが、現時点では **分析・データ基盤の基礎**（品質、再現性、パイプライン化）を優先しています。

</details>
