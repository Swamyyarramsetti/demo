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




