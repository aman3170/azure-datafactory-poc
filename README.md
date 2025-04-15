# Azure Data Factory PoC: Data Lake to SQL

A complete Proof of Concept for copying data from Azure Data Lake Storage Gen2 to Azure SQL Database using Azure Data Factory.

## Architecture Overview

```plaintext
Azure Data Lake Gen2 (CSV/Parquet) → ADF Pipeline → Azure SQL Database
```
---

<img width="797" alt="image" src="https://github.com/user-attachments/assets/8f2a5b12-0b3d-4179-b8ad-458b5a6e7510" />

<img width="886" alt="image" src="https://github.com/user-attachments/assets/e978a62c-de67-4bdb-9a2e-7400c15004b4" />


## Import via ADF Studio (UI)

Navigate to ADF Studio and open your Data Factory instance.

Go to Manage > ARM Template.

Click on Import ARM Template.

Choose the template.json and parameters.json file from this repo.

Follow the wizard to deploy the pipeline, linked services, and datasets.

Once deployed, go to Author tab to view and trigger the pipeline.




