# Azure-End-to-end-Data-Engineering-Project
A project to familarise myself with the Azure data engineering ecosystem

## 1. Project overview

- **Goal**: Briefly describe the business problem (e.g., “build a scalable pipeline to ingest and transform X data for reporting”).
- **Scenario**: Where the data comes from, who would use the outputs.
- **Architecture style**: Medallion (Bronze → Silver → Gold) / Delta Lake / batch + near real-time, etc.

## 2. Architecture

- **High-level flow**:
  1. Ingest data from `<source>` into Azure Data Lake Gen2 (Bronze).
  2. Transform with Databricks (Bronze → Silver → Gold).
  3. Expose curated tables to `<Synapse / SQL DB / Power BI>` for analytics.
- Include an image:
  - `docs/architecture.png` (add later and link as: `![Architecture](docs/architecture.png)`)

## 3. Tech stack

- Azure Data Factory (or Synapse Pipelines) – orchestration
- Azure Data Lake Storage Gen2 – data lake
- Azure Databricks + Spark – data transformations
- Azure SQL / Synapse / Power BI – serving layer
- Delta Lake / Parquet – storage format
- GitHub – version control and documentation

## 4. Data model

- **Bronze layer**: Raw ingested data, minimal transformations.
- **Silver layer**: Cleaned, conformed tables (e.g., Customers, Orders, Products).
- **Gold layer**: Business-friendly tables or star schema (e.g., FactOrders + dimensions).
- Optionally add a small table or diagram showing main tables and key relationships.

## 5. Pipeline design

### 5.1 Ingestion (ADF / Pipelines)

- Ingests from: `<e.g., HTTP API, CSV files, database>`.
- Frequency: `<daily / hourly / near real-time>`.
- Incremental strategy: `<watermark column, last modified date, etc.>`.
- Key activities:
  - Metadata-driven pipeline?
  - Parameterised file paths?
  - Error handling / retries?

### 5.2 Transformations (Databricks)

- Notebooks for:
  - Bronze → Silver: cleaning, deduping, type casting.
  - Silver → Gold: joins, aggregations, SCDs, etc.
- Main techniques:
  - Partitioning strategy
  - Handling late/missing data
  - Data quality checks (if any)

## 6. Security & governance

- Authentication: Managed identity / service principal.
- Secrets management: Azure Key Vault for connection strings, keys.
- Access control:
  - RBAC roles used for storage and Databricks.
- Any governance notes (e.g., naming conventions, folder structure).

## 7. Monitoring & observability

- Monitoring tool: `<ADF Monitoring, Log Analytics, Alerts>`.
- What you monitor:
  - Pipeline failures / durations.
  - Data quality checks.
- How alerts are configured (email, webhook, etc.).

## 8. How to run this project

1. Prerequisites: Azure subscription, Databricks workspace, etc.
2. Clone this repo.
3. Create the necessary resources (list or link to a script if you add one).
4. Import notebooks into Databricks.
5. Configure ADF linked services and datasets.
6. Trigger pipeline run and validate outputs.

## 9. DP-203 skills demonstrated

- Design and implement data storage:
  - ADLS Gen2, partitioning, file formats.
- Develop data processing:
  - Spark transformations, batch pipelines.
- Secure, monitor, and optimize:
  - Key Vault, RBAC, monitoring/alerts.

## 10. Future improvements

- Add streaming / real-time ingestion.
- Add more data quality checks.
- Add CI/CD (e.g., GitHub Actions or Azure DevOps).

