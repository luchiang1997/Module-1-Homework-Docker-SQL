# Module-1-Homework-Docker-SQL

## Question 1. Understanding docker first run
Run docker with the python:3.12.8 image in an interactive mode, use the entrypoint bash.
What's the version of pip in the image?
•	24.3.1
•	24.2.1
•	23.3.1
•	23.2.1

Answer: 
docker run -it python:3.12.8 bash
pip --version
•	24.3.1
-----------------------------------------------------------------------------------------------------------------------------------------
Question 2. Understanding Docker networking and docker-compose
Given the following docker-compose.yaml, what is the hostname and port that pgadmin should use to connect to the postgres database?
Answer:
db:5433
-----------------------------------------------------------------------------------------------------------------------------------------
## Question 3. Trip Segmentation Count
During the period of October 1st 2019 (inclusive) and November 1st 2019 (exclusive), how many trips, respectively, happened:

Up to 1 mile
In between 1 (exclusive) and 3 miles (inclusive),
In between 3 (exclusive) and 7 miles (inclusive),
In between 7 (exclusive) and 10 miles (inclusive),
Over 10 miles

Answer:
SELECT COUNT(1)
FROM green_trip_data
--WHERE trip_distance <= 1
--WHERE trip_distance BETWEEN 1.01 AND 3
--WHERE trip_distance BETWEEN 3.01 AND 7
--WHERE trip_distance BETWEEN 7.01 AND 10
--WHERE trip_distance > 10
AND CAST(lpep_pickup_datetime AS DATE) BETWEEN '2019-10-01' AND '2019-10-31'
AND CAST(lpep_dropoff_datetime AS DATE) BETWEEN '2019-10-01' AND '2019-10-31';
104,802; 198,924; 109,603; 27,678; 35,189
-----------------------------------------------------------------------------------------------------------------------------------------
## Question 4. Longest trip for each day
Which was the pick up day with the longest trip distance? Use the pick up time for your calculations.

Tip: For every day, we only care about one single trip with the longest distance.

Answer:
SELECT max(trip_distance), lpep_pickup_datetime
  FROM green_trip_data
 GROUP BY lpep_pickup_datetime
 ORDER BY max(trip_distance) DESC;
2019-10-31
-----------------------------------------------------------------------------------------------------------------------------------------
## Question 5. Three biggest pickup zones
Which were the top pickup locations with over 13,000 in total_amount (across all trips) for 2019-10-18?

Consider only lpep_pickup_datetime when filtering by date.

Answer:
SELECT 
  CAST(lpep_dropoff_datetime AS DATE) AS dropoff_date, 
  SUM(total_amount) AS ttl, 
  z."Zone"
FROM green_trip_data g
JOIN zones z ON g."PULocationID" = z."LocationID"
WHERE CAST(lpep_dropoff_datetime AS DATE) = '2019-10-18'
GROUP BY CAST(lpep_dropoff_datetime AS DATE), z."Zone"
HAVING SUM(total_amount) > 13000;
East Harlem North, Morningside Heights
-----------------------------------------------------------------------------------------------------------------------------------------
## Question 6. Largest tip
For the passengers picked up in October 2019 in the zone name "East Harlem North" which was the drop off zone that had the largest tip?

Note: it's tip , not trip

We need the name of the zone, not the ID.

Answer:
SELECT 
  MAX(tip_amount) AS MA, 
  g."DOLocationID"
FROM green_trip_data g
JOIN zones z ON g."PULocationID" = z."LocationID"
WHERE 
  CAST(lpep_dropoff_datetime AS DATE) BETWEEN '2019-10-01' AND '2019-10-31'
  AND z."Zone" = 'East Harlem North'
GROUP BY g."DOLocationID"
ORDER BY MA DESC;
SELECT z."Zone"
  FROM green_trip_data g
  JOIN zones z ON g."DOLocationID" = z."LocationID"
 WHERE g."DOLocationID" = 132
 JFK Airport
-----------------------------------------------------------------------------------------------------------------------------------------
