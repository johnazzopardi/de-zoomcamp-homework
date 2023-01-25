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

```
$ docker run -i \
  --env POSTGRES_USER="root" \
  --env POSTGRES_PASSWORD="root" \
  --env POSTGRES_DB="ny_taxi" \
  --volume /home/john/data-engineering-zoomcamp/cohorts/2023/ny_taxi_postgres_data:/var/lib/postgresql/data \
  --publish 5432:5432 \
  postgres:13
```

Connect to postgres with pgcli tool
`$ pgcli -h localhost -p 5432 -u root -d ny_taxi`

## Question 3. Count records 

```
SELECT COUNT(*) FROM green_trip_data 
WHERE CAST(lpep_pickup_datetime AS DATE) = '2019-01-15' 
  AND CAST(lpep_dropoff_datetime AS DATE) = '2019-01-15'
```

Answer: 20530