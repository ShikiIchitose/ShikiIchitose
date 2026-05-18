# Analytics Engineering Modeling and Operating Approach

> This document is an overview version of my analytics engineering modeling materials.  
> A more detailed version is also available for readers who would like to review the full material set:
> [Analytics Engineering Modeling Perspectives](https://github.com/ShikiIchitose/ShikiIchitose/blob/main/docs/analytics-engineering-modeling/00_analytics_engineering_modeling_index.md)

## 1. Positioning and Core Approach

This document summarizes my approach to analytics engineering work, especially dbt modeling, model grain, tests, documentation, BI / semantic layer consumption, stakeholder alignment, and decision-support outputs.

It is based on portfolio implementation experience and public information. It does not assume access to any company's internal data, production systems, or existing dbt project. The purpose is not to propose a complete production architecture for a specific company. The purpose is to show a structured way to connect business questions to analytical models and decision-support outputs.

My portfolio projects are scoped implementations, not production systems. However, they were built with production-oriented data engineering practices in mind, including explicit model grain, dbt tests, documentation, lineage, reproducibility, and business-facing analytical outputs.

The core sequence is:

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

Analytics engineering is not only about transforming tables. It is about making business data easier to trust, explain, reuse, and act on.

---

## 2. Modeling Principles

The following principles summarize the dbt modeling approach used in this document.

| Principle | Application |
|---|---|
| Start from business questions | Clarify what decision, metric, dashboard, or review workflow the model should support before starting from table names. |
| Identify business processes | Model measurable events, transactions, or states such as orders, subscriptions, product usage, approvals, billing, inventory snapshots, pricing changes, or support interactions. |
| Define explicit model grain | Make the one-row meaning clear so that metrics are not double-counted or aggregated at an unsafe level. |
| Separate cleanup from semantics | Keep staging models close to raw source grain; introduce business rules, joins, re-graining, and metric logic in later layers where intent can be documented and tested. |
| Model reusable facts and dimensions | Use fact models for measurable business processes and dimension models for relatively stable entities and descriptive context. |
| Separate stock and flow metrics | Distinguish period activity such as orders, sessions, revenue, or spend from point-in-time state such as inventory on hand, open requests, active subscriptions, or approved users at month end. |
| Separate failures from review signals | Fail dbt tests for broken structural or semantic contracts; surface valid but undesirable business states as marts or dashboard review signals. |
| Test and document assumptions | Treat dbt tests and model documentation as part of the model contract, not as afterthoughts. |

Dimensional modeling, especially the Kimball-style separation of facts and dimensions, remains one of the most established foundations for analytical data modeling. Modern data teams may also use or discuss approaches such as Data Vault, Third Normal Form, One Big Table, wide denormalized marts, or semantic-layer-first designs. However, the core idea of representing measurable business processes as facts and descriptive business context as dimensions remains useful for building reusable, BI-ready analytical models. Accordingly, this document adopts the fact-and-dimension distinction as a foundational modeling principle.

A typical layer separation is:

```text
sources
  -> staging
  -> intermediate
  -> marts / dimensional models
  -> BI or semantic layer
```

The exact folder names can vary by project. What matters is that each layer has a clear responsibility, each exposed model has a documented grain, and downstream users can understand what the model represents and how it should be interpreted.

A particularly important distinction is the difference between transformation failures and business review signals.

> Business exceptions are outputs. Transformation inconsistencies are failures.

A missing required key, duplicate primary key, invalid accepted value, or unresolved relationship should fail a test. In contrast, a valid row showing low adoption, stale inventory, usage without approval, spend without usage, or margin below threshold may be a business review signal. It should be surfaced for review, not treated as a broken pipeline by default.

---

## 3. Production-Oriented Operating Approach

In a production-oriented environment, I would generally treat Looker, Tableau, the dbt Semantic Layer, or a comparable BI / semantic layer as the primary consumption layer for reusable dimensional models.

A typical pattern is:

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

In this structure, dbt provides tested and documented facts and dimensions. The BI or semantic layer defines user-facing relationships, measures, dashboards, explores, and self-service analysis experiences. This avoids pushing all dashboard-specific joins and calculations into ad hoc SQL while still allowing business users to explore governed data.

At the same time, dbt reporting marts can still add value where logic should be materialized, reviewed, tested, reused, or optimized. Suitable cases include heavy or repeated aggregations, complex stock-and-flow metrics, dashboard performance optimization, dashboard-ready extracts, operational review candidate tables, urgent lightweight reporting, and cross-tool reuse across BI, SQL, notebooks, and scripts.

For example, a BI tool may be a good place to define user-facing exploration over `fct_sales_order` and `dim_product`. However, a recurring operational surface such as `inventory_health_daily`, `sales_velocity_monthly`, or `operations_review_candidates` may be better modeled directly in dbt if it requires complex logic, stable grain, repeated use, or clear test coverage.

### 3.1 Analytics Engineering Responsibility and Stakeholder Ownership

Analytics engineering can provide trusted decision-support models, but it should not replace business ownership of pricing, support, customer success, risk, finance, product, or operational decisions.

Analytics engineering should own or strongly contribute to:

- translating stakeholder questions into modelable business concepts;
- clarifying metric definitions and model grain;
- identifying required source data and source contracts;
- designing reusable facts, dimensions, intermediate models, and marts;
- implementing dbt tests for structural and semantic assumptions;
- documenting assumptions, limitations, and interpretation cautions;
- exposing stable models to BI tools, semantic layers, dashboards, reports, or review workflows;
- maintaining reproducibility, lineage, and traceability of transformation logic.

Business stakeholders should own decisions such as pricing changes, campaign eligibility, support policy changes, customer success prioritization, risk handling, finance review actions, and product strategy decisions.

The role of analytics engineering is to make business conditions measurable, explainable, reusable, and discussable. It should support decision-making, not silently replace it.

For this reason, active communication with stakeholders is important, especially around metric definitions, model grain, the meaning of review signals, and the intended use of analytical outputs.

---

## 4. Case Study: Customer Lifecycle and Monetization Analytics

Customer lifecycle and monetization analytics is a useful reusable case study because it appears across many business models. Different industries use different terms, but many businesses need to understand how customers, accounts, users, buyers, sellers, or organizations move from acquisition to activation, usage, monetization, retention, and follow-up.

A central stakeholder question for this domain could be:

> Which customer segments, accounts, products, plans, or channels are driving activation, usage, revenue, and retention, and where do lifecycle states require follow-up?

The first modeling step is to identify the relevant business processes. In this domain, likely processes include signup / registration, onboarding / activation, product usage, transactions or orders, subscription lifecycle, billing or invoicing, support interactions, and lifecycle review signals.

The source data would depend on the company, but likely categories include customer or account master data, user or member data, acquisition data, product or service catalog data, signup and onboarding events, usage or behavioral events, orders or transactions, subscription or contract data, billing or revenue data, and support or operations data.

A reusable model structure may include dimensions such as `dim_customer`, `dim_account`, `dim_user`, `dim_product`, `dim_plan`, `dim_channel`, and `dim_date`; facts such as `fct_signup`, `fct_activation_event`, `fct_usage_daily`, `fct_transaction`, `fct_order`, `fct_subscription_snapshot_monthly`, `fct_invoice`, `fct_payment`, and `fct_support_case`; and intermediate models for monthly usage aggregation, customer revenue aggregation, month-end account status, recent activity classification, or lifecycle review signal preparation.

The exact model names are less important than the modeling discipline: each exposed fact, dimension, or mart should have a clear responsibility, a documented grain, and testable assumptions.

For example, `fct_usage_daily` may expose one row per customer, account, or user, product, and day. A monthly lifecycle mart may expose one row per reporting month, segment, and lifecycle state. A current account health surface may expose one row per current account. These models answer different questions and should not be casually mixed without understanding grain.

This domain also combines flow and stock metrics. Flow metrics may include signups, activations, usage, revenue, or support cases during a period. Stock metrics may include active accounts, active subscriptions, unresolved support cases, dormant accounts, or at-risk accounts as of month end. If these metrics are combined in one mart, the documentation should explain which metrics are period flows and which metrics are point-in-time stocks.

The domain also includes valid business review signals, such as signup without activation, activated account with no recent usage, high usage with low monetization, paying account with no recent usage, active usage without expected billing, declining usage before renewal, or dormant customer with prior high value. These signals should be modeled as decision-support outputs. They do not necessarily indicate data quality failures, and they should not automatically trigger business actions without stakeholder alignment.

A production-oriented exposure pattern for this domain could place tested facts and dimensions in dbt, then expose governed relationships and measures through a BI / semantic layer. Examples of governed measures may include activated customers, active accounts, monthly active users, revenue amount, churned accounts, retention rate, and average revenue per account. Revenue-related measures should be explicit about whether they represent billed revenue, collected payment amount, recognized revenue, transaction fees, gross revenue, net revenue, gross margin, or another business-specific monetization concept.

Reporting marts may still be useful for stable, repeated, or review-oriented logic, such as `customer_lifecycle_monthly`, `activation_funnel_weekly`, `usage_engagement_monthly`, `revenue_retention_monthly`, `account_health_current`, `churn_risk_candidates`, `monetization_review_candidates`, or `operational_review_candidates`. The goal of these marts is not to replace the BI or semantic layer. The goal is to materialize stable, reusable, testable, and traceable business logic where doing so creates clear value.

---

## 5. Portfolio Evidence

The main portfolio evidence behind this approach is `access-governance-warehouse`, a dbt + DuckDB + BigQuery analytics engineering portfolio project for enterprise AI tool access governance.

The project demonstrates deterministic synthetic raw data, dbt source contracts and tests, explicit model grain, staging / core / intermediate / marts layers, reusable facts and dimensions, re-graining and stock logic isolated in intermediate models, reporting-oriented marts, separation between transformation failures and business review signals, dbt documentation and lineage, Looker Studio dashboard artifacts, static report outputs, a reproducible local execution path, and an optional cloud execution path using BigQuery.

The implemented portfolio structure is:

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

This structure is closer to an ELT-oriented analytics engineering workflow than to a full production ETL ingestion platform. It focuses on warehouse-side dbt modeling, testing, documentation, lineage, and BI-ready outputs after raw data has been loaded into an analytical environment. Production-scale pre-load transformation, streaming ingestion, and Dataflow / Apache Beam-style ETL processing are outside the current portfolio scope, but I recognize them as important adjacent data engineering areas to learn and contribute to where required.

This differs from some production-oriented structures where reusable `dim_*` and `fct_*` models may live directly under `marts` and be consumed through Looker, Tableau, or a semantic layer. In the portfolio, `core` is used for reusable facts and dimensions while `marts` are reserved for reporting-oriented, stakeholder-facing outputs. This fits the portfolio scope because Looker Studio is used as a lightweight visualization layer and reusable business logic is kept in dbt rather than in BI-layer custom logic.

The transferable point is not the exact folder structure. The transferable point is the modeling discipline: define explicit grain, preserve source contracts, separate cleanup from business logic, build reusable facts and dimensions, isolate re-graining logic, expose business-facing analytical outputs, test structural assumptions, and document limitations and interpretation rules.

## 6. Initial 30 / 60 / 90-Day Contribution Approach

In the first 30 days after joining an analytics or data engineering team, I would focus on understanding the existing business processes, source systems, dbt project structure, model grain, metric definitions, BI usage, team-specific tools and workflows, and current pain points.

By around 60 days, I would look for opportunities to contribute through small, well-scoped improvements such as documentation updates, dbt test additions, model reviews, lineage checks, or support for existing reporting marts and dashboards.

By around 90 days, I would aim to support reusable and decision-ready analytical models by applying principles practiced in my portfolio: explicit grain, source contracts, tested assumptions, documented limitations, and clear separation between transformation failures and business review signals.

The goal is not to redesign an existing data platform immediately. The goal is to become familiar with the team's analytics engineering work, understand the current environment, ask the right questions, contribute safely to existing workflows, and gradually support more reliable and reusable analytics engineering practices.

---

## References and Public Sources

This document is based on portfolio implementation experience and public information. The following sources were used as references for dbt modeling principles, dimensional modeling, analytics engineering practices, documentation, semantic-layer thinking, and public examples of data team workflows, ETL / ELT workflow boundaries, data pipeline design.

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
