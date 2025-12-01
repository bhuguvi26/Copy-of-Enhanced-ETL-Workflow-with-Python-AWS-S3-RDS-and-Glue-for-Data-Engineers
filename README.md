# Copy-of-Enhanced-ETL-Workflow-with-Python-AWS-S3-RDS-and-Glue-for-Data-Engineers
AWS S3 → RDS ETL Pipeline (Colab Implementation)
1. Introduction

This project implements a production-grade ETL (Extract → Transform → Load) pipeline using AWS cloud services.

The pipeline extracts CSV, JSON, and XML files from an S3 bucket, performs data transformation, and loads the output into:

AWS S3 (under /transformed/)

AWS RDS (MySQL/PostgreSQL table)

The pipeline supports secure credential handling, multi-file processing, and centralized logging stored in S3. This simulates a real-world data engineering scenario.

2. Objectives

Extract Data

Supports CSV, JSON, XML

Reads all files dynamically from S3 /raw/ prefix

Transform Data

Normalizes column names

Converts:

Height: inches → meters

Weight: pounds → kilograms

Combines multiple source files into a unified dataset

Load Data

Uploads transformed CSV to S3 /transformed/

Inserts records into AWS RDS table using SQLAlchemy

Logging

Logs every ETL step

Uploads logs to S3 /logs/

3. Architecture
          ┌───────────────┐
          │   Raw Data    │ CSV / JSON / XML
          └───────┬───────┘
                  │ Extract
                  ▼
          ┌───────────────┐
          │  ETL Pipeline │ (Python / Colab)
          │ - Normalize   │
          │ - Transform   │
          └───────┬───────┘
                  │ Load
         ┌────────┴─────────┐
         │                  │
 ┌───────────────┐   ┌─────────────┐
 │   AWS S3      │   │ AWS RDS DB  │
 │ transformed/  │   │ etl_merged_data │
 └───────────────┘   └─────────────┘
                  ▲
                  │ Logs
           ┌───────────────┐
           │ AWS S3 /logs/ │
           └───────────────┘

4. Tools & Technologies
Tool/Service	Purpose
Python 3	ETL development
Pandas	Data manipulation
Boto3	AWS S3 interactions
SQLAlchemy + PyMySQL	Connect & load data to RDS
AWS S3	Data storage (raw + transformed)
AWS RDS	Persistent relational table storage
Logging	ETL monitoring & debugging
lxml / xml.etree.ElementTree	XML parsing
5. Project Folder Structure
ETL_Cloud_Project/
│
├── etl_pipeline.ipynb       # Colab notebook with ETL code
├── README.md                # Project documentation
└── assets/
    └── screenshots/         # Optional: S3, RDS, ETL outputs

6. Configuration (Colab)

Edit before running:

AWS_REGION = "ap-southeast-2"
AWS_ACCESS_KEY = "<your_access_key>"
AWS_SECRET_KEY = "<your_secret_key>"

S3_BUCKET = "bhuguvibucket"
RAW_PREFIX = "raw/"
TRANSFORMED_PREFIX = "transformed/"
LOGS_PREFIX = "logs/"

RDS_HOST = "<your_rds_endpoint>"
RDS_PORT = 3306
RDS_USER = "admin"
RDS_PASSWORD = "<your_db_password>"
RDS_DB = "bhuguvidb"
TARGET_TABLE = "etl_merged_data"


Note: Never commit credentials to GitHub. Store secrets securely or set as environment variables.

7. Steps to Run

Install dependencies:

!pip install -q boto3 pandas sqlalchemy pymysql lxml


Set AWS credentials (environment variables or Colab config).

Run ETL pipeline:

run_pipeline()


The pipeline will:

Connect to S3 and list files under /raw/

Extract CSV, JSON, and XML

Transform the dataset

Upload transformed CSV to /transformed/

Load data into RDS table etl_merged_data

Upload ETL logs to /logs/

8. Sample Log Output
INFO | ETL job starting
INFO | Connected to S3 bucket: bhuguvibucket
INFO | Extracting CSV: raw/source1.csv
INFO | Extracting JSON: raw/source1.json
INFO | Extracting XML: raw/source1.xml
INFO | Combined dataframe rows: 39
INFO | Transformation complete
INFO | Uploaded transformed CSV to S3
INFO | Loaded 39 rows into RDS table
INFO | Logs uploaded to s3://bhuguvibucket/logs/
Pipeline completed successfully
Transformed CSV: s3://bhuguvibucket/transformed/transformed_data.csv
RDS table: bhuguvidb.etl_merged_data

9. Common Issues

RDS Connection Failed

Verify endpoint, port, and security group allow access from Colab

Double-check credentials

S3 Access Issues

Confirm bucket name, region, and IAM permissions

Data Parsing Errors

Ensure CSV, JSON, XML files are well-formed

10. Deliverables

etl_pipeline.ipynb — ETL script

README.md — Documentation

S3 bucket with:

/raw/ → Input files

/transformed/ → Transformed CSV

/logs/ → ETL run logs

RDS table: etl_merged_data

11. Future Improvements

Add incremental ETL to process only new files

Integrate AWS Glue for schema inference

Schedule pipeline with AWS Lambda / Step Functions

Implement data quality validation
