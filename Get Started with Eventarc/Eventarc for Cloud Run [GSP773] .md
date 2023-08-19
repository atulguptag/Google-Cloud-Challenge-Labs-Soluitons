## `Lab Name` - *Eventarc for Cloud Run [GSP773]*

## `Lab Link` - [Click Here](https://www.cloudskillsboost.google/focuses/15657?parent=catalog)


Run the following commands in the Cloud Shell Terminal.

<!-- gcloud config set project <PROJECT_ID>

gcloud config set run/region <REGION>

gcloud config set run/platform managed

gcloud config set eventarc/location $REGION -->

```
export PROJECT_NUMBER="$(gcloud projects list \
  --filter=$(gcloud config get-value project) \
  --format='value(PROJECT_NUMBER)')"

gcloud projects add-iam-policy-binding $(gcloud config get-value project) \
  --member=serviceAccount:${PROJECT_NUMBER}-compute@developer.gserviceaccount.com \
  --role='roles/eventarc.admin'

gcloud beta eventarc attributes types list

gcloud beta eventarc attributes types describe \
  google.cloud.pubsub.topic.v1.messagePublished

export SERVICE_NAME=event-display

export IMAGE_NAME="gcr.io/cloudrun/hello"

gcloud run deploy ${SERVICE_NAME} \
  --image ${IMAGE_NAME} \
  --allow-unauthenticated \
  --max-instances=3

gcloud beta eventarc attributes types describe \
  google.cloud.pubsub.topic.v1.messagePublished

gcloud beta eventarc triggers create trigger-pubsub \
  --destination-run-service=${SERVICE_NAME} \
  --matching-criteria="type=google.cloud.pubsub.topic.v1.messagePublished"

export TOPIC_ID=$(gcloud eventarc triggers describe trigger-pubsub \
  --format='value(transport.pubsub.topic)')

gcloud eventarc triggers list

gcloud pubsub topics publish ${TOPIC_ID} --message="Hello there"

export BUCKET_NAME=$(gcloud config get-value project)-cr-bucket

gsutil mb -p $(gcloud config get-value project) \
  -l $(gcloud config get-value run/region) \
  gs://${BUCKET_NAME}/
```

### Enable Audit Logs

* From the *Navigation menu*, select *IAM & Admin* > `Audit Logs`.

* In the list of services, check the box for Google Cloud Storage.

* On the right hand side, click the `LOG TYPE tab`. Admin Write is selected by default, make sure you also select `Admin Read`, `Data Read`, `Data Write` and then click `Save`.

```
echo "Hello World" > random.txt

gsutil cp random.txt gs://${BUCKET_NAME}/random.txt
```


* In the Cloud Console, go to *Navigation menu* > *Logging* > `Logs Explorer`.

* Under *Resource*, choose `GCS Bucket` > [Bucket Name] > `Location` then choose `your bucket` and `its location`. 

* Click `Add`.

* Click on `Run Query`.

```
gcloud beta eventarc attributes types describe google.cloud.audit.log.v1.written

gcloud beta eventarc triggers create trigger-auditlog \
--destination-run-service=${SERVICE_NAME} \
--matching-criteria="type=google.cloud.audit.log.v1.written" \
--matching-criteria="serviceName=storage.googleapis.com" \
--matching-criteria="methodName=storage.objects.create" \
--service-account=${PROJECT_NUMBER}-compute@developer.gserviceaccount.com

gcloud eventarc triggers list

gsutil cp random.txt gs://${BUCKET_NAME}/random.txt
```
* The above command will take some time to get the score in the Task 6.

# Congratulations🎉! You're all done with this lab.