# FlowPay Product Analytics Case Study

## Project Overview

This project is an end-to-end product analytics case study based on a synthetic fintech product called **FlowPay**. FlowPay allows users to register, complete identity verification, and make international money transfers.

The project investigates the customer journey across these stages and demonstrates a structured product analytics workflow, from data validation and metric definition to exploratory analysis, visualization, and product recommendations.

The analysis is designed as a realistic portfolio project and focuses on analytical reasoning rather than only technical execution.

## Project Objectives

The objectives of this case study are to:

- understand the structure and quality of the available product data;
- define relevant product metrics and analytical assumptions;
- examine the user journey through registration, verification, and transfers;
- identify meaningful patterns, friction points, and areas for further investigation;
- translate analytical findings into practical product recommendations;
- clearly distinguish observed relationships from causal conclusions.

The final analytical questions and scope will be refined after the data preparation and exploratory analysis stages.

## Data

The project uses five synthetic datasets:

- `users` — user registration and profile information;
- `events` — product interaction events;
- `verifications` — identity verification records;
- `transfers` — money transfer records;
- `support_contacts` — customer support interactions.

All data used in this project is synthetic and does not contain real customer information.

## Tools

- **Google BigQuery and SQL** — data storage, validation, transformation, and analysis;
- **Python** — exploratory analysis, statistical analysis, and visualization where appropriate;
- **Jupyter Notebook** — reproducible Python analysis;
- **Git and GitHub** — version control and project documentation;
- **Visual Studio Code** — local development environment.

## Repository Structure

```text
fintech-case-study/
├── data/
│   ├── raw/            # Raw data is excluded from version control
│   └── processed/      # Locally generated datasets are excluded from version control
├── images/             # Charts and other visual outputs
├── notebooks/          # Jupyter notebooks
├── reports/            # Final reports and supporting documentation
├── sql/                # SQL queries organized by analysis stage
├── src/                # Reusable Python code
├── .gitignore
├── README.md
└── requirements.txt
```

## Analytical Workflow

The project follows these stages:

1. Set up the analytical environment and repository.
2. Load the source tables into BigQuery.
3. Validate table structure, data types, completeness, and consistency.
4. Document the data model, assumptions, and metric definitions.
5. Conduct exploratory product analysis.
6. Investigate the customer journey and relevant segments.
7. Use Python for analysis that benefits from statistical or visual exploration.
8. Summarize findings, limitations, and product recommendations.
9. Prepare a portfolio-ready case study presentation.

## Project Status

**In progress — data preparation and validation.**

## Notes

- Findings will be added only after the relevant analysis has been completed and validated.
- Metric definitions, assumptions, and limitations will be documented alongside the analysis.
- The repository will evolve as the investigation progresses.
