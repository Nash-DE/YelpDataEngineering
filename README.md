# Yelp Data Engineering Challenge Solution

## Understanding the Challenge
The objective of this challenge was to design and implement a robust data engineering solution using the Yelp dataset. The solution should:

1. **Ingest the Yelp dataset** into a BI system.
2. **Model the data** into an optimized schema to support reporting and analytics.
3. **Transform the data** using PySpark to meet the requirements of business intelligence (BI) and analytical queries.
4. Identify **"Rising Star" businesses** using SQL based on specific criteria:
   - At least 10 reviews in the past year.
   - Average rating in the past year is at least 1 star higher than the average rating before the past year.
5. Ensure the solution is scalable, efficient, and easy to monitor.

---

## Architecture of the Solution
The architecture was designed to ensure scalability, performance, and ease of use for BI teams. The main components include:

### 1. **Data Ingestion**
- **Source**: JSON files from AWS S3 bucket (`de-yelp-dataset-raw`).
- **Ingestion Method**: PySpark was used to load the data, parse the JSON files, and write them to S3 in Parquet format.

### 2. **Data Modeling**
- **Schema Design**: A star schema was implemented for optimized querying and reporting:
  - **Fact Table**: Reviews (capturing metrics like ratings and dates).
  - **Dimension Tables**: Businesses, Users, Photos, Check-ins.

### 3. **Data Transformation**
- PySpark pipelines were built to clean, aggregate, and prepare the data for analysis. Aggregations included average ratings, review counts, and time-based filtering for "past year" and "before past year."

### 4. **Rising Stars Identification**
- SQL query implemented in PySpark filtered businesses based on the criteria and returned their IDs and names.

---

## Components and Technology Used
### **Tech Stack**
1. **AWS S3**: Storage for raw and processed data.
2. **Databricks**: Scalable platform for processing large datasets using PySpark.
3. **PySpark**: Framework for parallel data processing, cleaning, and transformations.
4. **Parquet**: Optimized file format for efficient storage and querying.
5. **SQL**: For implementing the "Rising Stars" business logic.

### **Why These Technologies?**
- **AWS S3**: Highly available, cost-efficient, and scalable storage.
- **Databricks**: Simplifies PySpark development and offers robust cluster management.
- **PySpark**: Efficient for large-scale data transformations.
- **Parquet**: Reduces storage costs and speeds up query performance.

---

## Understanding the Dataset
The Yelp dataset includes the following key entities:

1. **Businesses**:
   - Attributes: `business_id`, `name`, `categories`, `location`.
2. **Reviews**:
   - Attributes: `review_id`, `user_id`, `business_id`, `rating`, `date`.
3. **Users**:
   - Attributes: `user_id`, `name`, `review_count`, `average_stars`.
4. **Photos**:
   - Attributes: `photo_id`, `business_id`, `label`.
5. **Check-ins**:
   - Attributes: `business_id`, `time`.

Exploratory data analysis revealed:
- Businesses have varying numbers of reviews and ratings.
- Dates span multiple years, requiring careful filtering for "past year" and "before past year" logic.

---

## Steps Taken

### **1. Data Ingestion**
- **Raw Data**: JSON files ingested from S3 into Databricks.
- **Transformation**: Loaded JSON files were written to Parquet format for efficient processing.
- **Output**: Data was stored in S3 (`parquet-data-for-bi`).

### **2. Data Modeling**
- Designed a star schema to optimize BI queries.
- Fact table and dimension tables were defined and loaded from Parquet files.

### **3. Data Transformation**
- Implemented PySpark pipelines to:
  - Filter reviews by "past year" and "before past year."
  - Aggregate metrics like `average rating` and `review count` for each business.
  - Join fact and dimension tables to prepare a comprehensive dataset.

### **4. Rising Stars Identification**
- SQL logic applied to:
  - Identify businesses with `>= 10 reviews` in the past year.
  - Filter businesses where `avg_rating_past_year - avg_rating_before_past_year >= 1`.
- Returned results with `business_id` and `name`.

### **5. Scalability and Optimization**
- **Scalability**:
  - Parquet format reduces I/O overhead.
  - Databricks cluster configured with autoscaling for peak loads.
- **Monitoring**:
  - Spark UI and logs were used to benchmark and debug pipelines.
- **Optimizations**:
  - Used partitions for time-based filtering.
  - Cached intermediate results to avoid redundant computations.

---

## Optimization Decisions
1. **File Format**: Parquet chosen for its compression and query efficiency.
2. **Cluster Autoscaling**: Ensured resources scaled dynamically during processing peaks.
3. **Partitioning**: Data was partitioned by `date` for faster filtering.
4. **Joins**: Broadcast joins used where applicable to optimize joins between large and small datasets.

---

## End-to-End Solution Summary
1. **Data Ingestion**:
   - Raw JSON data loaded from S3.
   - Transformed and written to Parquet in a star schema.
2. **Data Transformation**:
   - Metrics aggregated for "past year" and "before past year."
   - Pipelines built using PySpark to prepare data for BI needs.
3. **Rising Stars Identification**:
   - SQL query implemented to identify businesses meeting criteria.
   - Results stored in Parquet for BI consumption.






