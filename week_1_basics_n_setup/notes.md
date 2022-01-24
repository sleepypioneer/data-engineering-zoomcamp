# Week One Notes

Data source: https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page

### Running database locally in Docker

```bash
docker-compose up
```

Run in detached mode:

```bash
docker-compose up -d
```

Shutting it down:

```bash
docker-compose down
```

***note***

As my code is saved on a share drive with the Windows partition, I had to change the mouning path to the following:

```
-v /e/zoomcamp/...:/var/lib/postgresql/data
```

root@localhost:ny_taxi> \dt
+--------+-------------------+-------+-------+
| Schema | Name              | Type  | Owner |
|--------+-------------------+-------+-------|
| public | yellow_taxi_trips | table | root  |
+--------+-------------------+-------+-------+

root@localhost:ny_taxi> \d yellow_taxi_trips
+-----------------------+-----------------------------+-----------+
| Column                | Type                        | Modifiers |
|-----------------------+-----------------------------+-----------|
| index                 | bigint                      |           |
| VendorID              | bigint                      |           |
| tpep_pickup_datetime  | timestamp without time zone |           |
| tpep_dropoff_datetime | timestamp without time zone |           |
| passenger_count       | bigint                      |           |
| trip_distance         | double precision            |           |
| RatecodeID            | bigint                      |           |
| store_and_fwd_flag    | text                        |           |
| PULocationID          | bigint                      |           |
| DOLocationID          | bigint                      |           |
| payment_type          | bigint                      |           |
| fare_amount           | double precision            |           |
| extra                 | double precision            |           |
| mta_tax               | double precision            |           |
| tip_amount            | double precision            |           |
| tolls_amount          | double precision            |           |
| improvement_surcharge | double precision            |           |
| total_amount          | double precision            |           |
| congestion_surcharge  | double precision            |           |
+-----------------------+-----------------------------+-----------+


### Home work

#### Question 1

369.0.0

#### Question 2

google_bigquery_dataset.dataset: Creating...
google_storage_bucket.data-lake-bucket: Creating...
google_storage_bucket.data-lake-bucket: Creation complete after 1s [id=dtc_data_lake_speedy-elf-338916]
google_bigquery_dataset.dataset: Creation complete after 1s [id=projects/speedy-elf-338916/datasets/trips_data_all]

#### Question 3

```sql
SELECT COUNT(*) FROM yellow_taxi_trips WHERE date(tpe
 p_pickup_datetime) = '2021-01-15';


+-------+
| count |
|-------|
| 48810 |
+-------+
```

#### Question 4

```sql
SELECT date(tpep_pickup_datetime) FROM yellow_taxi_trips WHERE EXTRACT(MONTH FROM  tpe
 p_pickup_datetime) = 1 AND tip_amount = ( SELECT MAX (tip_amount) FROM yellow_taxi_trips);


+------------+
| date       |
|------------|
| 2021-01-20 |
+------------+
```

#### Question 5

```sql
SELECT "DOLocationID", COUNT(*) AS count FROM yellow_taxi_trips WHERE da
 te(tpep_pickup_datetime) = '2021-01-14' GROUP BY "DOLocationID" ORDER BY count DESC, "DOLocatio
 nID" LIMIT 1;
+--------------+-------+
| DOLocationID | count |
|--------------+-------|
| 236          | 2936  |
+--------------+-------+
```

Upper East Side North

#### Question 6

```sql
SELECT "PULocationID",
   "DOLocationID", MAX(total_amount) FROM yellow_taxi_trips GROUP BY "PULocationID", "DOLocation
 ID" ORDER BY count DESC LIMIT 1;
+--------------+--------------+-------+
| PULocationID | DOLocationID | count |
|--------------+--------------+-------|
| 237          | 236          | 10133 |



SELECT * FROM (SELECT "PULocationID",
   "DOLocationID", total_amount FROM yellow_taxi_trips GROUP BY "PULocationID", "DOLocationID",
 "total_amount")AS foo WHERE total_amount = (SELECT MAX(tip_amount) FROM yellow_taxi_trips);


SELECT MAX(total_amount) FROM (SELECT "PULocationID",
   "DOLocationID", total_amount FROM yellow_taxi_trips GROUP BY "PULocationID", "DOLocationID",
 "total_amount")AS foo;

```