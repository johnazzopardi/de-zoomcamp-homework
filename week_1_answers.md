## Week 1 Homework: Answers for John Azzopardi

## Question 1. Knowing docker tags
```
$ docker build --help
--iidfile string          Write the image ID to the file
```

## Question 2. Understanding docker first run 

```
$ docker run --entrypoint bash --interactive python:3.9
$ pip list 
```

Number of packages: 3

## Prep for remaining questions

Setup docker

Create docker network for pgAdmin
`$ docker network create pg-network`

```
$ docker run -i \
  --env POSTGRES_USER="root" \
  --env POSTGRES_PASSWORD="root" \
  --env POSTGRES_DB="ny_taxi" \
  --volume /home/john/data-engineering-zoomcamp/cohorts/2023/ny_taxi_postgres_data:/var/lib/postgresql/data \
  --publish 5432:5432 \
  --network="pg-network" \
  --name="pg-database" \
  postgres:13
```

Connect to postgres with pgcli tool
`$ pgcli -h localhost -p 5432 -u root -d ny_taxi`

```
$ docker run -i \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD="root" \
  --network="pg-network" \
  --name="pgadmin" \
  -p 8080:80 dpage/pgadmin4
```

## Question 3. Count records 

```
SELECT COUNT(*) FROM green_trip_data 
WHERE CAST(lpep_pickup_datetime AS DATE) = '2019-01-15' 
  AND CAST(lpep_dropoff_datetime AS DATE) = '2019-01-15'
```

Answer: 20530

## Question 4. Largest trip for each day

```
SELECT 
CAST(lpep_pickup_datetime AS DATE) AS pickup_date,
trip_distance,
fare_amount
FROM green_trip_data
ORDER BY trip_distance DESC
LIMIT 1
```

Answer: 2019-01-15

## Question 5. The number of passengers

```
SELECT 
  passenger_count,
  COUNT(index) AS num_trips
FROM green_trip_data
WHERE CAST(lpep_pickup_datetime AS DATE) = '2019-01-01'
GROUP BY passenger_count
```

Answer: 
  2 passengers: 1282
  3 passengers: 254 

## Question 6. Largest tip

```
SELECT
 puz."Zone" AS pick_up_zone,
 doz."Zone" AS drop_off_zone,
 MAX(tip_amount) AS max_tip_amount
FROM green_trip_data t
LEFT JOIN taxi_zone_lookup puz ON t."PULocationID" = puz."LocationID"
LEFT JOIN taxi_zone_lookup doz ON t."DOLocationID" = doz."LocationID"
WHERE puz."Zone" = 'Astoria'
GROUP BY 1,2
ORDER BY max_tip_amount DESC
```

Answer: Long Island City/Queens Plaza
