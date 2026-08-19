# MTN Technical Assessment Q&A

This document consolidates the interview-style Q&A provided for the MTN technical assessment, covering SQL, Power BI, Excel, and data engineering design questions.

---

## SECTION 1 - SQL (5 Questions)

### SQL Question 1 - Window Functions

You have a transaction table:

- msisdn
- transaction_date
- amount

Write a SQL query that returns each customer's first transaction date and amount.

```sql
WITH RankedTransactions AS (
    SELECT
        msisdn,
        transaction_date,
        amount,
        ROW_NUMBER() OVER (
            PARTITION BY msisdn
            ORDER BY transaction_date ASC
        ) AS rn
    FROM transaction
)

SELECT
    msisdn,
    transaction_date,
    amount
FROM RankedTransactions
WHERE rn = 1;
```

### SQL Question 2 - Data Quality

You have two tables:

- transactions: transaction_id, amount
- partner_transactions: partner_tx_id, amount

Write a SQL query that returns transactions that exist in transactions but not in partner_transactions.

```sql
SELECT
    t.transaction_id,
    t.amount
FROM transactions t
LEFT JOIN partner_transactions pt
    ON t.transaction_id = pt.partner_tx_id
WHERE pt.partner_tx_id IS NULL;
```

### SQL Question 3 - Aggregation Logic

You have a table:

- agent_id
- transaction_date
- amount

Write a query that returns:

- agent_id
- total_transactions
- total_amount

for the last 30 days only.

```sql
SELECT
    agent_id,
    COUNT(*) AS total_transactions,
    SUM(amount) AS total_amount
FROM transactions
WHERE transaction_date >= DATEADD(day, -30, CAST(GETDATE() AS DATE))
GROUP BY agent_id;
```

### SQL Question 4 - Data Engineering Scenario

Your dataset has duplicates because of ingestion retries.

Table:

- transaction_id
- amount
- load_timestamp

Return only the latest record per transaction_id.

```sql
WITH Deduped AS (
    SELECT
        transaction_id,
        amount,
        load_timestamp,
        ROW_NUMBER() OVER (
            PARTITION BY transaction_id
            ORDER BY load_timestamp DESC
        ) AS rn
    FROM transactions
)

SELECT
    transaction_id,
    amount,
    load_timestamp
FROM Deduped
WHERE rn = 1;
```

### SQL Question 5 - Thinking Question

A table has 1 billion rows. A query joining two large tables suddenly became very slow. Name two things you would check first.

Answer:

- Check the indexes on the join keys and filter columns.
- Review the execution plan to identify whether the query is scanning large tables, missing joins, or causing expensive sorts/hash operations.

---

## SECTION 2 - Power BI (4 Questions)

### Power BI Question 1 - Data Model

You have two tables:

- Transactions: transaction_id, agent_id, amount
- Agents: agent_id, region

What relationship should exist between them?

Answer: B) One to many

- Transactions (many) -> Agents (one)

### Power BI Question 2 - DAX

Write a measure that returns Total Sales.

```DAX
Total Sales = SUM(Transactions[amount])
```

### Power BI Question 3 - Performance

A report becomes slow when slicing by date. Name two things you would check.

Answer:

1. Check the relationship between the Date dimension table and the Fact table.
2. Check whether Auto Date/Time is enabled and whether it is creating unnecessary date tables.

### Power BI Question 4 - Real Scenario

You want to show:

- Total transactions
- Unique customers

in one visual.

What aggregation functions would you use?

```DAX
Total Transactions = COUNT(Transactions[transaction_id])
Unique Customers = DISTINCTCOUNT(Transactions[customer_id])
```

Then place the measures on the Y axis and use the Date field on the X axis.

---

## SECTION 3 - Excel (4 Questions)

### Excel Question 1

You have:

| Agent | Amount |

Write a formula to calculate total amount per agent.

```excel
=SUMIF(A:A, A2, B:B)
```

### Excel Question 2

How do you remove duplicate rows in Excel?

Answer:

- Go to the Data tab
- Click Remove Duplicates

### Excel Question 3

You need to look up a Region from another sheet using Agent ID. Which function would you use?

Answer:

- VLOOKUP

### Excel Question 4

You receive a CSV file where numbers appear as text. Name two ways to convert them to numbers.

Answer:

1. Use the VALUE() function.
2. Select the text-number column and use Data > Text to Columns.

---

## SECTION 4 – Data Engineering (3 Questions)

### Experience Question 1

Describe a data pipeline you have built from source to reporting.

Explain:

- source system
- transformation
- storage
- reporting layer

Answer:

I use the medallion architecture when creating an end-to-end data pipeline.

#### Bronze

Land the data from source systems, making sure it is immutable for audit trail purposes, with an append-only pattern. The only additions to the landed data are metadata columns such as import timestamp, source system, and filename.

#### Silver

Transformation logic is applied here such as data type casting, deduplication, null remediation, and PII obfuscation or masking. For late-arriving data, a merge/upsert pattern can be used to handle updates.

#### Gold

Data is highly refined and aggregated for consumption and analytics, such as modeling the data in different schemas and calculating business metrics before final reporting.

### Experience Question 2

Tell me about a time when data was wrong in production. What steps did you take to investigate?

Answer:

This is a behavioral/experience-based response and should be tailored to a real incident. In a typical response, the candidate would explain:

- I identified the data discrepancy in the source vs. target data, once I had identified the decrepency I Then worked backwards from production to analyze where the issue was ( where ingestion, transformation or aggregation ) essentially attempting to isolate the root cause, after identifying the root cause I implemented the fix and documented the process to prevent further issues

### Experience Question 3

What is the largest dataset you have worked with? How many rows and how did you handle performance?

Answer:

I worked with a table around 1 TB. Once the data was landed on Azure Blob Storage ( via Apache airflow), I followed the medallion architecture. In the Gold layer, when consumed in Power BI, the selections were becoming slower, so I aggregated the data before Power BI consumption and used optimization methods such as OPTIMIZE, VACUUM, and indexing in the Silver layer to improve downstream performance.

---

