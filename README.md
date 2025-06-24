# NYC Taxi Trip ETL & Analytics Pipeline
## Project Overview 
This project implements a Big Data ETL (Extract, Transform, Load) pipeline for analyzing New York City taxi trip data. The system is designed to handle large volumes of trip records and generate insights for operational planning, revenue tracking, and performance analysis.

## Technologies Used
- HDFS for raw data storage
- PySpark for data transformation and enrichment
- Hive for ad-hoc analytical queries
- Cassandra for fast retrieval of aggregated insights
- Bash for automation
- Pandas/Matplotlib (Optional) for data visualization

## Pipeline Phases

### Phase 1: Data Ingestion
- Creates the HDFS directory: /data/nyctaxi/raw/
- Uploads the CSV dataset to HDFS.

`bash`

    hdfs dfs -mkdir -p /data/nyctaxi/raw/
    hdfs dfs -put /home/swamyyarr70wedu/nyc_taxi_data.csv /data/nyctaxi/raw/
### Phase 2: Data Transformation
- Launches a PySpark script (transfor.py) that:
   - Loads raw CSV data.
   - Cleans and filters records:
       - Removes null/malformed rows.
       - Filters rows where trip_distance or passenger_count ≤ 0.
       - Ensures logical pickup_datetime < dropoff_datetime.
       - Drops trips longer than 24 hours or shorter than 60 seconds.
   - Adds a computed column: trip_duration = dropoff_time - pickup_time.
     
`bash`
    
    spark-submit transfor.py
### Phase 3: Data Storage
- Stores cleansed data as Parquet format in HDFS, partitioned by vendor_id.
- Creates an external Hive table pointing to the partitioned data for SQL querying.
  
`bash`
    
    hive -f external.hql
### Phase 4: Data Analysis
- Executes Spark SQL queries defined in analysis.py to:
     - Find top 5 vendors by revenue.
     - Analyze trip duration trends by hour of day.
     - Correlate trip distance with total fare.
  - Can export small datasets for visualization using pandas and matplotlib
  
`bash`

    spark-submit analysis.py

    
### Phase 5: Performance Tuning
- Optimizations implemented:
      - Parquet format used for efficient I/O.
      - Hive tables partitioned by vendor_id to reduce scan size.
      - Caching used on intermediate Spark DataFrames.
      - explain(True) used to examine and improve query plans.
      - Broadcast joins where applicable.


# How to Run
- Make sure the dataset is available at the correct local path, then execute the entire pipeline using:
  
`bash`
    
    bash script.sh
This will execute all five phases sequentially.

## File Structure
  
```bash
├── script.sh           # Master script to run all pipeline phases
├── transfor.py         # PySpark script for data cleansing & transformation
├── external.hql        # Hive DDL for creating external table
├── analysis.py         # PySpark SQL queries for data analysis
├── README.md           # Project documentation
└── nyc_taxi_data.csv   # Input dataset (CSV format)
```
## Sample Queries (Phase 4)
    Top 5 Vendors by Revenue
    Trip Duration Trend by Hour
    Correlation between Distance and Fare




