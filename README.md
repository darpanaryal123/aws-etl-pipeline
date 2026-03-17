# Cloud ETL Pipeline — AWS

> Automated ETL on 10GB+ daily datasets · AWS Glue · S3 · Redshift · 70% availability improvement

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat&logo=amazonaws&logoColor=white)
![Redshift](https://img.shields.io/badge/Redshift-8C4FFF?style=flat&logo=amazonaws&logoColor=white)

---

## Problem Statement

Manual data extraction and loading processes are error-prone, slow, and create data availability bottlenecks. This project automates the full ETL lifecycle on AWS — ingesting raw data from S3, transforming it via AWS Glue, and loading to Redshift for analytics — with built-in data quality checks and monitoring.

---

## Architecture

```
Raw Data Sources
       │
       ▼
  AWS S3 (Raw)
  (Partitioned by date/source)
       │
       ▼
  AWS Glue (ETL Jobs)
  ├── Schema validation
  ├── Data type enforcement
  ├── Null / outlier handling
  └── Business logic transforms
       │
       ▼
  AWS S3 (Processed)
  (Parquet format, partitioned)
       │
       ▼
  AWS Redshift
  (COPY command, columnar storage)
       │
       ▼
  Analytics / BI Layer
  (Power BI / Tableau connect here)
```

---

## Key Results

| Metric | Value |
|---|---|
| Daily data volume | 10GB+ |
| Data availability improvement | 70% vs manual process |
| Pipeline failure rate | < 2% in production |
| Data quality check coverage | 100% of ingested records |

---

## Tech Stack

| Component | Technology |
|---|---|
| Orchestration | AWS Glue (PySpark jobs) |
| Raw storage | AWS S3 (partitioned by date) |
| Processed storage | AWS S3 (Parquet format) |
| Data warehouse | AWS Redshift |
| Monitoring | AWS CloudWatch |
| Notifications | AWS SNS (failure alerts) |
| Language | Python / PySpark |
| SDK | Boto3 |

---

## Project Structure

```
aws-etl-pipeline/
├── glue_jobs/
│   ├── extract.py                # S3 extraction logic
│   ├── transform.py              # Data cleaning & transformation
│   └── load.py                   # Redshift COPY and upsert logic
├── data_quality/
│   └── checks.py                 # Schema validation, null checks, range checks
├── monitoring/
│   └── cloudwatch_alerts.py      # CloudWatch metric publishing
├── utils/
│   ├── s3_utils.py               # S3 helper functions
│   └── redshift_utils.py         # Redshift connection helpers
├── config/
│   └── pipeline_config.yaml      # Environment configuration
├── tests/
│   └── test_transforms.py        # Unit tests for transform logic
├── requirements.txt
└── README.md
```

---

## Data Quality Checks

Every record passes through automated checks before loading to Redshift:

- Schema validation — column names and types match expected contract
- Null enforcement — required fields must be non-null
- Range checks — numeric fields within expected bounds
- Referential integrity — foreign key relationships validated
- Duplicate detection — deduplication before load

Records failing checks are routed to a quarantine S3 prefix for investigation.

---

## Setup & Run

```bash
git clone https://github.com/darpanaryal/aws-etl-pipeline.git
cd aws-etl-pipeline

# Configure AWS credentials
aws configure

# Install dependencies
pip install -r requirements.txt

# Run locally (requires AWS credentials with S3/Glue/Redshift access)
python glue_jobs/extract.py --date 2026-01-01
python glue_jobs/transform.py --date 2026-01-01
python glue_jobs/load.py --date 2026-01-01
```

---

## What I Learned

- Designing idempotent ETL jobs that can safely re-run without data duplication
- Optimizing Redshift COPY commands and table distribution styles for query performance
- Implementing data quality frameworks at scale with automated quarantine routing
- Building CloudWatch dashboards for pipeline observability and on-call alerting

---

## Author

**Darpan Aryal** · [LinkedIn](https://linkedin.com/in/darpanaryal) · [aryaldarpan20@gmail.com](mailto:aryaldarpan20@gmail.com)
