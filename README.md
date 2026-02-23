# Python | Data & Analytics Portfolio

I’m looking for Python roles, currently prioritizing **data analytics / data engineering**.

**Proof points:** Tested • CI-gated • Reproducible outputs

## Current focus (near-term)
- Production-minded Python: **CLI design, testing, CI, reproducible runs**
- End-to-end analytics workflows: **fetch → validate → preprocess → analyze → report & plots**

## Featured projects
- **[url-monitor](https://github.com/ShikiIchitose/url-monitor)** — Python CLI to check URL availability/latency and generate a **Markdown report + JSON results**
  - Links: [README](https://github.com/ShikiIchitose/url-monitor#readme) · [CI](https://github.com/ShikiIchitose/url-monitor/actions) · [Releases](https://github.com/ShikiIchitose/url-monitor/releases)
  - Highlights:
    - Reproducible runs and clear, reviewable outputs
    - CI quality gates (**ruff / pytest**)
    - Responsible network-use guidance (portfolio-friendly defaults)

- **[Exoplanet catalog analysis](https://github.com/ShikiIchitose/exoplanet-analysis-report)** — Reproducible analytics pipeline using **NASA Exoplanet Archive (TAP)** data  
  *(fetch → validate → preprocess → analyze → report & plots)* with **DuckDB** as the local warehouse
    - Highlights:
    - Reproducibility & auditability: seeded bootstrap + snapshot/schema hashes + locked deps captured per run in [`run.json`](https://github.com/ShikiIchitose/exoplanet-analysis-report/blob/main/sample-artifacts/run.json)
    - Output artifacts are generated deterministically (reports/plots under a fixed directory)
    - Designed for real-world data issues: schema drift, missing values, outliers, automated reporting
    - Reusability: domain-agnostic pipeline scaffolding (fetch→validate→preprocess→analyze→report & plots) with DuckDB as the local warehouse

## Longer-term direction
- I’m interested in **ML / AI** in the longer term, but I’m currently prioritizing analytics and data engineering fundamentals.

## Background
- M.S. in Aerospace Engineering
- Scientific/numerical computing with **FORTRAN** (plus C/Python), including runs on **HPC (high-performance computing)** systems

## Contact
Please use the links on my GitHub profile.

---

### Notes
This profile emphasizes engineering practices and reproducible deliverables over domain-specific research claims.

<details>
<summary><strong>日本語（概要）</strong></summary>

現在、Pythonエンジニア職を中心に仕事を探しています。直近は **データ分析／データエンジニアリング** を主軸に、
**再現性（reproducibility）** と **データ品質（data quality）** を重視したパイプラインを実装し、成果物として提示しています。

（要点）テスト済み／CI（continuous integration: 継続的インテグレーション）で品質ゲート／再現可能な成果物

## 現時点の注力
- **実装衛生（engineering hygiene）**：CLI設計、テスト（pytest）、静的解析（ruff）、CIによる品質管理
- **分析パイプライン（analytics pipeline）**：取得（fetch）→検証（validate）→前処理（preprocess）→分析（analyze）→レポート／図表生成（report & plots）
- **実行の監査可能性（auditability）**：実行条件・入力・依存関係をメタデータとして残し、後から再現／検証できる設計

## 代表プロジェクト
- **[url-monitor](https://github.com/ShikiIchitose/url-monitor)**  
  URLの疎通／遅延を計測し、MarkdownレポートとJSON結果を生成するPython CLI。テスト／CI／再現可能な実行を重視し、ネットワーク利用に配慮したデフォルト動作を明記しています。
- **[Exoplanet catalog analysis](https://github.com/ShikiIchitose/exoplanet-analysis-report)**  
  NASA公開の系外惑星カタログ（TAP: Table Access Protocol）を用いた end-to-end 分析パイプライン。DuckDBをローカルのデータウェアハウスとして利用しています。  
  - **再現性（reproducibility）**：ブートストラップ（bootstrap）のseed、スナップショット／スキーマのハッシュ、依存関係ロック等を `run.json` に記録（例：[`sample-artifacts/run.json`](https://github.com/ShikiIchitose/exoplanet-analysis-report/blob/main/sample-artifacts/run.json)）
  - **再利用性（reusability）**：ドメイン非依存のパイプライン骨格＋監査可能な実行メタデータ＋DuckDBローカル集計という構成

## 背景
航空宇宙工学修士。FORTRAN中心の数値計算（C/Python併用）と、HPC（high-performance computing: 高性能計算）環境での計算実行経験があります。  
機械学習／AIは将来の方向性として関心がありますが、現時点では **分析・データ基盤の基礎**（品質、再現性、パイプライン化）を優先しています。

</details>
