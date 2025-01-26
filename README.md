# Zoomcamp2025

## Question 1. Understanding docker first run
First we create a Dockerfile with the right requirements

Then we navigate via terminal to the right directory
    cd 'Module 1 Homework: Docker & SQL'

Then we build & run this DockerFile
    docker build -t question:one .
    docker run -it question:one

Inside the container we run the following bash command
    pip --version

pip 24.3.1 from /usr/local/lib/python3.12/site-packages/pip (python 3.12)


## Question 2. Understanding Docker networking and docker-compose
Hostname: db
Port: 5432

## Question 3. Trip Segmentation Count
// Answer: 104802
SELECT COUNT(*) AS trip_count
FROM green_taxi_trips
WHERE lpep_pickup_datetime >= '2019-10-01' 
  AND lpep_dropoff_datetime < '2019-11-01'
  AND trip_distance <= 1;

// Answer: 198924
SELECT COUNT(*) AS trip_count
FROM green_taxi_trips
WHERE lpep_pickup_datetime >= '2019-10-01' 
  AND lpep_dropoff_datetime < '2019-11-01'
  AND trip_distance > 1 
  AND trip_distance <= 3;

// Answer: 109603
SELECT COUNT(*) AS trip_count
FROM green_taxi_trips
WHERE lpep_pickup_datetime >= '2019-10-01' 
  AND lpep_dropoff_datetime < '2019-11-01'
  AND trip_distance > 3 
  AND trip_distance <= 7;

// Answer: 27678
SELECT COUNT(*) AS trip_count
FROM green_taxi_trips
WHERE lpep_pickup_datetime >= '2019-10-01' 
  AND lpep_dropoff_datetime < '2019-11-01'
  AND trip_distance > 7 
  AND trip_distance <= 10;

// Answer: 35189
SELECT COUNT(*) AS trip_count
FROM green_taxi_trips
WHERE lpep_pickup_datetime >= '2019-10-01' 
  AND lpep_dropoff_datetime < '2019-11-01'
  AND trip_distance > 10;


# Question 4. Longest trip for each day
// Answer: 515.89
SELECT 
    DATE(lpep_pickup_datetime) AS pickup_day,
    MAX(trip_distance) AS longest_trip_distance
FROM 
    green_taxi_trips
GROUP BY 
    DATE(lpep_pickup_datetime)
ORDER BY 
    longest_trip_distance DESC
LIMIT 1;

# Question 5. Three biggest pickup zones
// Answer: East Harlem North, East Harlem South, Morningside Heights
SELECT 
    z."Zone" AS pickup_zone,
    SUM(gt.total_amount) AS total_amount_collected
FROM 
    green_taxi_trips gt
JOIN 
    zones z ON gt."PULocationID" = z."LocationID"
WHERE 
    DATE(gt.lpep_pickup_datetime) = '2019-10-18'
GROUP BY 
    z."Zone"
HAVING 
    SUM(gt.total_amount) > 13000
ORDER BY 
    total_amount_collected DESC;


# Question 6. Largest tip
// Answer: JFK Airport
SELECT 
    z."Zone" AS dropoff_zone,
    MAX(gt.tip_amount) AS max_tip
FROM 
    green_taxi_trips gt
JOIN 
    zones z ON gt."DOLocationID" = z."LocationID"
WHERE 
    gt."PULocationID" = (SELECT "LocationID" FROM zones WHERE "Zone" = 'East Harlem North')
    AND DATE(gt.lpep_pickup_datetime) BETWEEN '2019-10-01' AND '2019-10-31'
GROUP BY 
    z."Zone"
ORDER BY 
    max_tip DESC
LIMIT 1;


# Question 7. Terraform Workflow
terraform init
terraform apply -auto-approve
terraform destroy