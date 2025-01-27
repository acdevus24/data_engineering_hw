## Local Setup for Terraform and GCP

### Homework 1
## Question 1. Understanding docker first run 

adsouza@ADSOUZA:~$ docker run -it python:3.12.8 bash
root@f59a029b0771:/# pip -V
pip 24.3.1 from /usr/local/lib/python3.12/site-packages/pip (python 3.12)

**Answer**: pip 24.3.1

## Question 2. Understanding Docker networking and docker-compose

**Answer**: db:5432

## Question 3. Trip Segmentation Count

**Answer**: 104,802; 198,924; 109,603; 27,678; 35,189

```sql
select count(1) from green_taxi_trips 
where
lpep_pickup_datetime >= '2019-10-01 00:00:00'
and lpep_dropoff_datetime < '2019-11-01 00:00:00'
and trip_distance <=1.0;--104802

select count(1) from green_taxi_trips 
where
lpep_pickup_datetime >= '2019-10-01 00:00:00'
and lpep_dropoff_datetime < '2019-11-01 00:00:00'
and trip_distance > 1.0
and trip_distance <=3.0;--198924

select count(1) from green_taxi_trips 
where
lpep_pickup_datetime >= '2019-10-01 00:00:00'
and lpep_dropoff_datetime < '2019-11-01 00:00:00'
and trip_distance > 3.0
and trip_distance <=7.0;--109603

select count(1) from green_taxi_trips 
where
lpep_pickup_datetime >= '2019-10-01 00:00:00'
and lpep_dropoff_datetime < '2019-11-01 00:00:00'
and trip_distance > 7.0
and trip_distance <=10.0;--27678

select count(1) from green_taxi_trips 
where
lpep_pickup_datetime >= '2019-10-01 00:00:00'
and lpep_dropoff_datetime < '2019-11-01 00:00:00'
and trip_distance > 10.0;--35189
```

## Question 4. Longest trip for each day

**Answer**: 2019-10-31

```sql
select lpep_pickup_datetime, max(trip_distance) as max_distance
from green_taxi_trips
group by 1
order by 2 desc
limit 1;
```

## Question 5. Three biggest pickup zones

**Answer**: East Harlem North, East Harlem South, Morningside Heights

```sql
select gt."PULocationID", lkp."Zone", sum(gt.total_amount) as total_amount
from green_taxi_trips gt
left join taxi_zone_lookup lkp
on gt."PULocationID" = lkp."LocationID"
where gt.lpep_pickup_datetime::date = '2019-10-18'
group by 1 ,2
having sum(gt.total_amount)>13000
order by 3 desc
;
```

## Question 6. Largest tip

**Answer**: JFK Airport

```sql
select 
gt."PULocationID"
, puz."Zone" as pickupzone
, gt."DOLocationID"
, doz."Zone" as dropoffzone
, max(gt.tip_amount) as max_tip
from green_taxi_trips gt
left join taxi_zone_lookup puz
on gt."PULocationID" = puz."LocationID"
left join taxi_zone_lookup doz
on gt."DOLocationID" = doz."LocationID"
where puz."Zone" = 'East Harlem North'
and date_part('month', gt.lpep_pickup_datetime) = '10'
group by 1,2,3,4
order by 5 desc
LIMIT 1
;
```

## Question 7. Terraform Workflow

**Answer**: terraform init, terraform apply -auto-approve, terraform destroy