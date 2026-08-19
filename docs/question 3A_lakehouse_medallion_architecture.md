# Lakehouse & Medallion Architecture

## Overview

Target data architecture explaining the Sources → Ingestion → Bronze → Silver → Gold → Consumption and what business logic belongs in each layer


## Architecture diagram

```mermaid
%%{init: {'flowchart': {'useMaxWidth': false, 'nodeSpacing': 140, 'rankSpacing': 80, 'diagramPadding': 20}}}%%
flowchart LR
    subgraph Sources
        S1[SAP S/4HANA]
        S2[SAP CRM]
        S3[Retail POS]
        S4[Pharmacy Systems]
        S5[Loyalty Platforms]
        S6[External Financial Partner]
    end

    Bronze[Bronze]
    Silver[Silver]
    Gold[Gold]
    Consumption[Consumption]

    S1 --> Bronze
    S2 --> Bronze
    S3 --> Bronze
    S4 --> Bronze
    S5 --> Bronze
    S6 --> Bronze

    Bronze --> Silver
    Silver --> Gold
    Gold --> Consumption

    subgraph BronzeDetails[ ]
        B1[Raw Delta tables]
        B2[Append-only audit landing]
        B3[Load metadata / batch info]
        B4[Late-arrival tagging]
        B5[Source payload preservation]
    end
    Bronze --> B1
    Bronze --> B2
    Bronze --> B3
    Bronze --> B4
    Bronze --> B5

    subgraph SilverDetails[ ]
        C1[Schema enforcement]
        C2[Invalid row handling]
        C3[Deduplicated entities]
        C4[Cleansed business data]
        C5[Entity joins]
        C6[Incremental reconciliation]
    end
    Silver --> C1
    Silver --> C2
    Silver --> C3
    Silver --> C4
    Silver --> C5
    Silver --> C6

    subgraph GoldDetails[ ]
        D1[Analytics-ready metrics]
        D2[Business KPIs]
        D3[Semantic models]
        D4[Consumption-ready tables]
    end
    Gold --> D1
    Gold --> D2
    Gold --> D3
    Gold --> D4
```

## Diagram explanation

- **Sources**: multiple operational systems feed raw data directly into Bronze, which includes ingestion and landing responsibilities.
- **Bronze**: ingests and stores raw Delta tables in append-only form, preserves audit records, and tags late arrivals.
- **Silver**: applies schema enforcement, invalid-row handling, deduplication, and business cleansing.
- **Gold**: generates analytics-ready metrics, KPIs, and semantic tables for consumption.
- **Consumption**: supports BI, SQL analytics, ML feature pipelines, and data APIs using trusted Gold datasets.

## Sources

Sources include:

- SAP S/4HANA
- SAP CRM
- Retail POS transactions
- Pharmacy systems
- Loyalty platforms
- External financial-services partner

Each source should deliver data directly into Bronze through a controlled landing mechanism such as file landing, streaming event bus, or partner API. The full Bronze landing and ingestion workflow can be implemented using Spark Structured Streaming (SDS) and Delta Autoloader, which enables schema inference, incremental file discovery, and append-only writes into Delta.