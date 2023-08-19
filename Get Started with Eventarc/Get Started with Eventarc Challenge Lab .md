## `Lab Name` - *Get Started with Eventarc: Challenge Lab [ARC118]*

## `Lab Link` - [Click Here](https://www.cloudskillsboost.google/focuses/63244?parent=catalog)


Run the following commands in the Cloud Shell Terminal.


```
export REGION=
```

```
gcloud config set run/region "REGION"

export SERVICE_NAME=pubsub-events

export IMAGE_NAME="gcr.io/cloudrun/hello"
```


## Task 1. Create a Pub/Sub topic

```
gcloud pubsub topics create $DEVSHELL_PROJECT_ID-topic

gcloud  pubsub subscriptions create --topic $DEVSHELL_PROJECT_ID-topic $DEVSHELL_PROJECT_ID-topic-sub
```


## Task 2. Create a Cloud Run sink

```
gcloud run deploy ${SERVICE_NAME} \
  --image ${IMAGE_NAME} \
  --allow-unauthenticated \
  --max-instances=3
  --region=$REGION
```

* If asking to Enble APIs, then press `y`.

## Task 3. Create and test a Pub/Sub event trigger using Eventarc

```
gcloud eventarc triggers create pubsub-events-trigger \
  --location=$REGION \
  --destination-run-service=pubsub-events \
  --destination-run-region=$REGION \
  --transport-topic=$DEVSHELL_PROJECT_ID-topic \
  --event-filters="type=google.cloud.pubsub.topic.v1.messagePublished"

gcloud pubsub topics publish $DEVSHELL_PROJECT_ID-topic --message="Hello there"
```
* If asking to Enble APIs, then press `y`.


# Congratulations🎉! You're all done with this Challenge Lab.