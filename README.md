# FlowPay Product Analytics Case Study

## Overview

FlowPay is an independent product analytics portfolio project based on synthetic data for a fictional European money-transfer product.

The project investigates why some new users do not complete their first successful transfer within seven days of signing up and which measurable source of friction or acquisition-mix issue the Product team should investigate or test first.

The case study is designed to demonstrate product judgement, metric design, SQL analysis, data-quality investigation, statistical thinking, visual communication, and decision-oriented recommendations.

## Product Question

> What is preventing new users from completing their first successful transfer within seven days of signup, and what should the Product team address first?

The customer journey examined in the project is:

`signup → identity verification → recipient creation → transfer initiation → successful transfer`

The analysis will first establish whether seven-day activation has changed over time. It will then investigate where the observed friction occurs, which users are most affected, and whether the strongest evidence points to product experience, acquisition mix, geography, verification, or transfer behaviour.

Because the source data is observational, the project will distinguish associations from causal conclusions. Findings will be used to prioritise a product action and design a future experiment rather than claim an unobserved causal uplift.

## Primary Metric

The primary outcome is `activation_7d`.

A user is activated when they complete at least one successful transfer in the half-open interval:

`[signup_timestamp, signup_timestamp + 168 hours)`

The denominator includes all eligible non-internal users, including users with no events, verification attempts, or transfers. The `transfers` table is the source of truth for the activation outcome; `events` is used for behavioural analysis and reconciliation.

Eligibility rules, observation-window requirements, exclusions, boundary conditions, and supporting metrics will be documented in `docs/metric_contract.md` before the final metric is calculated.

## Data

The project contains five relational synthetic datasets:

| Dataset | Grain | Rows | Purpose |
|---|---|---:|---|
| `users.csv` | One row per registered user | 50,000 | Signup attributes and eligibility |
| `events.csv` | One row per tracked product event | 273,596 | Behavioural journey and event reconciliation |
| `verifications.csv` | One row per verification attempt | 38,029 | Verification outcomes, attempts, and failure context |
| `transfers.csv` | One row per transfer attempt | 28,324 | Transfer outcomes and activation source of truth |
| `support_contacts.csv` | One row per support contact | 1,411 | Customer-reported friction and support context |

The data is synthetic and contains no real customer information. Raw files are treated as immutable source data. Known data-quality issues, reconciliation decisions, and analytical limitations will be recorded in `docs/quality_log.md`.

## Tools

- **Supabase / PostgreSQL and SQL** — relational storage, schema constraints, validation, transformation, and product analysis;
- **Python and Jupyter** — targeted exploratory analysis, statistical checks, and reproducible validation;
- **Tableau** — portfolio dashboard and visual communication;
- **Git and GitHub** — version control, documentation, and reproducibility;
- **GitHub Pages** — final public case-study narrative.

## Repository Structure

```text
fintech-case-study/
├── data/
│   ├── raw/                 # Immutable synthetic source files
│   ├── processed/    # Temporary generated datasets; not committed
│   └── published/    # Small validated datasets behind final visuals; committed
├── docs/
│   ├── project_brief.md
│   ├── metric_contract.md
│   ├── data_dictionary.md
│   ├── quality_log.md
│   └── decision_log.md
├── images/                  # Exported portfolio visuals
├── notebooks/               # Focused and reproducible Python analysis
├── reports/                 # Decision memo and presentation materials
├── sql/                     # Ordered validation, modelling, and analysis queries
├── src/                     # Reusable Python code if required
├── .gitignore
├── README.md
└── requirements.txt
```

## Analytical Plan

1. Document table grain, keys, relationships, timestamps, and sources of truth.
2. Validate primary keys, foreign-key relationships, missingness, duplicates, timestamp logic, and coverage.
3. Finalise the metric contract and mature-cohort observation rules.
4. Build a tested analytical model with one row per eligible user without losing zero-activity users.
5. Establish signup volume and seven-day activation by mature signup cohort.
6. Reconstruct the observed customer journey and reconcile entity tables with tracked events.
7. Select at most two evidence-based deep dives rather than inspect every available segment.
8. Evaluate alternative explanations and distinguish composition effects from product friction.
9. Recommend one priority and one credible alternative, including uncertainty and missing evidence.
10. Design a future A/B test with a hypothesis, randomisation unit, exposure definition, primary metric, guardrails, power assumptions, duration, and decision rule.
11. Reconcile all published metrics across SQL, Python, Tableau, and the final narrative.

## Planned Outputs

- documented metric and data contracts;
- reproducible SQL validation and analysis;
- a tested user-level analytical dataset;
- cohort, activation, and milestone analysis;
- focused Python analysis where it adds statistical value;
- a Tableau dashboard with reconciled totals;
- a concise product decision memo;
- an A/B-test proposal;
- a public GitHub Pages case study.

## Current Status

**In progress — data model documentation and relational-key validation.**

Findings and recommendations will be published only after the corresponding analysis has been completed and checked. The repository will document where AI assisted the workflow; every published query, definition, visual, and conclusion remains explainable and verifiable by the author.
