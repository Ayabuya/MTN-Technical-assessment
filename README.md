# MTN Technical Assessment

This repository contains my technical assessment for MTN. It is a theoretical, presentation-focused response to the assessment brief and is designed to show how I approach data engineering challenges in a telecom environment.

The focus is on building a trusted, scalable, and governable data platform for customer and transaction data. The work covers architecture, pipeline design, operational resilience, and data quality controls without relying on any production implementation or live system access.

## Overview

I have structured the assessment to reflect the type of thinking I would bring to a real data engineering problem at MTN:

- a modern Lakehouse architecture for ingestion and analytics
- clear separation between raw, trusted, and consumption-ready data layers
- strong handling of duplicates, late-arriving records, and invalid data
- operational controls for pipeline recovery, monitoring, and stakeholder communication
- governance and security controls appropriate for customer and transaction data

## Assessment scope

The repository includes the core elements of the brief and presents them as a coherent solution narrative:

- architecture and medallion-layer design
- business logic placement across Bronze, Silver, and Gold
- pipeline and integration design for high-volume transaction processing
- failure recovery and communication planning
- data quality, governance, and security controls

## Repository structure

- docs/ contains the assessment responses and design notes
- README.md introduces the project and its objective
- The material is intentionally theoretical and presentation-focused

## Why this is relevant to MTN

The design choices reflect the realities of a large telecommunications and digital-services business:

- customer and transaction data must be trusted and consistent
- data pipelines need to be resilient and auditable
- governance is essential because the data includes sensitive information
- analytics and operational reporting must be built on reliable, well-governed foundations

## Notes

- This is a theoretical assessment and not a live production deployment.
- No production credentials, environment details, or sensitive customer data are included.
- The work is written to be clear, credible, and suitable for a hiring or technical review conversation.


