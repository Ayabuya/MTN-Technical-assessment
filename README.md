# Technical Assessment Q&A

This repository contains the written responses for a data engineering assessment. It is intentionally documentation-focused and does not include any implementation code or production environment details.

## Purpose

The assessment covers how to design a trusted, scalable, and governable data platform for customer and transaction data, with a strong focus on telecom-style operational realities.

## Q&A overview

1. Q3A — [Lakehouse & Medallion Architecture](docs/question%203A_lakehouse_medallion_architecture.md)
   - Explains the raw-to-consumption data flow and the purpose of Bronze, Silver, and Gold.

2. Q3B — [Business Logic by Medallion Layer](docs/question%203B_layer_logic.md)
   - Defines what belongs in each layer and what should remain out of scope for each stage.

3. Q4A — [Pipeline & Integration Design](docs/question%204A_pipeline_integration_design.md)
   - Covers ingestion, schema handling, deduplication, reprocessing, monitoring, and CI/CD.

4. Q4B — [Failure Recovery & Stakeholder Communication](docs/question%204B_failure_recovery_communication.md)
   - Describes how to recover from a failed pipeline and keep stakeholders informed.

5. Q5A — [Data Quality, Governance & Security](docs/question%205A_data_quality_governance_security.md)
   - Connects technical controls to trust, compliance, privacy, and data stewardship.

## Repository structure

- docs/ — assessment answers and design notes
- README.md — project overview and Q&A index

## Notes

- This is a theoretical assessment.
- No production code, secrets, or live system access are included.
- The focus is on clear design thinking and structured reasoning.


