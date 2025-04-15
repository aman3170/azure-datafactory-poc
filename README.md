# Azure Data Factory (ADF) PoC: Copy Data from Azure Data Lake to Azure SQL Database

This Proof of Concept (PoC) demonstrates how to use **Azure Data Factory (ADF)** to copy data from **Azure Data Lake Storage Gen2 (ADLS)** into an **Azure SQL Database** using SQL Authentication. It includes setup steps, pipeline creation, and troubleshooting common issues.

---

## Objective

- Read data from Azure Data Lake Storage Gen2 (CSV/Parquet)
- Load it into a target table in Azure SQL Database
- Use ADF pipelines for orchestrated data movement
- Document and resolve common errors during setup

---

## Architecture Overview

```plaintext
Azure Data Lake Gen2 (CSV/Parquet) → ADF Pipeline → Azure SQL Database
