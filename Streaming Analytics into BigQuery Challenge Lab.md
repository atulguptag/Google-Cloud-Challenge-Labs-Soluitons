## `Lab Name` - *Streaming Analytics into BigQuery: Challenge Lab [ARC106]*

## `Lab Link` - [Click Here](https://www.cloudskillsboost.google/focuses/61948?parent=catalog)

<!-- ## [YouTube Solution Link]() -->

Run the following commands in the Cloud Shell Terminal.

```
export BUCKET_NAME=

export BIGQUERY_DATASET_NAME=

export TABLE_NAME=

export TOPIC_NAME=

export ZONE=
```

## Task 1. Create a Cloud Storage bucket

```
gcloud services enable dataflow

gsutil mb gs://$BUCKET_NAME/
```

## Task 2. Create a BigQuery dataset and table

```
bq mk $BIGQUERY_DATASET_NAME --zone=$ZONE

bq mk \
--schema data:string -t $BIGQUERY_DATASET_NAME.$TABLE_NAME
```

## Task 3. Set up a Pub/Sub topic
 
```
gcloud pubsub topics create $TOPIC_NAME

gcloud pubsub subscriptions create $TOPIC_NAME-sub --topic $TOPIC_NAME
```

## Task 4. Run a Dataflow pipeline to stream data from a Pub/Sub topic to BigQuery

* Follow the *Video Instruction Carefully*.

## Task 5. Publish a test message to the topic and validate data in BigQuery

* Go to *Navigation Menu* > *Bigquery* > Click on `+Create Query`.

* Run this query - 

```
SELECT * FROM '<DATASET_NAME>.<TABLE_NAME>'
```

# Congratulations 🎉! You're all done with this Challenge Lab.