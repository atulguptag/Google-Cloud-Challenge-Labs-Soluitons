## `Lab Name` - *Engineer Data in Google Cloud: Challenge Lab [GSP327]*

## Lab Link - [Click Here](https://www.cloudskillsboost.google/focuses/12379?parent=catalog)

## [YouTube Solution Link](https://youtu.be/sK_gAUwzPQc)

## Task 1: Clean your training data

- Run this Query (change the values given in lab manual)

```
CREATE OR REPLACE TABLE
taxirides.<LAB_TABLE_NAME> AS
SELECT
(tolls_amount + fare_amount) AS <LAB_FATE_AMOUNT>,
pickup_datetime,
pickup_longitude AS pickuplon,
pickup_latitude AS pickuplat,
dropoff_longitude AS dropofflon,
dropoff_latitude AS dropofflat,
passenger_count AS passengers,
FROM
taxirides.historical_taxi_rides_raw
WHERE
RAND() < 0.001
AND trip_distance > 2               [Change_as_mention_in_lab]
AND fare_amount >= 3.0              [Change_as_mention_in_lab]
AND pickup_longitude > -78
AND pickup_longitude < -70
AND dropoff_longitude > -78
AND dropoff_longitude < -70
AND pickup_latitude > 37
AND pickup_latitude < 45
AND dropoff_latitude > 37
AND dropoff_latitude < 45
AND passenger_count > 2             [Change_as_mention_in_lab]
```

## Task 2: Create a BQML model

-  Run this Query (change the values given in lab manual)
```
CREATE OR REPLACE MODEL taxirides.<LAB_MODEL_NAME>
TRANSFORM(
* EXCEPT(pickup_datetime)
 
, ST_Distance(ST_GeogPoint(pickuplon, pickuplat), ST_GeogPoint(dropofflon, dropofflat)) AS euclidean
, CAST(EXTRACT(DAYOFWEEK FROM pickup_datetime) AS STRING) AS dayofweek
, CAST(EXTRACT(HOUR FROM pickup_datetime) AS STRING) AS hourofday
)
OPTIONS(input_label_cols=['<LAB_FATE_AMOUNT>'], model_type='linear_reg')
AS
 
SELECT * FROM taxirides.<LAB_TABLE_NAME>
```

## Task 3: Perform a batch prediction on new data

-  Run this Query (change the values given in lab manual)
```
CREATE OR REPLACE TABLE taxirides.2015_fare_amount_predictions
AS
SELECT * FROM ML.PREDICT(MODEL taxirides.<LAB_MODEL_NAME>,(
SELECT * FROM taxirides.report_prediction_data)
)
```

# Congratulations🎉! You're all done with this Challenge Lab.