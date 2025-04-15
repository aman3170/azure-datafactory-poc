# Azure Data Factory PoC: Data Lake to SQL

A complete Proof of Concept for copying data from Azure Data Lake Storage Gen2 to Azure SQL Database using Azure Data Factory. This project includes scripts, templates, and a detailed setup guide.

## 🧱 Architecture Overview

```plaintext
Azure Data Lake Gen2 (CSV/Parquet) → ADF Pipeline → Azure SQL Database
```
---

## ✅ Step-by-Step Guide

### 1. Provision Azure SQL Database

```bash
# Create an Azure SQL Server
az sql server create \
  --name myazuresqlserver \
  --resource-group adf-poc-rg \
  --location eastus \
  --admin-user sqladmin \
  --admin-password 'StrongPassword@123'

# Create a SQL Database
az sql db create \
  --name employee-db \
  --server myazuresqlserver \
  --resource-group adf-poc-rg \
  --service-objective S0

# Allow public access for PoC (secure this later)
az sql server firewall-rule create \
  --resource-group adf-poc-rg \
  --server myazuresqlserver \
  --name AllowAllAzureIPs \
  --start-ip-address 0.0.0.0 \
  --end-ip-address 0.0.0.0
```

### 2. Create SQL Login and Table

> Connect to SQL Server using Azure Cloud Shell or SSMS.

```sql
-- Run in master DB
CREATE LOGIN adf_user WITH PASSWORD = 'StrongPassword@123';

-- Switch to employee-db
USE employee-db;
CREATE USER adf_user FROM LOGIN adf_user;

-- Create table
CREATE TABLE dbo.Employees (
    EmployeeID INT,
    FirstName NVARCHAR(50),
    LastName NVARCHAR(50),
    Email NVARCHAR(100),
    Department NVARCHAR(50)
);

-- Grant permissions
GRANT INSERT, SELECT ON dbo.Employees TO adf_user;
```

### 3. Create Azure Data Factory

```bash
# Create ADF instance
az datafactory create \
  --resource-group adf-poc-rg \
  --factory-name my-adf-factory \
  --location eastus
```

> Access ADF Studio from Azure Portal to build the pipeline.

### 4. Create Linked Services in ADF Studio

#### a. Azure Data Lake Storage Gen2

- Go to **Manage > Linked Services > + New**
- Choose **Azure Data Lake Storage Gen2**
- Select authentication method:
  - Account Key / SAS Token / Managed Identity
- Test connection and Save

#### b. Azure SQL Database

- Go to **Manage > Linked Services > + New**
- Choose **Azure SQL Database**
- Use SQL Authentication (adf_user / password)
- Test connection and Save

### 5. Create Datasets

#### Source Dataset – ADLS
- Type: `DelimitedText` or `Parquet`
- Linked Service: ADLS Gen2
- File path: `/input-data/employees.csv`

#### Sink Dataset – Azure SQL
- Type: `Azure SQL Table`
- Table name: `dbo.Employees`

### 6. Build and Deploy Pipeline

1. Go to **Author > + New pipeline**
2. Drag **Copy Data** activity to canvas
3. Set:
   - Source: ADLS dataset
   - Sink: Azure SQL dataset
4. (Optional) Configure column mapping
5. Debug and Trigger the pipeline

### 7. Monitor Pipeline Execution

- Go to **Monitor** tab in ADF
- View pipeline run status and logs

> Validate data in SQL:

```sql
SELECT * FROM dbo.Employees;
```

---

✅ Done! 🎉 Your data is now flowing from **Azure Data Lake** to **Azure SQL** via **ADF pipeline**.

---

## 💡 Contribution
Feel free to open issues or submit PRs if you'd like to contribute or enhance this PoC.

---

## 📄 License
MIT License
