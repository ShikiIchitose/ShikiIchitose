# Common Principles of dbt Modeling

## 1. Purpose and Positioning

This document explains dbt modeling perspectives for turning raw operational data into tested, documented, and reusable analytical assets.

It is based on portfolio implementation experience and public information, not on access to any company's internal data, production systems, or existing dbt project. The purpose is to show how business questions can be connected to source data, model grain, facts, dimensions, marts, tests, documentation, and BI consumption in an analytics engineering context.

The portfolio projects are scoped implementations, not complete production architectures. However, they were built with production-oriented data engineering practices in mind, including explicit model grain, dbt tests, documentation, lineage, reproducibility, and business-facing analytical outputs.

The examples below include both implemented portfolio patterns and illustrative production-oriented patterns. Where the distinction matters, this document separates implemented portfolio work from production-oriented examples.

## 2. What dbt Modeling Contributes

I see dbt modeling as a practice for converting raw operational data into decision-ready analytical models.

In many organizations, important business data is spread across application databases, spreadsheets, event logs, billing systems, CRM tools, advertising platforms, and other operational systems. Without a modeling layer, analysts and business stakeholders may repeatedly redefine the same metrics, duplicate transformation logic, or build dashboards whose assumptions are difficult to trace.

dbt modeling can contribute by creating a governed transformation layer that makes business data more consistent, reusable, testable, and understandable.

The main contribution areas are:

| Area | Contribution |
|---|---|
| Metric consistency | Centralize important definitions so teams do not calculate the same KPI differently |
| Reusable transformations | Move repeated cleanup and business logic out of ad hoc SQL and BI tools |
| Data quality | Validate structural and semantic assumptions through dbt tests |
| Lineage | Make upstream and downstream dependencies visible |
| Documentation | Help downstream users understand model grain, columns, assumptions, and intended use |
| BI reliability | Provide stable models for dashboards, self-service BI, and reporting |
| Decision support | Expose analytical outputs that support operational and executive decisions |

The goal is not just to build tables. The goal is to make data easier to trust, explain, reuse, and act on.

## 3. Modeling Principles

My dbt modeling approach is based on the following principles.

### 3.1 Start from business questions

I do not start from table names alone. I first try to understand the business question that the data model should support.

Illustrative business questions across domains:

- Which products, customers, teams, tools, or channels are driving performance?
- Which operational states require follow-up?
- Which metrics need to be trusted by multiple teams?
- Which dashboard decisions depend on this model?

A good dbt model should make a business process easier to analyze, not simply mirror the shape of an upstream source table.

### 3.2 Identify the business process

Before designing facts and dimensions, I identify the business process being modeled.

Common business processes I would look for in a production-oriented modeling study:

- order process
- inventory snapshot process
- pricing process
- subscription lifecycle
- approval workflow
- product usage process
- warehouse movement process

This step matters because a fact table should represent a process or measurable event, not just a convenient join of raw sources.

### 3.3 Define explicit model grain

Every model should have a clear grain.

The grain defines what one row represents. Without an explicit grain, metrics can be misinterpreted or double-counted.

Common grain examples:

| Model type | Example grain |
|---|---|
| Order fact | One row per order line |
| Usage fact | One row per user, product, and day |
| Inventory snapshot | One row per date, warehouse, product, and condition |
| Monthly reporting mart | One row per reporting month, team, and product |

Metrics should be interpreted only at the grain exposed by the model. If a business question requires a different grain, the warehouse should expose another model at that grain rather than forcing the BI or reporting layer to infer unavailable detail.

### 3.4 Separate source cleanup from business semantics

I prefer to separate low-level source cleanup from business meaning.

A typical production-oriented separation is:

```text
sources
  -> staging
  -> intermediate
  -> marts / dimensional models
  -> BI or semantic layer
```

Staging models should preserve the raw grain while standardizing names, types, timestamp handling, and lightweight source-level cleanup.

Business rules, joins, re-graining, and metric logic should be introduced in later layers where the intent is easier to document and test.

The exact layer names can vary by project. In my portfolio, I used a separate `core` layer for reusable facts and dimensions; in a production-oriented structure, those models may instead live directly under `marts` as dimensional models.

### 3.5 Model reusable facts and dimensions

Dimensional modeling, especially the Kimball-style separation of facts and dimensions, remains one of the most established foundations for analytical data modeling. Modern data teams may also use or discuss other approaches such as Data Vault, Third Normal Form, One Big Table, wide denormalized marts, or semantic-layer-first designs. However, the core idea of representing measurable business processes as facts and descriptive business context as dimensions remains highly useful for building reusable, BI-ready analytical models. Accordingly, this material adopts the fact-and-dimension distinction as a foundational modeling principle.

For production-oriented modeling, I would generally expose reusable dimensional models such as:

```text
dim_*
fct_*
```

Dimension models should represent relatively stable business entities and descriptive context, while fact models should represent measurable business processes, events, transactions, or snapshots.

Examples of reusable dimensional models I would consider, depending on the domain:

```text
dim_customer
dim_product
dim_device_model
dim_sales_channel
fct_order
fct_inventory_snapshot_daily
fct_usage_daily
fct_price_change
```

These facts and dimensions can then be consumed by BI tools, semantic layers, downstream marts, notebooks, or ad hoc SQL analysis.

### 3.6 Distinguish stock metrics from flow metrics

Stock and flow metrics should not be mixed casually.

Flow metrics describe activity during a period.

Common flow metric examples:

```text
orders_total
sales_amount
sessions_total
units_sold
monthly_spend
```

Stock metrics describe state at a point in time.

Common stock metric examples:

```text
inventory_on_hand
open_requests
approved_users_at_month_end
active_subscriptions_at_month_end
```

When stock and flow metrics are combined, the reporting grain and time semantics must be explicit. Otherwise, dashboards can produce plausible-looking but incorrect numbers.

### 3.7 Separate transformation failures from business review signals

Not every undesirable business condition should fail a pipeline.

> Business exceptions are outputs. Transformation inconsistencies are failures.

I distinguish between:

| Type | Meaning | Handling |
|---|---|---|
| Transformation failure | The data model is structurally broken or violates a required contract | Fail dbt tests |
| Business review signal | The data is valid but indicates a state that should be reviewed | Surface in marts or dashboards |

Illustrative business review signals across domains include:

- low adoption
- stale inventory
- usage without approval
- spend without usage
- high inventory aging
- margin below threshold
- valid operational states that require follow-up

For example, a missing required key should fail a test. However, a valid row showing low adoption, stale inventory, usage without approval, or spend without usage may be a business review signal rather than a transformation error.

### 3.8 Use tests to validate assumptions

dbt tests should validate assumptions that downstream users depend on.

Common test examples:

- primary key uniqueness
- non-null required fields
- accepted values
- relationship checks
- date range assumptions
- metric consistency checks
- reconciliation between source and transformed totals

Tests are not just quality gates. They are executable documentation of what the model assumes to be true.

### 3.9 Document assumptions, lineage, and downstream use

Model documentation should explain:

- what the model represents
- its grain
- important columns
- business assumptions
- known limitations
- downstream consumers
- interpretation cautions

Good documentation helps downstream consumers discover and understand curated datasets. dbt also provides project documentation that can be generated and rendered as a website.

### 3.10 Choose the BI exposure pattern based on tools and needs

The final exposure pattern should depend on the available BI and semantic-layer capabilities.

If a capable BI or semantic layer such as Looker or Tableau is available, tested facts and dimensions can be exposed through that layer for governed exploration and dashboarding.

If the BI layer is lightweight, or if heavy repeated aggregation, dashboard performance, urgent reporting, or operational review lists are required, dbt reporting marts can provide stable dashboard-ready outputs.

## 4. Production-Oriented Model Structure

In a production-oriented environment, I would generally treat Looker, Tableau, the dbt Semantic Layer, or a comparable BI / semantic layer as the primary consumption layer for reusable dimensional models.

A typical production-oriented structure would be:

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
  - LookML Explores
  - Tableau relationships / data model
  - governed measures
  - semantic metrics
  ↓
dashboards / self-service BI / stakeholder reporting
```

In this structure, dbt provides tested and documented facts and dimensions. The BI or semantic layer then defines user-facing relationships, measures, explores, dashboards, and self-service analysis experiences.

This pattern avoids pushing all dashboard-specific joins and calculations into ad hoc SQL while still allowing the BI layer to provide flexible exploration for business users.

Here, `marts / dimensional models` refers to the layer where reusable business-facing `dim_*` and `fct_*` models are exposed; reporting-oriented marts may be added separately where materialized aggregation or operational review logic provides clear value.

## 5. Role of dbt Reporting Marts

Even when a capable BI or semantic layer exists, dbt reporting marts can still be useful.

I would use dbt reporting marts where they provide clear value, such as:

- heavy or repeated aggregations
- complex stock-and-flow metrics
- dashboard performance optimization
- dashboard-ready extracts
- operational review candidate tables
- urgent lightweight reporting
- cross-tool reuse across BI, SQL, notebooks, and scripts
- governance of important business logic outside a single BI tool

Illustrative production-oriented examples include:

- `inventory_health_daily`
- `sales_velocity_monthly`
- `pricing_performance_daily`
- `margin_by_model_grade_channel`
- `operations_review_candidates`

For example, a BI tool may be a good place to define user-facing exploration over `fct_sales_order` and `dim_product`. However, a recurring operational surface such as `inventory_health_daily`, `sales_velocity_monthly`, or `operations_review_candidates` may be better modeled directly in dbt if it requires complex logic, stable grain, repeated use, or clear test coverage.

In this sense, dbt reporting marts are not merely a fallback for lightweight BI tools. They are a controlled way to materialize business logic when the logic needs to be reusable, testable, performant, reviewable, traceable, or shared across multiple downstream consumers.

## 6. Portfolio Connection

My portfolio projects are scoped implementations, but they are designed to demonstrate transferable analytics engineering practices.

In `access-governance-warehouse`, I implemented a layered dbt warehouse using deterministic synthetic raw data. The project separates source cleanup, reusable warehouse semantics, intermediate re-graining logic, and business-facing analytical outputs.

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

The implemented responsibility split is:

| Layer | Role |
|---|---|
| Sources | Define raw input contracts |
| Staging | Standardize raw fields while preserving raw grain |
| Core | Define reusable dimensions and facts |
| Intermediate | Isolate re-graining, stock logic, and reusable mart support logic |
| Marts | Provide business-facing analytical outputs |

This differs slightly from production-oriented structures where `dim_*` and `fct_*` models may be placed directly under `marts` and consumed through Looker, Tableau, or a semantic layer.

In my portfolio, I intentionally used `core` for reusable facts and dimensions, while reserving `marts` for reporting-oriented, stakeholder-facing outputs. This fit the portfolio scope because Looker Studio was used as a lightweight visualization layer and reusable business logic was kept in dbt rather than in BI-layer custom logic.

Implemented portfolio examples include:

```text
dim_tool
dim_user
fct_access_request
fct_tool_usage_daily
fct_tool_spend_monthly
access_requests_monthly
tool_adoption_monthly
governance_exceptions_current
adoption_review_candidates_monthly
```

The transferable point is not the exact folder structure. The transferable point is the modeling discipline:

- define explicit grain
- preserve source contracts
- separate cleanup from business logic
- build reusable facts and dimensions
- isolate re-graining logic
- expose business-facing analytical outputs
- test structural assumptions
- document limitations and interpretation rules

## 7. Summary

My dbt modeling approach is based on the idea that analytics engineering is not only about transforming tables, but about making business data trustworthy, reusable, documented, and decision-ready.

The core sequence is:

```text
business question
  → business process
  → model grain
  → facts and dimensions
  → tests and documentation
  → BI / semantic layer or reporting marts
  → business decision support
```

The portfolio demonstrates foundational modeling principles in a deliberately scoped environment. The accompanying case study builds on that evidence, not as a structure to copy as-is, but as a foundation for developing production-oriented modeling hypotheses around a reusable business domain, source-system considerations, and stakeholder questions.

## References and Public Sources

This document is based on portfolio implementation experience and public information. The following sources were used as references for dbt modeling principles, dimensional modeling, analytics engineering practices, documentation, and semantic-layer thinking.

- dbt Labs. “What is dbt?” dbt Developer Hub, version 1.11.  
  https://docs.getdbt.com/docs/introduction?version=1.11  
  Used as a primary reference for dbt's role in analytics engineering, transformation workflows, testing, documentation, and software engineering practices for analytics code.

- dbt Labs. “Best Practice Guides.” dbt Developer Hub, version 1.11.  
  https://docs.getdbt.com/best-practices?version=1.11  
  Used as a primary reference for recommended dbt project structure, modeling patterns, testing practices, and maintainable analytics engineering workflows.

- dbt Labs. “Building a Kimball dimensional model with dbt.” dbt Developer Blog, April 20, 2023.  
  https://docs.getdbt.com/blog/kimball-dimensional-model  
  Used as a reference for positioning dimensional modeling as one of several modern data modeling techniques, while still widely adopted for analytics, and for explaining how Kimball-style facts, dimensions, business processes, grain, documentation, and consumption can be implemented in dbt.

- Kimball Group. “Dimensional Modeling Techniques.” Kimball Group.  
  https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/  
  Used as a foundational reference for Kimball-style dimensional modeling concepts, including business processes, grain, dimensions, facts, star schemas, conformed dimensions, and slowly changing dimensions.
