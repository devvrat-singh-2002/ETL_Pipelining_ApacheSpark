# ETL Pipeline for Sales Data Processing

**Tools**: Python, PySpark, Pandas, AWS S3, Databricks  
**Domain**: Data Engineering | ETL Pipelines | Batch Processing  

---

## Project Overview  
An **ETL pipeline** built to ingest, validate, transform, and load sales, customer, and product data from AWS S3 into structured datasets. All transformations (filtering, joins, calculations) are implemented using **Python and PySpark**, with error handling to ensure data integrity. Outputs are analytics-ready Parquet/Delta tables.  

---

## Key Features  
1. **Data Ingestion**: Fetch raw data (CSV/JSON) from AWS S3 using Python (boto3) or PySpark.  
2. **Validation & Error Handling**:  
   - Validate records (e.g., check missing `customer_id`, valid `pincode`).  
   - Route invalid data to `s3://error-directory` for auditing.  
3. **Python/PySpark Transformations**:  
   - Compute derived fields (e.g., `total_cost = price × quantity`).  
   - Join datasets (customer, sales, product) using PySpark DataFrames or Pandas.  
   - Enforce schema alignment (e.g., `customer`, `product`, `sales` tables).  
4. **Output**: Write processed data to S3 as Parquet/Delta files for downstream use.  

---

## Data Schema  
![Database Schema](database_schema.drawio.png)  
- **Core Datasets**:  
  - `customer`: Attributes like `customer_id`, `address`, `joining_date`.  
  - `sales`: Transaction links to `product`, `store`, and `sales_team` with `total_cost`.  
  - `product`: Pricing history (`current_price`, `old_price`, `expiry_date`).  
  - `store`: Location details, reviews, and manager info.  

---

## Workflow  
![Architecture](architecture.png)  
1. **Extract**: Pull raw data from AWS S3 into PySpark DataFrames/Python objects.  
2. **Validate**:  
   - Apply data quality checks (Python/PySpark logic).  
   - Split data into valid/invalid partitions.  
3. **Transform**:  
   - Calculate metrics (e.g., `total_cost`) using Python UDFs or PySpark.  
   - Join datasets via PySpark DataFrame operations (e.g., `join()`, `merge()`).  
4. **Load**: Save structured data to S3 (Parquet/Delta format).  
5. **Error Handling**: Invalid records archived in `s3://error-directory`.  

---

## Setup & Usage  
### Prerequisites  
- Python 3.8+, PySpark 3.0+, AWS CLI, boto3.  
- AWS S3 credentials configured.  

### Steps  
1. **Clone Repo**:  
   ```bash  
   git clone https://github.com/devvrat-singh-2002/ETL_Pipelining_ApacheSpark.git

2. ```bash
   pip install -r requirements.txt  # Includes PySpark, pandas, boto3

3. ```bash
   spark-submit --master local[*] etl_pipeline.py \  
   --input_path s3://raw-data \  
   --output_path s3://processed-data \  
   --error_path s3://errors  
