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

- ## Issues I solved

### 1. Missing columns in Databricks schema
While working through the project, I found that the raw CSV contained two columns (`Product_Name` and `Year`) that were missing from the SQL table schema and downstream Data Factory import.

**What was happening**
- The source file had 12 columns, but the SQL table / imported schema only had 10.
- As a result, Databricks was reading an incomplete schema.

**How I fixed it**
- Updated the SQL table schema to include the missing columns.
- Re-imported the schema in Azure Data Factory.
- Re-ran the copy pipeline and verified the corrected schema in Databricks.

**What I learned**
- Schema mismatches can happen across source files, SQL staging tables, ADF datasets, and lake outputs.
- Fixing the schema upstream is better than patching it only in notebooks.

### 2. Incremental load / watermark pipeline issues
I ran into multiple issues while setting up the incremental pipeline using `water_table`, Lookup activities, and the `UpdateWatermarkTable` stored procedure.

**What was happening**
- Lookup outputs did not always match the expected structure from the tutorial (`value[0]` vs `firstRow`).
- The watermark table sometimes held a later `Date_ID` than expected, which changed the row count copied.
- Stored procedure parameter configuration in ADF initially failed because the parameter shape was incorrect.

**How I fixed it**
- Verified Lookup outputs directly in ADF debug runs.
- Aligned activity expressions with the actual output structure.
- Corrected the stored procedure parameter mapping for `UpdateWatermarkTable`.
- Validated the incremental copy by checking copied row counts and downstream data.

**What I learned**
- Incremental pipelines depend heavily on control tables and exact activity outputs.
- Small configuration differences in ADF can change pipeline behaviour significantly.
