# Retail-Data-Project

## Retail Data Pipeline (Azure + Databricks)

## Overview

This project implements an end-to-end data engineering pipeline using Azure services. It ingests retail transaction data from GitHub, stores it in Azure Data Lake, and processes it using Databricks following the Medallion Architecture (Bronze, Silver, Gold). Azure Data Factory also checks for updated CSV and runs pipleine to update notebook and tiers when a CSV is updated. 

## Architecture

GitHub (CSV)  
↓  
Azure Data Factory (Ingestion + Orchestration)  
↓  
Azure Data Lake Storage Gen2 (Raw / Bronze / Silver / Gold)  
↓  
Azure Databricks (PySpark Transformations)

## Tech Stack

- Azure Data Factory (ADF)
- Azure Data Lake Storage Gen2 (ADLS)
- Azure Databricks
- PySpark
- Delta Lake
- GitHub

## Pipeline Workflow

### 1. Data Ingestion (ADF)

- ADF pulls a CSV file from a GitHub repository
- Data is stored in the raw layer of ADLS

### 2. Data Processing (Databricks)

#### Bronze Layer

- Raw data ingested from ADLS
- Stored in Delta format
- Minimal transformations (schema applied)

#### Silver Layer

Data cleaning and transformation:

- Removed null values
- Filtered invalid records (e.g., negative quantity)
- Converted timestamps

Created new column:

- Revenue = Quantity × UnitPrice

#### Gold Layer

Aggregated data for analytics:

- Total revenue per customer
- Sorted highest-value customers
- Ready for reporting and BI tools

##  Project Structure

```text
Retail-Data-Project/
├── README.md
├── notebooks/
│   └── retail_pipeline.py
├── screenshots/
│   ├── adf_pipeline.png
│   ├── databricks_notebook.png
│   ├── bronze_silver_gold.png
│   └── pipeline_success.png
```

## Key Features

- End-to-end pipeline from ingestion to analytics
- Medallion architecture implementation
- Data stored in Delta Lake format
- Automated orchestration using ADF
- Integration between ADF and Databricks

## Security Considerations

- No credentials or secrets are stored in code
- Azure Storage keys should be managed using:
  - Databricks Secrets
  - Azure Key Vault

## Sample Output (Gold Layer)

- Top customers by revenue
- Cleaned and structured transaction data
- Aggregated metrics for analytics

## 🚀 Future Improvements

- Add incremental data loading
- Integrate Azure Synapse or Power BI for visualization
- Implement event-based triggers (instead of manual runs)
- Use Azure Key Vault for full secret management
