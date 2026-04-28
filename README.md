# Retail-Data-Project

## Retail Data Pipeline (Azure + Databricks)

## Overview

This project implements an end-to-end data engineering pipeline using Azure services. It ingests retail transaction data from GitHub, stores it in Azure Data Lake, and processes it using Databricks following the Medallion Architecture (Bronze, Silver, Gold). Azure Data Factory also checks for updated CSV and runs the pipeline to refresh the notebook and tiered tables when the CSV changes. Gold-tier data is connected to Power BI for reporting; an interactive dashboard was in progress (see `Dashboard/README.md`).

## Architecture

End-to-end flow as implemented:

GitHub (CSV source)  
↓  
Azure Data Factory (ingestion + orchestration)  
↓  
Azure Data Lake Storage Gen2 (Raw → Bronze → Silver → Gold)  
↓  
Azure Databricks (PySpark notebook: medallion transforms)  
↓  
Power BI (reports on gold-tier data)

What lives in this repository: sample source data (`CSV/`), Databricks notebooks (`Databricks Notebooks/`), ADF pipeline artifacts (`Azure Data Factory Pipeline/`), ADLS screenshots (`Azure Data Lake/`), Databricks job run evidence (`Databricks Jobs+Runs/`), and dashboard notes plus screenshot (`Dashboard/`).

## Tech Stack

- Azure Data Factory (ADF)
- Azure Data Lake Storage Gen2 (ADLS)
- Azure Databricks
- PySpark
- Delta Lake
- Power BI
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

## Project Structure

```text
Retail-Data-Project/
├── README.md
├── CSV/
│   └── online_retail.csv
├── Databricks Notebooks/
│   ├── tiers_retail.ipynb
│   ├── logging.ipynb
│   └── quality_checks.ipynb
├── Azure Data Factory Pipeline/
│   ├── retail_pipeline1.zip
│   ├── Screenshot 2026-04-25 234555.png
│   └── trigger+alert update.png
├── Azure Data Lake/
│   └── Data Lake.png
├── Databricks Jobs+Runs/
│   └── Job1.png
└── Dashboard/
    ├── README.md
    └── Dashboard.png
```

## Key Features

- End-to-end pipeline from ingestion to analytics
- Medallion architecture implementation
- Data stored in Delta Lake format
- Automated orchestration using ADF
- Integration between ADF, Databricks, and Power BI for reporting

## Security Considerations

- No credentials or secrets are stored in code
- Azure Storage keys should be managed using:
  - Databricks Secrets
  - Azure Key Vault

## Sample Output (Gold Layer)

- Top customers by revenue
- Cleaned and structured transaction data
- Aggregated metrics for analytics

## Future improvements

- Add incremental data loading
- Expand Power BI dashboards (rebuild interactive dashboard; see `Dashboard/README.md`)
- Implement event-based triggers (instead of manual runs)
- Use Azure Key Vault for full secret management
