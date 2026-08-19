# MTN Technical Assessment: Lakehouse Data Engineering Solution

This repository is a portfolio-ready version of my technical assessment, prepared for MTN and for other potential employers evaluating my data engineering capability.

The work demonstrates how I would design, build, and operate a modern data platform for large-scale transaction and customer analytics using a Lakehouse architecture, SQL-based transformation logic, PySpark ETL, and strong operational controls around data quality, deduplication, and idempotency.

## Project objective

The goal of this assessment is to show that I can:

- design reliable, scalable data pipelines for large enterprise datasets
- build trusted analytics layers with clear business logic and lineage
- enforce quality checks and recovery patterns in production-style workflows
- deliver practical engineering solutions that balance performance, governance, and maintainability

## Assessment scope

This project is structured as a clean, reviewable technical submission based on the assessment document provided for MTN. It brings together the key solution areas needed to demonstrate strong data engineering capability in a modern telecom and digital-services context:

- SQL and analytical reporting for customer and transaction insights
- Data validation, deduplication, and resilience in ETL flows
- Lakehouse design and medallion-style data layering
- Pipeline orchestration and operational recovery thinking
- Data quality, governance, and security controls

## Repository structure

```text
.
├── docs/                                      # Architecture, integration, governance, and recovery answers
├── src/
│   └── etl/
│       ├── question 2A: clean_transactions.py
│       └── question 2B: pipeline_idempotent.py
├── sql/
│   ├── question_1A_customer_monthly_summary.sql
│   ├── question_1B_top_customers_by_region.sql
│   ├── question1: test_data_setup.sql
│   └── question 1C: performance_diagnosis.md
├── tests/
│   └── test_question2.py
├── requirements-test.txt
├── run_question2_tests.sh
├── README.md
└── .github/
```

## Why this is relevant to MTN and modern data engineering

This solution reflects the kind of thinking expected in a telecom or digital-services context:

- trusted customer and transaction data at scale
- awareness of data quality and operational resilience
- Lakehouse patterns suited to structured analytics and downstream consumption
- clear separation of raw, refined, and analytics-ready datasets
- emphasis on business logic, governance, and maintainability rather than ad hoc scripts

## Assessment notes

- The repository is intended as a technical assessment artifact and presentation asset.
- It does not include production credentials, sensitive data, or live infrastructure secrets.
- The technical substance has been preserved while the project framing has been adapted for a professional hiring audience.

## Presentation focus

This MTN version is designed to be reviewed as a full solution narrative rather than as a numbered answer set. The project highlights the practical engineering decisions behind a modern, trusted, production-ready data platform for a telecom or digital services environment.


