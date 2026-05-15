# Target-Domain Case Study: Customer Lifecycle and Monetization Analytics

## 1. Purpose of This Case Study

This case study applies the dbt modeling principles from the common part to a reusable business domain: customer lifecycle and monetization analytics.

The purpose is not to propose a complete production architecture for a specific company. Instead, this document presents a modeling perspective for connecting business questions to source data, business processes, model grain, facts, dimensions, BI / semantic layer exposure, dbt reporting marts, tests, and decision-support outputs.

This case study is intended to be generic enough to apply across multiple business domains, while still being concrete enough to show practical analytics engineering thinking.

The domain can be interpreted differently depending on the company:

- In SaaS, it may mean account activation, product usage, subscription revenue, expansion, and churn.
- In Fintech, it may mean onboarding, account activity, transaction volume, monetization, retention, and operational or risk review.
- In e-commerce or marketplace businesses, it may mean acquisition, first purchase, repeat purchase, order value, seller activity, GMV, and customer retention.
- In B2B services, it may mean account onboarding, contract activity, product engagement, renewal, support load, and account health.

The common modeling idea is that many businesses need to understand how customers or accounts move from acquisition to activation, usage, monetization, retention, and follow-up.

---

## 2. Why This Domain Is Useful as a Reusable Modeling Archetype

Customer lifecycle and monetization analytics is useful as a reusable modeling archetype because it appears in many business models.

Different industries use different terms, but the underlying analytical structure is often similar.

A business usually wants to understand:

- who the customer, account, buyer, seller, user, or organization is;
- how that entity was acquired or onboarded;
- whether it reached an activation milestone;
- whether it continues to use the product, platform, account, or service;
- how usage or transactions connect to revenue;
- which lifecycle states require follow-up;
- which metrics should be trusted across teams.

This makes the domain suitable for demonstrating dbt modeling because it naturally involves:

- entity dimensions;
- event and transaction facts;
- snapshot facts;
- stock and flow metrics;
- cohort and lifecycle analysis;
- operational review signals;
- BI / semantic layer consumption;
- reporting marts where stable grain and repeated logic matter.

The value of dbt modeling in this domain is not just to build tables. It is to create tested, documented, reusable analytical models that help business and analytics teams reason consistently about customer behavior, engagement, revenue, and follow-up priorities.

---

## 3. Plausible Stakeholder Questions

A central stakeholder question for this domain could be:

> Which customer segments, accounts, products, plans, or channels are driving activation, usage, revenue, and retention, and where do lifecycle states require follow-up?

This question can be decomposed into more specific business questions.

### 3.1 Acquisition and onboarding

Illustrative questions:

- Which acquisition channels or customer segments produce activated customers or accounts?
- How long does it take from signup to activation?
- Which onboarding steps have the largest drop-off?
- Are there customers or accounts that signed up but never reached a meaningful activation event?

### 3.2 Usage and engagement

Illustrative questions:

- Which customers, accounts, products, or plans are driving active usage?
- Which features or product areas are associated with continued engagement?
- Which accounts have access but low or no activity?
- Are usage metrics increasing, decreasing, or concentrated in a few segments?

### 3.3 Monetization

Illustrative questions:

- Which customers, accounts, plans, products, or channels generate revenue?
- How does usage relate to subscription revenue, transaction revenue, fees, or order value?
- Are there high-cost or high-touch accounts with low monetization?
- Are there monetization gaps, such as active usage without billing or paid access without usage?

### 3.4 Retention and lifecycle follow-up

Illustrative questions:

- Which customers or accounts are retained, inactive, dormant, or at risk of churn?
- Which lifecycle states require sales, customer success, operations, risk, or finance follow-up?
- Which segments have strong activation but weak retention?
- Which accounts show declining usage before renewal or repeat purchase?

### 3.5 Executive and operational reporting

Illustrative questions:

- Which KPIs should be trusted across teams?
- Which dashboards need stable, governed definitions?
- Which operational review lists should be refreshed regularly?
- Which assumptions should be tested before metrics are used for decision-making?

---

## 4. Business Processes to Model

Before designing facts and dimensions, I would identify the business processes that need to be modeled.

In this domain, common business processes may include:

| Business process | Description | Possible model(s) |
|---|---|---|
| Signup / registration | A customer, account, user, buyer, or seller enters the system | `fct_signup` |
| Onboarding / activation | The entity reaches a meaningful first-value milestone | `fct_activation_event` |
| Product usage | The entity uses a product, feature, account, or service | `fct_usage_event` / `fct_usage_daily` |
| Transaction / order | The entity performs a transaction, order, transfer, or purchase | `fct_transaction` / `fct_order` |
| Subscription lifecycle | A subscription starts, renews, expands, contracts, churns, or remains active over time | `fct_subscription_event` / `fct_subscription_snapshot_monthly` |
| Billing / invoicing | Tracks billing, invoicing, payment collection, refunds, and revenue recognition | `fct_invoice` / `fct_payment` |
| Support / service interaction | A customer, user, or account contacts support or requires operational handling | `fct_support_case` |
| Lifecycle review signal | A customer, account, or segment is classified into a follow-up state | `fct_lifecycle_review_signal` or `lifecycle_review_candidates` mart |

The exact processes would depend on the company and source systems. The important modeling step is to separate business processes rather than joining all raw tables into one broad analytical table.

A fact table should represent a measurable event, transaction, or state in a business process. It should not simply be a convenient join of available raw sources.

---

## 5. Source Data Categories

A production-oriented implementation would start by understanding the available source systems, the teams that own them, their grain, update patterns, and data quality.

Illustrative source categories include:

| Source category | Example source data | Main modeling role |
|---|---|---|
| Customer / account master data | customer records, account records, organization records | define stable entity dimensions |
| User or member data | users, seats, team members, buyer/seller profiles | support user-level or role-level analysis |
| Acquisition data | campaigns, channels, referrals, landing sources | analyze acquisition quality |
| Product or service catalog | products, plans, features, SKUs, services | define what is being used or purchased |
| Signup / onboarding events | registration, verification, first login, first key action | model activation funnel |
| Usage or behavioral events | sessions, feature usage, API calls, page views, product events | model engagement and activity |
| Orders / transactions | purchases, transfers, trades, orders, transaction events | model business activity and transaction behavior |
| Subscription / contract data | plans, renewals, cancellations, expansions, contract terms | model recurring revenue lifecycle |
| Billing / revenue data | invoices, collected payments, fees, refunds, credits | model monetization and reconciliation |
| Support / operations data | tickets, cases, manual reviews, interventions | model follow-up and operational load |

The source layer should preserve the raw source grain and expose source contracts where appropriate. Staging models should standardize names, types, timestamps, and lightweight source-level helper fields while avoiding heavy business logic.

---

## 6. Reusable Facts and Dimensions

A reusable model structure would typically include dimensions for stable business entities and fact models for measurable business processes.

The following models are illustrative candidates, not prescriptive implementation requirements. Actual model design would depend on the company's source systems, business processes, BI / semantic layer, and stakeholder priorities.

The layer names used here are intentionally generic. In a typical production-oriented dbt structure, reusable `dim_*` and `fct_*` models may live under `marts / dimensional models`, with intermediate models preparing the logic those models reference.

In my portfolio, I used a separate `core` layer for reusable facts and dimensions, and used intermediate models downstream of `core` to isolate re-graining, stock logic, and mart support logic. The important point is not the folder name itself, but that each layer has a clear responsibility and that the dependency direction remains understandable.

### 6.1 Dimension model candidates

| Model | Example grain | Purpose |
|---|---|---|
| `dim_customer` | One row per customer | customer attributes and segmentation |
| `dim_account` | One row per account or organization | account-level analysis for B2B or multi-user products |
| `dim_user` | One row per user or member | user-level activity and role analysis |
| `dim_product` | One row per product or service | product-level performance analysis |
| `dim_plan` | One row per plan or pricing tier | subscription and monetization analysis |
| `dim_channel` | One row per acquisition or sales channel | acquisition and channel performance |
| `dim_date` | One row per calendar date | consistent time-based reporting |

In some domains, `customer`, `account`, `organization`, `buyer`, and `seller` may be separate entities; in others, some of these concepts may be combined.

### 6.2 Fact model candidates

| Model | Example grain | Purpose |
|---|---|---|
| `fct_signup` | One row per signup event | acquisition and registration analysis |
| `fct_activation_event` | One row per activation event | activation and time-to-value analysis |
| `fct_usage_daily` | One row per customer, account, or user, product, and day | engagement and usage analysis |
| `fct_transaction` | One row per transaction | transaction volume, value, and behavior analysis |
| `fct_order` / `fct_order_line` | One row per order, or one row per order line | commerce-style purchase analysis |
| `fct_subscription_snapshot_monthly` | One row per account and month | recurring revenue lifecycle and churn analysis |
| `fct_invoice` | One row per invoice | billing, invoicing, and billed amount analysis |
| `fct_payment` | One row per payment | payment collection and payment status analysis |
| `fct_support_case` | One row per support or operations case | service load and customer follow-up analysis |

### 6.3 Reusable intermediate model candidates

Intermediate models should isolate logic that is useful but not necessarily end-user-facing.

Examples include:

| Model | Example grain | Purpose |
|---|---|---|
| `int_usage_aggregated_to_month` | One row per customer, account, or user, product, and month | prepare monthly usage metrics for downstream marts |
| `int_customer_revenue_monthly` | One row per customer or account and month | aggregate invoice, payment, or transaction revenue |
| `int_account_status_as_of_month_end` | One row per account and month | derive month-end lifecycle state |
| `int_recent_activity_30d` | One row per customer, account, or user | derive recent activity state |
| `int_lifecycle_review_signals` | One row per customer or account and signal | isolate follow-up logic before mart exposure |

Intermediate models are useful for re-graining, stock logic, lifecycle classification, and repeated mart support logic. They should make complex transformations easier to inspect and test.

In particular, intermediate models are a good place to isolate transformations such as aggregating event-level data to a daily grain, aggregating order-line data to an order grain, deriving month-end status, or preparing reusable inputs for multiple downstream marts.

### 6.4 Grain and re-graining policy

Unlike staging models, fact and dimension models do not always preserve the raw source grain. Instead, each exposed model should have a clearly defined business grain.

A dimension model should represent a stable business entity or entity version. For example, `dim_customer` may have one row per customer, while a history-aware customer dimension may have one row per customer version or effective-date range.

A fact model should represent a measurable business process, event, transaction, or snapshot. For example, `fct_transaction` may have one row per transaction, while `fct_usage_daily` may have one row per customer, account, or user, product, and day.

When a fact model requires re-graining, I would usually separate the re-graining logic into an intermediate model if the logic is complex, reused by multiple downstream models, or important for protecting metric interpretation.

Examples include:

| Source grain | Intermediate logic | Exposed model grain |
|---|---|---|
| One row per usage event | Aggregate events to the selected entity-product-day grain | `fct_usage_daily`: one row per customer, account, or user, product, and day |
| One row per order line | Aggregate order lines to order | `fct_order`: one row per order |
| One row per transaction | Aggregate transactions to account-month | `fct_account_activity_monthly`: one row per account and month |
| One row per support case | Aggregate support cases to account-month | `fct_account_support_monthly`: one row per account and month |
| Support activity plus plan and revenue context | Combine support load, plan tier, and revenue context | `account_support_monetization_monthly` mart: one row per account and month |

For simple transformations with a single downstream use, the re-graining logic may be implemented directly in the exposed fact model. However, when the logic is reused, complex, or important for interpretation, I would prefer an intermediate model so that the transformation remains visible, testable, and reusable.

The key point is not that every transformation requires an intermediate model. The key point is that each exposed fact, dimension, or mart should have a documented grain, and any grain-changing logic should be easy to inspect and test.

---

## 7. Grain Decisions

Grain is one of the most important modeling decisions in this domain.

Each model should clearly define what one row represents. Metrics should be interpreted only at the grain exposed by the model. When the same business area needs multiple analytical perspectives, I would expose separate models at separate grains rather than mixing those grains in one model.

### 7.1 Example grains

| Model | Grain | Interpretation |
|---|---|---|
| `dim_customer` | One row per customer | customer-level attributes and segmentation |
| `dim_account` | One row per account or organization | account-level attributes and B2B analysis |
| `fct_signup` | One row per signup event | registration and acquisition flow |
| `fct_activation_event` | One row per activation event | first-value or milestone achievement |
| `fct_usage_daily` | One row per customer, account, or user, product, and usage date | daily engagement at a defined entity-product grain |
| `fct_transaction` | One row per transaction | transaction-level activity and value |
| `fct_order` | One row per order | order-level purchase behavior |
| `fct_order_line` | One row per order line | item-level or SKU-level purchase behavior |
| `fct_subscription_snapshot_monthly` | One row per account and month | recurring lifecycle state as of a monthly period |
| `customer_lifecycle_monthly` | One row per reporting month, segment, and lifecycle state | aggregated lifecycle reporting |
| `account_health_current` | One row per current account | current account-level health review |
| `churn_risk_candidates_current` | One row per current account or customer requiring review | current review candidate surface |

### 7.2 Why grain matters

If a model mixes multiple grains, metrics can become difficult to interpret.

For example:

- `active_users_total` may be valid within a product-level grain but unsafe to sum across product rows if the same user can use multiple products.
- `monthly_revenue` may be valid at account-month grain but double-counted if joined to user-level usage without careful re-graining.
- `transactions_total` may be a flow metric for a month, while `active_accounts_at_month_end` may be a stock metric as of month end.

A good model should make these boundaries explicit. If a new business question requires a different grain, I would prefer to expose another documented model or mart at that grain rather than relying on BI-layer calculations to infer unavailable detail from aggregated rows.

---

## 8. Stock and Flow Metrics

Customer lifecycle and monetization analytics naturally combines stock and flow metrics. These should be modeled carefully.

### 8.1 Flow metrics

Flow metrics describe activity during a period.

Examples:

| Metric | Meaning |
|---|---|
| `signups_total` | number of signups during a period |
| `activations_total` | number of activations during a period |
| `sessions_total` | total sessions during a period |
| `transactions_total` | total transactions during a period |
| `orders_total` | total orders during a period |
| `revenue_amount` | revenue measured or recognized for a period |
| `payments_total` | number of payments recorded during a period |
| `payment_amount` | payment amount collected during a period |
| `support_cases_opened` | support cases opened during a period |

### 8.2 Stock metrics

Stock metrics describe state at a point in time.

Examples:

| Metric | Meaning |
|---|---|
| `active_accounts_at_month_end` | accounts active as of month end |
| `active_subscriptions_at_month_end` | subscriptions active as of month end |
| `open_support_cases_at_month_end` | unresolved support cases as of month end |
| `dormant_accounts_at_month_end` | accounts classified as dormant as of month end |
| `at_risk_accounts_at_month_end` | accounts classified as at risk as of month end |

### 8.3 Modeling implication

When stock and flow metrics are combined in one mart, the reporting grain and time semantics must be explicit.

For example, a monthly account lifecycle mart may include:

- signups during the month;
- activations during the month;
- usage during the month;
- revenue during the month;
- active accounts as of month end;
- dormant accounts as of month end.

These metrics are all useful, but they do not have the same time semantics. The mart documentation should explain which metrics are period flows and which metrics are point-in-time stocks.

Revenue-related metrics should also clarify whether they represent billed amounts, collected payments, recognized revenue, or another business-specific revenue definition.

---

## 9. Business Review Signals

Not every undesirable business condition should fail a pipeline.

In this domain, many unusual or undesirable states are valid analytical outputs. They should be surfaced as review signals rather than treated as transformation failures.

Illustrative business review signals include:

| Review signal | Possible interpretation |
|---|---|
| signup without activation | onboarding follow-up candidate |
| activated account with no recent usage | engagement or customer success follow-up candidate |
| high usage with low monetization | pricing, packaging, or billing review candidate |
| paying account with no recent usage | renewal or customer success review candidate |
| active usage without expected billing | finance or contract review candidate |
| high support load with low retention | customer experience review candidate |
| declining usage before renewal | churn risk review candidate |
| dormant customer with prior high value | reactivation candidate |

These signals should be modeled as decision-support outputs. They do not necessarily indicate data quality failures.

### 9.1 Review signals are not automatic business actions

A business review signal should not be treated as an automatic business action.

For example, a signal such as `dormant_customer_with_prior_high_value` does not automatically mean that a discount, campaign, or outreach action should be sent. It means that the customer may be worth reviewing as a reactivation candidate because they were historically valuable but are currently inactive.

Analytics engineering can provide the reviewed customer list, metric definitions, and relevant context. However, the actual campaign, offer, outreach strategy, exclusion rules, or prioritization logic should be decided by the appropriate business stakeholders.

This distinction matters because review signals are designed to make business conditions visible and discussable. They should support decision-making, not silently replace it.

### 9.2 Context fields and stakeholder alignment

Analytics engineering may identify that a simple review flag is not sufficient for action.

For example, `is_reactivation_candidate = true` may need additional context before it can be used responsibly by marketing, sales, customer success, or operations teams. Useful context may include:

- `contact_permission_status`;
- `unsubscribe_flag`;
- prior complaint history;
- historical margin;
- recent campaign exposure;
- recent support status;
- current account or customer status;
- last activity date;
- days since last activity.

These context fields can help stakeholders make a more appropriate decision. However, they should not silently become official campaign eligibility rules, exclusion rules, or prioritization rules.

For example, analytics engineering may propose that `contact_permission_status` and `unsubscribe_flag` should be included in a reactivation candidate mart because they help stakeholders understand whether outreach may be appropriate. However, whether those fields should exclude a customer from a campaign, affect prioritization, or define outreach eligibility should be aligned with the relevant business owners.

In this sense, analytics engineering can identify modeling limitations, propose useful context, expose supporting fields, and document the interpretation caveats. The final inclusion, exclusion, prioritization, and outreach rules should be decided with the appropriate business stakeholders before being incorporated into official review logic.

Transformation failures should be reserved for structural or contractual problems, such as:

- missing required keys;
- duplicate primary keys where uniqueness is required;
- invalid accepted values;
- unresolved relationships to required dimensions;
- impossible or inconsistent timestamps;
- broken reconciliation between source and transformed totals.

This separation keeps dbt tests focused on structural and semantic assumptions, while marts and dashboards surface valid business states that require review.

The next section generalizes this distinction into the broader responsibility boundary between analytics engineering and business stakeholders.

---

## 10. Analytics Engineering Responsibility and Stakeholder Alignment

Analytics engineering can provide trusted decision-support models, but it should not replace business ownership of pricing, support, customer success, risk, finance, or product decisions.

In this domain, a dbt mart may classify customers or accounts into review-oriented states such as:

- `high_touch_high_revenue`
- `high_touch_low_revenue`
- `low_touch_high_revenue`
- `low_touch_high_support_load`

These classifications are useful because they make customer lifecycle, support load, and monetization patterns easier to inspect. However, they should be treated as decision-support outputs, not automatic business decisions.

For example, a `high_touch_low_revenue` account may require discussion about pricing, packaging, support policy, customer success motion, or account strategy. The analytics model can surface the condition, define the metric logic, and make the pattern visible. The actual decision should remain with the relevant business owners, such as leadership, customer success, sales, support, finance, product, or risk teams.

### 10.1 What analytics engineering should own

In this context, analytics engineering should own or strongly contribute to:

- translating stakeholder questions into modelable business concepts;
- clarifying metric definitions and model grain;
- identifying required source data and source contracts;
- designing reusable facts, dimensions, intermediate models, and marts;
- implementing dbt tests for structural and semantic assumptions;
- documenting assumptions, limitations, and interpretation cautions;
- exposing stable models to BI tools, semantic layers, dashboards, reports, or review workflows;
- maintaining reproducibility, lineage, and traceability of transformation logic.

The goal is to make business conditions measurable, explainable, and reusable.

In a production environment, this responsibility may also be supported by a structured dbt change workflow. For example, one public example is GitLab's dbt change workflow, which separates dbt changes into planning, creation, and verification stages, and includes defining scope, defining tests, preparing environments, validating changes locally and remotely, reviewing downstream table and report impact, and confirming business logic alignment.

This kind of workflow reinforces that analytics engineering is not only about writing transformation SQL. It also involves making model changes testable, reviewable, documented, and understandable to downstream consumers.

### 10.2 What should remain outside analytics engineering ownership

Analytics engineering should not unilaterally decide:

- whether a pricing plan should be changed;
- whether phone support should become a paid feature;
- whether a customer should be moved to another plan;
- whether a support policy should be restricted or expanded;
- whether a sales, customer success, finance, product, or risk team should take a specific action;
- whether a review candidate is definitively a problem.

Those are business decisions. Analytics engineering can make the evidence clearer, but the interpretation and action should be owned by the appropriate domain stakeholders.

### 10.3 Stakeholder questions to clarify before modeling

Before implementing review-oriented classifications, I would first align with stakeholders on the meaning of key terms.

For example, if the business wants to identify high-touch accounts, I would clarify:

- Does high-touch mean phone support, dedicated customer success support, manual onboarding, advisory service, or total support workload?
- Should support load be measured by case count, handling time, escalation count, phone minutes, or estimated support cost?
- Should thresholds vary by plan, segment, region, account size, or contract type?
- Should temporary onboarding support be treated differently from recurring support burden?

If the business wants to identify low monetization, I would clarify:

- Should monetization mean subscription revenue, transaction revenue, fees, gross revenue, net revenue, gross margin, or contribution margin?
- Should the metric be measured monthly, quarterly, trailing 12 months, or over the customer lifetime?
- Should discounts, refunds, credits, or promotional pricing be deducted?
- Should the comparison be made against support cost, plan expectation, customer segment, or acquisition channel?

If the business wants to define review candidates, I would clarify:

- Is the output intended for executive reporting, customer success prioritization, support operations, finance review, product strategy, or risk monitoring?
- Should the mart produce descriptive status labels, priority scores, or operational work queues?
- Which false positives are acceptable, and which false negatives are costly?
- Who owns the follow-up action after a candidate is surfaced?

### 10.4 From initial detection rules to official review logic

Some review signals may be affected by temporary spikes, missing context, or lifecycle timing.

For example, a low-touch account may be flagged as `high_support_load` in a monthly mart because support activity increased during one reporting month. The classification may be technically correct for that month, but it may be misleading if the spike was caused by a one-time onboarding issue that has already been resolved.

In this situation, analytics engineering should not silently turn the initial detection rule into a business decision rule.

Instead, analytics engineering should surface the limitation and propose additional context, such as:

- trailing 3-month or 6-month support load;
- number of high-support-load months within a recent window;
- onboarding-period flags;
- support case categories;
- resolved and unresolved case counts;
- estimated support cost;
- plan tier and expected support level;
- prior-month and next-month comparison.

A diagnostic model such as `account_support_load_trend_monthly` may be useful before promoting the logic into an official review mart.

The diagnostic model could help stakeholders distinguish between:

- a one-time onboarding spike;
- recurring high support load;
- unresolved operational issues;
- expected high-touch support;
- low-touch accounts that create unexpectedly high support burden.

After the business owners agree on the definition, threshold, time window, and intended use, the logic can be promoted into an official review mart or governed metric.

This distinction matters because analytics engineering can identify modeling limitations and propose better analytical context, but the definition of what should become an official review condition should be aligned with the relevant stakeholders.

### 10.5 Example review classification

After stakeholder alignment, a reporting mart could expose a review-oriented field such as `support_monetization_status`.

Illustrative statuses may include:

| Status | Meaning | Possible follow-up owner |
|---|---|---|
| `high_touch_high_revenue` | The account receives high-touch support and also generates high revenue | Leadership / Sales / Customer Success |
| `high_touch_low_revenue` | The account receives high-touch support but monetization is weak | Customer Success / Finance / Sales |
| `low_touch_high_revenue` | The account generates high revenue with limited manual support | Leadership / Sales / Product |
| `low_touch_high_support_load` | The account is expected to be low-touch but creates high support load | Support / Product / Customer Success |
| `aligned` | The account does not currently match a review condition | No immediate follow-up owner |

The model should document how each status is derived. For example, it should specify whether `high_touch` is based on plan tier, support interactions, phone support usage, estimated support cost, or a combination of those inputs.

This makes the mart useful for decision support while keeping business ownership clear.


---

## 11. BI / Semantic Layer Exposure

In a production-oriented environment, I would generally treat a capable BI or semantic layer as the primary consumption layer for reusable dimensional models.

The purpose of this layer is not only to visualize dbt models, but also to provide governed relationships, measures, explores, dashboards, and self-service analysis experiences for business users.

### 11.1 Production-Oriented Exposure Pattern

A typical exposure pattern could be:

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
  - customer lifecycle explore / data model
  - account health explore / data model
  - product usage explore / data model
  - revenue and retention measures
  ↓
dashboards / self-service BI / stakeholder reporting
```

In this structure, dbt provides tested and documented facts and dimensions. The BI or semantic layer then defines user-facing relationships, governed measures, explores, dashboards, and self-service analysis paths.

This avoids pushing all dashboard-specific joins and calculations into ad hoc SQL while still allowing the BI or semantic layer to support flexible exploration.

### 11.2 Governed Relationships and Exploration

Reusable dimensional models such as customer, account, product, plan, usage, transaction, subscription, billing, and payment models can be exposed to the BI or semantic layer for governed exploration.

The important point is not to duplicate transformation logic in the BI layer. Instead, the BI or semantic layer should sit on top of tested dbt models and define how business users can safely explore them.

For example, the BI or semantic layer may define:

- how customer or account dimensions relate to usage, transaction, subscription, billing, or support facts;
- which joins are valid for exploration;
- which measures are available to business users;
- which dimensions are appropriate for slicing a metric;
- which grain and aggregation cautions should be made clear to downstream users.

This helps keep core transformation logic in dbt while still allowing business users to explore customer lifecycle, usage, monetization, retention, and review states through a governed interface.

### 11.3 Governed Measures and Metric Definitions

The BI or semantic layer can define governed measures on top of tested dbt models.

Examples of governed measures could include:

| Measure | Possible definition area |
|---|---|
| `activated_customers` | activation logic based on a defined milestone |
| `active_accounts` | account status, usage threshold, or subscription state |
| `monthly_active_users` | distinct active users within a reporting period |
| `revenue_amount` | billed, collected, recognized, transaction-based, or margin-based revenue logic |
| `churned_accounts` | subscription-based or activity-based churn definition |
| `retention_rate` | cohort-based or period-based retained account logic |
| `average_revenue_per_account` | revenue divided by active, paying, or eligible accounts |

The exact definitions should be aligned with business stakeholders and documented either in dbt model documentation, the semantic layer, or both.

Revenue-related measures should be especially explicit. The definition should clarify whether the metric represents billed revenue, collected payment amount, recognized revenue, transaction fees, gross revenue, net revenue, gross margin, or another business-specific monetization concept.

### 11.4 Dashboard and Self-Service BI Consumption

Dashboards and self-service BI should consume models at the grain intended for the business question.

For example, a customer lifecycle dashboard may monitor acquisition, activation, usage, monetization, and retention trends. An account health dashboard may focus on current account-level lifecycle and support states. A product usage dashboard may analyze engagement by product, feature, plan, segment, or account type. An operational review dashboard may surface accounts or customers requiring follow-up.

The BI or semantic layer should make model grain and metric interpretation clear to downstream users. For example, product-level active user counts in SaaS or category-level buyer counts in e-commerce may be valid within each product-month or category-month row. However, they should not be casually summed into a global distinct user or customer count when the same user or customer can appear in multiple rows.

This is especially important when dashboards combine lifecycle, usage, revenue, support, and review-state metrics. A dashboard can make these metrics easy to consume, but it should not hide the assumptions that make the metrics valid.

### 11.5 Interpretation Cautions and Bridge to Reporting Marts

The BI or semantic layer should expose business metrics in a way that is easy to use, but it should not hide important modeling assumptions.

Documentation should explain:

- model grain;
- metric definitions;
- stock and flow semantics;
- source-system assumptions;
- known limitations;
- valid aggregation paths;
- ownership of business definitions;
- interpretation cautions for review states.

For lifecycle review outputs, the BI layer should make review states visible and explainable, but it should not silently turn those states into automatic pricing, campaign, support, finance, customer success, or product actions.

For example, a `churn_risk_candidate` or `dormant_customer_with_prior_high_value` status may indicate that an account is worth reviewing. It does not by itself decide whether a campaign, discount, outreach action, pricing change, or support intervention should occur.

Those follow-up decisions should be aligned with the appropriate business stakeholders.

In practice, this documentation may be supported by dashboard descriptions, metric definitions, semantic model descriptions, data catalog entries, or BI tooltips. For example, dbt exposures can document downstream uses such as dashboards, while semantic layers and BI tools can expose metric definitions, descriptions, relationships, and interpretation context directly to data consumers.

Analytics engineering can own or strongly contribute to this documentation because it connects model grain, metric logic, lineage, and dashboard interpretation. However, metric documentation should still be aligned with business stakeholders when the metric affects official reporting, review workflows, or business action rules.

Even when a capable BI or semantic layer exists, some logic may still be better materialized in dbt reporting marts. This is especially true for repeated aggregations, stock-and-flow logic, review candidate classification, dashboard performance optimization, or cross-tool reuse.

The next section discusses where dbt reporting marts add value.


---

## 12. dbt Reporting Marts Where Useful

Even when a capable BI or semantic layer exists, dbt reporting marts can add value where repeated, complex, or review-oriented logic should be materialized.

I would consider dbt reporting marts for cases such as:

- heavy or repeated aggregations;
- complex stock-and-flow metrics;
- dashboard performance optimization;
- stable dashboard-ready extracts;
- operational review candidate tables;
- cross-tool reuse across BI, notebooks, SQL, scripts, and reports;
- business logic that should be tested and reviewed outside a single BI tool.

Illustrative reporting mart candidates include:

| Mart | Example grain | Purpose |
|---|---|---|
| `customer_lifecycle_monthly` | One row per reporting month, segment, and lifecycle state | summarize lifecycle movement and account/customer state |
| `activation_funnel_weekly` | One row per signup week and funnel step | analyze onboarding conversion and drop-off |
| `usage_engagement_monthly` | One row per reporting month, segment, product, and plan | analyze product engagement and usage intensity |
| `revenue_retention_monthly` | One row per reporting month, segment, product, and plan | compare revenue, retention, expansion, and churn |
| `account_health_current` | One row per current account | expose current account status and health indicators |
| `churn_risk_candidates` | One row per account or customer requiring review | support customer success or retention follow-up |
| `monetization_review_candidates` | One row per account, customer, or product requiring review | surface usage/revenue/billing alignment issues |
| `operational_review_candidates` | One row per operational review candidate | support cross-functional follow-up |

The goal of these marts is not to replace the BI or semantic layer. The goal is to materialize stable, reusable, testable, and traceable business logic where doing so creates clear value.

---

## 13. Industry-Specific Interpretations

The same modeling archetype can be interpreted differently by industry.

The following mapping is illustrative. Actual implementation would depend on the company's source systems, business model, data quality, and stakeholder priorities.

| Generic concept | SaaS interpretation | Fintech interpretation | E-commerce / Marketplace interpretation | B2B service interpretation |
|---|---|---|---|---|
| Customer / account | Account, workspace, organization, user | Customer, account, wallet, merchant | Customer, buyer, seller, shop | Account, client, contract, organization |
| Product / plan | Product, module, feature, subscription plan | Account type, financial product, card, wallet feature | Product, SKU, category, listing, marketplace service | Service plan, contract type, solution area |
| Acquisition | Signup, trial start, lead source | Application start, account opening, campaign source | Registration, first visit, seller onboarding | Lead creation, contract start, onboarding request |
| Activation | First key action, onboarding completion, first team invite | KYC completion, first deposit, first transaction | First purchase, first listing, first sale | First service use, first project launch |
| Usage / activity | Sessions, feature usage, API calls, seats used | Transactions, transfers, balance activity, card usage | Orders, sessions, listings, cart activity, seller activity | Service usage, workflow activity, support interactions |
| Monetization | Subscription revenue, MRR, ARR, expansion | Fees, interchange, transaction revenue, interest | GMV, net revenue, take rate, order value | Contract revenue, monthly fees, usage-based revenue |
| Retention | Renewal, continued usage, churn risk | Continued account activity, dormant accounts | Repeat purchase, repeat sales, buyer/seller retention | Renewal, continued contract usage, account health |
| Review signals | Low usage after onboarding, paid account with no activity | Failed onboarding, dormant account, unusual inactivity | High return rate, no repeat purchase, inactive seller | High support load, low service usage, renewal risk |

This mapping is useful because it lets a general modeling case study remain broadly applicable while still helping readers translate it into their own domain.

---

## 14. Summary

Customer lifecycle and monetization analytics is a useful target-domain case study because it appears across many business models while still requiring concrete modeling decisions.

The core modeling sequence is:

```text
stakeholder question
  → business process
  → source data
  → model grain
  → facts and dimensions
  → intermediate logic
  → tests and documentation
  → BI / semantic layer or reporting marts
  → decision-support output
```

The most important modeling decisions are not the exact model names. They are:

- which business processes should be modeled as facts;
- which entities should become reusable dimensions;
- what grain each model should expose;
- how stock and flow metrics should be separated or combined;
- which assumptions should be tested;
- which business review signals should be surfaced as outputs;
- which logic belongs in dbt, and which logic belongs in the BI or semantic layer.

This case study is designed to serve as a reusable bridge between general dbt modeling principles and company-specific modeling hypotheses.

## References and Public Sources

This document is based on portfolio implementation experience and public information. The following sources were used as references for dbt modeling principles, analytics engineering practices, documentation, semantic-layer thinking, and public examples of data team workflows.

- dbt Labs. “What is dbt?” dbt Developer Hub, version 1.11.  
  https://docs.getdbt.com/docs/introduction?version=1.11  
  Used as a primary reference for dbt's role in analytics engineering, transformation workflows, testing, documentation, and software engineering practices for analytics code.

- dbt Labs. “Best Practice Guides.” dbt Developer Hub, version 1.11.  
  https://docs.getdbt.com/best-practices?version=1.11  
  Used as a primary reference for recommended dbt project structure, modeling patterns, testing practices, and maintainable analytics engineering workflows.

- dbt Labs. “Developer Blog.” dbt Developer Hub.  
  https://docs.getdbt.com/blog?version=1.11  
  Used as a supplementary reference for current discussions around analytics engineering, semantic layers, data modeling, dbt ecosystem practices, and developer-oriented perspectives from dbt Labs and the dbt community.

- GitLab. “Data Team - How We Work” and “dbt Change Workflow.” The GitLab Handbook.  
  https://handbook.gitlab.com/handbook/enterprise-data/how-we-work/  
  https://handbook.gitlab.com/handbook/enterprise-data/how-we-work/dbt-change-workflow/  
  Used as public examples of how a mature data team documents workflows, responsibilities, review processes, downstream impact checks, and analytics engineering change management.
