## `Lab Name` - *Get Started with Pub/Sub: Challenge Lab [ARC113]*

## `Lab Link` - [*Click Here*](https://www.cloudskillsboost.google/focuses/63246?parent=catalog)

<!-- ## [YouTube Solution Link]() -->

Run the below commands in the Cloud Shell Terminal.

```
export REGION=
```
* You can find the REGION/LOCATION in the Task 2 (On the Lab Page).

## Task 1. Set up Cloud Pub/Sub

```
gcloud pubsub topics create $DEVSHELL_PROJECT_ID-cron-topic

gcloud pubsub subscriptions create $DEVSHELL_PROJECT_ID-cron-sub --topic $DEVSHELL_PROJECT_ID-cron-topic
```

## Task 2. Create a Cloud Scheduler job

```
gcloud services enable cloudscheduler.googleapis.com

gcloud scheduler jobs create pubsub $DEVSHELL_PROJECT_ID-cron-scheduler-job --schedule="* * * * *" --location $REGION --topic $DEVSHELL_PROJECT_ID-cron-topic --message-body="hello cron!"
```

## Task 3. Verify the results in Cloud Pub/Sub

```
gcloud pubsub subscriptions pull $DEVSHELL_PROJECT_ID-cron-sub --limit 5
```

# Congratulations🎉! You're all done with this Challenge Lab.