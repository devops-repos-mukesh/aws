# Amazon Athena

## SQL Queries & Data Analytics

Amazon Athena is a fully managed, serverless interactive query service that allows you to analyze data stored in **Amazon S3** using standard **SQL**. It enables organizations to perform ad hoc analytics without provisioning or managing servers. Athena integrates seamlessly with AWS services such as AWS Glue Data Catalog, AWS Lake Formation, Amazon QuickSight, and IAM, making it an ideal solution for data lakes, log analysis, business intelligence, and security auditing.

---

# Table of Contents

1. [What is Amazon Athena?](#1-what-is-amazon-athena)
2. [Why Use Amazon Athena?](#2-why-use-amazon-athena)
3. [Problems Athena Solves](#3-problems-athena-solves)
4. [How Amazon Athena Works](#4-how-amazon-athena-works)
5. [Amazon Athena Architecture](#5-amazon-athena-architecture)
6. [Workflow](#6-workflow)
7. [Core Components](#7-core-components)
   - [7.1 Amazon S3](#71-amazon-s3)
   - [7.2 AWS Glue Data Catalog](#72-aws-glue-data-catalog)
   - [7.3 Athena Query Engine](#73-athena-query-engine)
   - [7.4 Query Results](#74-query-results)
8. [Supported File Formats](#8-supported-file-formats)
9. [Sample SQL Queries](#9-sample-sql-queries)
10. [Athena Integrations](#10-athena-integrations)
11. [Pricing](#11-pricing)
12. [Advantages](#12-advantages)
13. [Limitations](#13-limitations)
14. [Best Practices](#14-best-practices)
15. [Real-World Use Cases](#15-real-world-use-cases)
16. [Athena vs Redshift](#16-athena-vs-redshift)
17. [Athena vs AWS Glue](#17-athena-vs-aws-glue)
18. [Summary](#18-summary)

---

# 1. What is Amazon Athena?

Amazon Athena is a **serverless query service** that allows users to analyze data stored directly in **Amazon S3** using **standard SQL**. There is no need to provision databases, manage servers, or load data into a data warehouse. Athena reads data directly from S3 and returns query results within seconds.

---

# 2. Why Use Amazon Athena?

Without Athena:

- Download files from S3
- Write custom scripts to process data
- Provision and manage databases
- Build analytics infrastructure

With Athena:

- Query data directly in S3
- No infrastructure management
- Pay only for the data scanned
- Automatic scaling
- Standard ANSI SQL support

---

# 3. Problems Athena Solves

- Ad hoc querying of large datasets
- Log analysis without ETL
- Data lake analytics
- Cost-effective business reporting
- Security and audit log analysis
- Eliminates the need to move data into databases

---

# 4. How Amazon Athena Works

1. Store data in **Amazon S3**.
2. Create metadata using **AWS Glue Data Catalog** or manually.
3. Write SQL queries in Athena.
4. Athena scans the required files in S3.
5. Query results are stored in an S3 output location.

---

# 5. Amazon Athena Architecture

<img width="1100" height="559" alt="image" src="https://github.com/user-attachments/assets/3d21676e-c0ce-45fc-8be2-a7b93bcfe3df" />


---

# 6. Workflow

<img width="1100" height="559" alt="image" src="https://github.com/user-attachments/assets/ac6356a3-9c5d-4a3d-96bf-3b28e3369c82" />


---

# 7. Core Components

## 7.1 Amazon S3

Stores structured and semi-structured data such as CSV, JSON, Parquet, ORC, and Avro files.

---

## 7.2 AWS Glue Data Catalog

Stores metadata including:

- Database names
- Table names
- Schema
- Column types
- Partitions

The catalog contains metadata only, not the actual data.

---

## 7.3 Athena Query Engine

Athena uses the **Trino (formerly Presto)** query engine to execute SQL queries directly on data stored in S3.

---

## 7.4 Query Results

After executing a query, Athena stores the output in a user-defined Amazon S3 bucket.

---

# 8. Supported File Formats

Athena supports:

- CSV
- JSON
- TSV
- Parquet
- ORC
- Avro
- Apache Iceberg
- Delta Lake (supported scenarios)

---

# 9. Sample SQL Queries

Retrieve all records:

```sql
SELECT * FROM employees;
```

Filter data:

```sql
SELECT *
FROM orders
WHERE status = 'Delivered';
```

Aggregate data:

```sql
SELECT product,
       SUM(amount)
FROM sales
GROUP BY product;
```

Join tables:

```sql
SELECT c.customer_name,
       o.order_amount
FROM customers c
JOIN orders o
ON c.customer_id = o.customer_id;
```

---

# 10. Athena Integrations

Amazon Athena integrates with:

- Amazon S3
- AWS Glue
- AWS Lake Formation
- Amazon QuickSight
- AWS IAM
- Amazon EventBridge
- AWS CloudTrail

---

# 11. Pricing

Athena charges **per terabyte (TB) of data scanned**.

Cost optimization techniques:

- Use Parquet or ORC
- Compress files
- Partition data
- Query only required columns

---

# 12. Advantages

- Fully managed
- Serverless
- Standard SQL support
- Automatic scaling
- No infrastructure management
- Direct integration with S3
- Cost-effective for analytics
- Fast ad hoc querying

---

# 13. Limitations

- Not suitable for OLTP workloads
- Query cost depends on data scanned
- Performance depends on file format and partitioning
- Large CSV files are slower than Parquet

---

# 14. Best Practices

- Store data in Parquet or ORC format.
- Partition datasets by date or region.
- Compress files using Snappy or GZIP.
- Use AWS Glue Crawlers for schema discovery.
- Filter data in queries to reduce scanned bytes.
- Store query results in a dedicated S3 bucket.
- Use IAM and Lake Formation for secure access.

---

# 15. Real-World Use Cases

- Analyze application logs
- Query AWS CloudTrail logs
- Build data lakes
- Business intelligence reporting
- Cost and usage analysis
- Security auditing
- Marketing analytics
- IoT data analysis

---

# 16. Athena vs Redshift

| Amazon Athena | Amazon Redshift |
|----------------|-----------------|
| Serverless | Managed data warehouse |
| Queries data in S3 | Stores data in Redshift |
| Pay per query | Pay for compute resources |
| Best for ad hoc analytics | Best for enterprise BI and reporting |

---

# 17. Athena vs AWS Glue

| Amazon Athena | AWS Glue |
|----------------|----------|
| Query service | ETL service |
| Executes SQL queries | Transforms and prepares data |
| Reads data directly from S3 | Cleans and moves data |
| Uses Glue Data Catalog | Creates metadata and ETL pipelines |

---

# 18. Summary

Amazon Athena is a serverless SQL query service that enables users to analyze data stored in Amazon S3 without managing infrastructure. By integrating with AWS Glue Data Catalog and supporting multiple file formats, Athena simplifies data lake analytics, log analysis, and business reporting. When combined with optimized storage formats such as Parquet and proper partitioning strategies, Athena provides a scalable, cost-effective, and highly efficient analytics solution.
