# Zoomcamp2025 - Module 3

## Create external table
CREATE OR REPLACE EXTERNAL TABLE `module-3-data-warehouse.taxi_data_eu10.external_yellow_tripdata_2024`
OPTIONS (
  format = 'PARQUET',
  uris = ['gs://module3-dez-sss/yellow_tripdata_2024-*.parquet']
);

## Create regular table external table
CREATE OR REPLACE TABLE `module-3-data-warehouse.taxi_data_eu10.your_regular_table_name`
AS SELECT * FROM `module-3-data-warehouse.taxi_data_eu10.external_yellow_tripdata_2024`;


## Question 1: What is count of records for the 2024 Yellow Taxi Data?
SELECT COUNT(*) 
FROM `module-3-data-warehouse.taxi_data_eu10.your_regular_table_name`;

Answer:
20332093


## Question 2: Write a query to count the distinct number of PULocationIDs for the entire dataset on both the tables.
What is the estimated amount of data that will be read when this query is executed on the External Table and the Table?

SELECT COUNT(DISTINCT PULocationID) 
FROM `module-3-data-warehouse.taxi_data_eu10.external_yellow_tripdata_2024`;

SELECT COUNT(DISTINCT PULocationID) 
FROM `module-3-data-warehouse.taxi_data_eu10.your_regular_table_name`;

Answer:
0 MB for the External Table and 155.12 MB for the Materialized Table


## Question 3: Write a query to retrieve the PULocationID from the table (not the external table) in BigQuery. Now write a query to retrieve the PULocationID and DOLocationID on the same table. Why are the estimated number of Bytes different?
SELECT PULocationID 
FROM `module-3-data-warehouse.taxi_data_eu10.your_regular_table_name`;

SELECT PULocationID, DOLocationID 
FROM `module-3-data-warehouse.taxi_data_eu10.your_regular_table_name`;

Answer:
BigQuery is a columnar database, and it only scans the specific columns requested in the query. Querying two columns (PULocationID, DOLocationID) requires reading more data than querying one column (PULocationID), leading to a higher estimated number of bytes processed.


## Question 4: How many records have a fare_amount of 0?
Answer:
8333


## Question 5: What is the best strategy to make an optimized table in Big Query if your query will always filter based on tpep_dropoff_datetime and order the results by VendorID (Create a new table with this strategy)
CREATE OR REPLACE TABLE `module-3-data-warehouse.taxi_data_eu10.optimized_yellow_tripdata_2024`
PARTITION BY DATE(tpep_dropoff_datetime)
CLUSTER BY VendorID AS (
  SELECT * 
  FROM `module-3-data-warehouse.taxi_data_eu10.your_regular_table_name`
);

Answer:
Partition by tpep_dropoff_datetime and Cluster on VendorID


## Question 6: Write a query to retrieve the distinct VendorIDs between tpep_dropoff_datetime 2024-03-01 and 2024-03-15 (inclusive)
Use the materialized table you created earlier in your from clause and note the estimated bytes. Now change the table in the from clause to the partitioned table you created for question 5 and note the estimated bytes processed. What are these values?

Choose the answer which most closely matches.


SELECT DISTINCT VendorID
FROM `module-3-data-warehouse.taxi_data_eu10.your_regular_table_name`
WHERE tpep_dropoff_datetime BETWEEN '2024-03-01' AND '2024-03-15';

SELECT DISTINCT VendorID
FROM `module-3-data-warehouse.taxi_data_eu10.optimized_yellow_tripdata_2024`
WHERE tpep_dropoff_datetime BETWEEN '2024-03-01' AND '2024-03-15';

Answer:
310.24 MB for non-partitioned table and 26.84 MB for the partitioned table


## Question 7: Where is the data stored in the External Table you created?
Answer:
GCP Bucket


## Question 8: It is best practice in Big Query to always cluster your data:
Answer:
False


## Question 9: No Points: Write a SELECT count(*) query FROM the materialized table you created. How many bytes does it estimate will be read? Why?
SELECT COUNT(*) 
FROM `module-3-data-warehouse.taxi_data_eu10.your_regular_table_name`;

Answer:
The answer is 0. It is a very efficient query, because it does not have to read contents of the rows; only count the number of rows.