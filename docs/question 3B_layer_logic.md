# Business Logic by Medallion Layer

This document explains which business logic belongs in Bronze, Silver, and Gold, and which logic should be intentionally prevented from being implemented in each layer.

## Bronze

Bronze is the raw landing and audit layer.

### Appropriate logic
- Append-only landing of raw source data from all sources
- Source metadata capture, ingest batching information, and audit lineage
- Preservation of raw payloads for replay and traceability
- Late-arrival tagging and partitioned landing by ingest date
- Minimal staging-level normalization only when required for storage

### Logic to prevent
- Deleting or overwriting source rows that are required for audit
- Schema enforcement and invalid-record discard logic
- Business-specific aggregations or metrics
- Domain enrichment beyond minimal source normalization
- Report-level logic or KPI calculations
- Multi-source joins that belong in curated layers

## Silver

Silver is the curated business entity layer.

### Appropriate logic
- Read audit-grade Bronze data and build cleansed, canonical business entities
- Enforce expected source schemas and validate record structure
- Separate malformed or invalid rows for remediation while preserving audit lineage
- Standardize timestamps, derive business dates, and tag late arrivals
- Deduplicate by business key and retain the correct canonical record
- Apply business validation rules and data corrections in a controlled, audit-friendly way
- Store cleansed Delta tables for entities such as customers, transactions, rewards, and loyalty events

### Logic to prevent
- One-off or dashboard-specific KPIs
- Final aggregated reports that are not reusable
- Overly reactive custom filters for a single consumer
- Raw ingestion or source recovery logic

## Gold

Gold is the analytics and consumption-ready layer.

### Appropriate logic
- Business metrics and KPI calculation
- Aggregated fact tables for reporting and analytics
- Semantic data marts for customer analytics, loyalty insights, and operational reporting
- Data models designed for query performance and consumability
- ML feature-ready datasets or feature tables for downstream use

### Logic to prevent
- Raw cleansing and ingestion validation
- Low-level deduplication or entity reconciliation
- Source-specific format or contract handling
- Transient staging artifacts or debugging data

## Summary

The medallion architecture should keep responsibilities separate:
- Bronze: ingest and validate raw source data
- Silver: clean, deduplicate, and assemble business entities
- Gold: deliver analytics-ready metrics and consumption layers

By preventing business metrics in Bronze and raw ingestion plumbing in Gold, the pipeline remains modular, auditable, and maintainable.
