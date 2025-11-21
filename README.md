# Azure_Project

# Azure Spotify Data Engineering Project

End-to-end modern data engineering project built on **Azure Data Factory**, **Azure SQL Database**, **Azure Databricks**, **Delta Lake**, and **Unity Catalog**.

The goal of this project is to simulate a real-world analytics pipeline for a Spotify-style streaming company: ingesting operational data incrementally, landing it in a Lakehouse, transforming it into a **dimensional model (star schema)**, and making it ready for BI / ML workloads.

---

##  Key Highlights

- **Cloud-native stack**: Azure Data Factory, Azure SQL DB, ADLS Gen2, Azure Databricks.
- **Incremental ingestion with backfilling** using ADF parameters, variables, and dynamic expressions.
- **Reusable ingestion pattern** driven by a `ForEach` loop → same pipeline for multiple tables.
- **Bronze–Silver architecture** with **Autoloader (cloudFiles)** + **Delta streaming**.
- **Dimensional model** for Spotify analytics:
  - `DimUser`, `DimArtist`, `DimTrack`, `DimDate`, `FactStream`
- **Python utilities package (`utils`)** to centralize and reuse PySpark transformations.
- Designed with **production-style patterns**: checkpoints, schema evolution, CDC, and standard naming conventions.

---

##  Architecture Overview

**Source → Orchestration → Lakehouse → Transformations → Dimensional Model**

1. **Source System**
   - Azure SQL Database (simulated operational DB)
   - Tables with CDC / watermark columns (e.g., `updated_at`)

2. **Orchestration & Ingestion**
   - Azure Data Factory:
     - `incremental_ingestion` pipeline
     - `incremental_loop` pipeline (wrapper with `ForEach`)
   - Data is copied from Azure SQL DB into **ADLS Gen2 (Bronze)** in **Parquet** & **JSON** formats.

3. **Lakehouse & Transformations**
   - Azure Databricks (workspaces, clusters, Unity Catalog)
   - Autoloader streaming jobs read Bronze → write **Silver Delta tables**
   - Reusable PySpark transformation utilities (drop columns, dedupe, etc.)

4. **Dimensional Model**
   - Silver tables organized as:
     - Dimension tables: `DimUser`, `DimArtist`, `DimTrack`, `DimDate`
     - Fact table: `FactStream`
   - Optimized for analytics and downstream BI / ML.

---

##  Tech Stack

- **Compute & Orchestration**
  - Azure Data Factory
  - Azure Databricks (Unity Catalog, Jobs)

- **Storage**
  - Azure Data Lake Storage Gen2 (Bronze / Silver containers)
  - Delta Lake tables

- **Data Processing**
  - PySpark / Spark Structured Streaming
  - Databricks Autoloader (cloudFiles)

- **Data Modeling**
  - Dimensional modeling (Star schema)
  - Slowly Changing Dimensions concepts (CDC, `updated_at` logic)

- **Code & Version Control**
  - Databricks Repos + GitHub
  - Databricks project structure (`src`, `utils`, notebooks, `databricks.yml`)

---


