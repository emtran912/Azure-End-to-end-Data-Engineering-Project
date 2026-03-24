# Azure-End-to-end-Data-Engineering-Project
A project to familarise myself with the Azure data engineering ecosystem

## 1. Project overview

- **Goal**: Build a scalable pipeline to ingest and transform  data.
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
