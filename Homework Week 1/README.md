QUESTION 1: Run docker with the python:3.12.8 image in an interactive mode, use the entrypoint bash. What's the version of pip in the image?
 
bash command: docker run -it --entrypoint bash python:3.12.8
(command in python image running) root@3c4541350229:/# pip --version
Output/Answer => pip 24.3.1 from /usr/local/lib/python3.12/site-packages/pip (python 3.12)

QUESTION 3: During the period of October 1st 2019 (inclusive) and November 1st 2019 (exclusive), how many trips, respectively, happened:

Up to 1 mile,
In between 1 (exclusive) and 3 miles (inclusive)
In between 3 (exclusive) and 7 miles (inclusive),
In between 7 (exclusive) and 10 miles (inclusive),
Over 10 miles

SQL CODE:

SELECT 
     SUM(CASE WHEN trip_distance <= 1 THEN 1 ELSE 0 END) AS trips_up_to_1_mile,
     SUM(CASE WHEN trip_distance > 1 AND trip_distance <= 3 THEN 1 ELSE 0 END) AS trips_between_1_and_3_miles,
     SUM(CASE WHEN trip_distance > 3 AND trip_distance <= 7 THEN 1 ELSE 0 END) AS trips_between_3_and_7_miles,
     SUM(CASE WHEN trip_distance > 7 AND trip_distance <= 10 THEN 1 ELSE 0 END) AS trips_between_7_and_10_miles,
     SUM(CASE WHEN trip_distance > 10 THEN 1 ELSE 0 END) AS trips_over_10_miles
 FROM green_taxi_data
 WHERE lpep_dropoff_datetime >= '2019-10-01'
   AND lpep_dropoff_datetime < '2019-11-01';


QUESTION 4: Which was the pick up day with the longest trip distance? Use the pick up time for your calculations.
Tip: For every day, we only care about one single trip with the longest distance.

SQL CODE:

SELECT 
    DATE(lpep_pickup_datetime) AS pickup_date,
    MAX(trip_distance) AS longest_trip
FROM green_taxi_data
WHERE DATE(lpep_pickup_datetime) IN ('2019-10-11', '2019-10-24', '2019-10-26', '2019-10-31')
GROUP BY pickup_date
ORDER BY longest_trip DESC;

QUESTION 5: Which were the top pickup locations with over 13,000 in total_amount (across all trips) for 2019-10-18?
Consider only lpep_pickup_datetime when filtering by date.

SQL CODE:

SELECT
z."Zone",
SUM(total_amount) AS total_trip_revenue
FROM green_taxi_data AS g
LEFT JOIN zones AS z
ON g."PULocationID" = z."LocationID"
WHERE DATE(g.lpep_pickup_datetime) = '2019-10-18'
GROUP BY 1
HAVING SUM(g.total_amount) > 13000
ORDER BY 2 DESC;


QUESTION 6: For the passengers picked up in October 2019 in the zone named "East Harlem North" which was the drop off zone that had the largest tip?
Note: it's tip , not trip. We need the name of the zone, not the ID.

SQL CODE: 
SELECT 
    dz."Zone" AS dropoff_zone,  
    g.tip_amount                
FROM green_taxi_data AS g
LEFT JOIN zones AS pz
    ON g."PULocationID" = pz."LocationID"  
LEFT JOIN zones AS dz
    ON g."DOLocationID" = dz."LocationID"  
WHERE g.lpep_pickup_datetime >= '2019-10-01' 
  AND g.lpep_pickup_datetime < '2019-11-01'
  AND pz."Zone" = 'East Harlem North'      
ORDER BY g.tip_amount DESC                 
LIMIT 1;              

QUESTION 7: Which of the following sequences, respectively, describes the workflow for:

Downloading the provider plugins and setting up backend,
Generating proposed changes and auto-executing the plan
Remove all resources managed by terraform`

SHELL COMMAND: terraform init, terraform apply -auto-approve, terraform destroy