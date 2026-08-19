# Data Quality, Governance & Security

This document describes controls for customer and transaction data that contains personally identifiable information (PII) and is shared internally and with an external partner.

## Problem context

Customer and transaction data contains personally identifiable information and is shared between internal platforms and an external partner.
Question A: Design the controls you would implement from ingestion through consumption. Connect technical controls to trust and compliance, rather than treating governance as a separate governance-team responsibility.

## Control categories

The controls are organized into these categories:

- ingestion controls
- data quality controls
- security controls
- governance controls
- partner controls
- monitoring, auditing, and compliance controls

## Bronze controls

Bronze is the raw landing layer and the first control boundary.

### What to do in Bronze

- Append-only landing of raw source data for audit and replay.
- Capture ingest metadata such as `source_system`, `file_name`, `batch_id`, `load_timestamp`, and `source_checksum`.
- Apply data contract validation to identify malformed or schema-variant records.
- Quarantine invalid records in a separate Bronze-level store without discarding them.
- Tag late-arriving records and preserve those events for reconciliation.
- Store raw data in Delta with encryption at rest.

### Why this matters

- preserves the original source payload for audit and troubleshooting
- provides an immutable record of what was received from each partner
- avoids silent data loss by retaining invalid and late-arriving rows
- supports forensic analysis and reconciliation if downstream issues occur

## Silver controls

Silver is the curated business entity layer where quality and governance are enforced.

### What to do in Silver

- Enforce schema and contract validation against the canonical transaction model.
- Cleanse and normalize fields such as `transaction_timestamp`, `transaction_amount`, and `customer_id`.
- Deduplicate records using a defined business key such as `transaction_id`.
- Validate business rules, for example: positive amounts, valid store IDs, and customer existence.
- Apply PII-sensitive handling such as tokenization, masking, or column-level encryption where appropriate.
- Track data lineage from Bronze to Silver, including transformation logic and audit metadata.
- Maintain a business-quality Silver table plus separate quarantine/exception tables.

### Why this matters

- prevents dirty or inconsistent data from reaching analytics and reporting
- enforces the domain rules that make transaction data trustworthy
- preserves a clear transformation lineage for compliance and debugging
- ensures PII is protected before downstream consumption

## Gold controls

Gold is the analytics and consumption layer where data is published to users.

### What to do in Gold

- Expose aggregated and normalized datasets for reporting, loyalty analytics, and AI features.
- Implement row-level and column-level access controls to restrict sensitive data.
- Use approved semantic models or curated views instead of raw Silver tables.
- Apply data minimization: only publish the fields required by each consumer.
- Document and enforce consumption policies for external partners and internal teams.
- Maintain data quality metrics and certification status for Gold datasets.

### Why this matters

- provides trusted datasets for business users without exposing raw PII
- reduces risk by limiting the scope of data available to each consumer
- ensures reporting and analytics operate on certified, governed outputs

## Security controls

### Authentication and authorization

- Use centralized identity management and role-based access control (RBAC).
- Implement least-privilege access for engineers, analysts, and external partners.
- Prefer managed catalog security such as Unity Catalog, Lakehouse ACLs, or cloud-native data governance services.

### Encryption

- Encrypt data at rest in object storage and Delta tables.
- Encrypt data in transit between systems and clients.
- Protect PII columns with additional encryption or tokenization when required.

### Data masking and anonymization

- Mask or hash sensitive fields such as customer name, email, and payment identifiers.
- Use tokenization for fields that need limited reuse in analytics.
- Avoid exposing raw PII in Gold-level datasets unless explicitly authorized.

## Governance controls

### Metadata and lineage

- Capture metadata for every dataset: owner, steward, source, schema, and refresh cadence.
- Track lineage across Bronze→Silver→Gold to show how data was transformed.
- Store schema and contract versions to detect drift and support audits.

### Data quality metrics

- Define quality metrics such as completeness, validity, uniqueness, and timeliness.
- Measure and log quality metrics at each layer.
- Use thresholds and alerts for quality degradation.

### Policy enforcement

- Use data contracts to define accepted schemas, value ranges, and PII handling rules.
- Enforce policies through automated validation pipelines and governance tools.
- Maintain a policy exceptions process for approved deviations.

## Partner and sharing controls

- Define partner-specific data contracts and SLAs for data delivery, quality, and privacy.
- Use secure data sharing mechanisms rather than ad hoc file exchange.
- Limit shared data to the minimum required scope, with partner-specific filters and masking.
- Log partner access and shared dataset usage separately.
- Implement contractual controls for data retention, deletion, and breach notification.

## Monitoring, auditing, and compliance controls

- Audit every pipeline run, data publish, and access event.
- Emit logs for data ingress, validation failures, reprocessing events, and access policy violations.
- Build dashboards for SLA performance, quality metrics, and audit trail completeness.
- Automate alerts for contract violations, schema drift, and unexpected access patterns.
- Retain audit logs long enough to satisfy compliance requirements.

## Why this is trust-building

These controls connect technical implementation to trust and compliance by:

- making data quality visible and enforceable
- ensuring PII is protected across the lakehouse
- preserving an auditable lineage from raw ingestion to consumption
- enforcing partner contracts with automated validation and secure sharing
- reducing risk through least-privilege access and data minimization
