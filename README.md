# DataTalks_Course
 Solutions to assignments
--data-engineering-zoomcamp
--this contains projects and assignments carried out in this study


--sql QUERIES FOR HOMEWORK 1
SELECT * FROM public.green_tripdata
LIMIT 100;

-- Up to 1 mile
SELECT COUNT(*) as TripLessThan1
FROM green_tripdata
WHERE (lpep_pickup_datetime BETWEEN '2019-10-01' AND '2019-11-01')
AND trip_distance <= 1;

-- In between 1(exclusive) and 3 miles (inclusive)
SELECT COUNT(*)
FROM green_tripdata
WHERE (lpep_pickup_datetime BETWEEN '2019-10-01' AND '2019-11-01')
AND trip_distance >1 AND trip_distance <= 3;

-- In between 3 (exclusive) and 7 miles (inclusive)
SELECT COUNT(*)
FROM green_tripdata
WHERE (lpep_pickup_datetime BETWEEN '2019-10-01' AND '2019-11-01')
AND trip_distance >3 AND trip_distance <= 7;

--In between 7 (exclusive) and 10 miles (inclusive)
SELECT COUNT(*)
FROM green_tripdata
WHERE (lpep_pickup_datetime BETWEEN '2019-10-01' AND '2019-11-01')
AND trip_distance >7 AND trip_distance <= 10;

-- Over 10 miles
SELECT COUNT(*)
FROM green_tripdata
WHERE (lpep_pickup_datetime BETWEEN '2019-10-01' AND '2019-11-01')
AND trip_distance > 10;

/*Which was the pick up day with the longest trip distance?
Use the pick up time for your calculations.*/
SELECT lpep_pickup_datetime

FROM green_tripdata
WHERE trip_distance =

(SELECT MAX(trip_distance)
FROM green_tripdata);

SELECT *
FROM taxi_zone_lookup;

SELECT "DOLocationID"
FROM green_tripdata
LIMIT 5;

/*Which were the top pickup locations 
with over 13,000 in total_amount
(across all trips) for 2019-10-18?*/
SELECT t."Zone",DATE(lpep_pickup_datetime), SUM(g."total_amount") as AMOUNT
FROM green_tripdata as g
JOIN taxi_zone_lookup as t
ON g."PULocationID" = t."LocationID"
WHERE DATE(lpep_pickup_datetime) = '2019-10-18'
group by 1,2
HAVING SUM(g."total_amount") > 13000
ORDER BY 3 DESC
;


/*For the passengers picked up in October 2019 
in the zone named "East Harlem North" 
which was the drop off zone that had the largest tip?*/
SELECT t_drop."Zone", MAX(g.tip_amount)
FROM green_tripdata as g
JOIN taxi_zone_lookup as t_pick
ON g."PULocationID" = t_pick."LocationID"
JOIN taxi_zone_lookup as t_drop
ON g."DOLocationID" = t_drop."LocationID"
WHERE t_pick."Zone" = 'East Harlem North'
GROUP BY 1
ORDER BY 2 DESC
limit 1;