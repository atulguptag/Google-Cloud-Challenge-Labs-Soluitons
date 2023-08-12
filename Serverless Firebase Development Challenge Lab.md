## Lab Name - Serverless Firebase Development: Challenge Lab
## Lab Link - [CLICH HERE](https://www.cloudskillsboost.google/focuses/14677?parent=catalog)

## [YouTube Solution Link](https://www.youtube.com/watch?v=-TefmOhEEPA)
### Start by executing the following commands:

* Run the below Commands 

```cmd
gcloud config set project $(gcloud projects list --format='value(PROJECT_ID)' --filter='qwiklabs-gcp')
git clone https://github.com/rosera/pet-theory.git
```
## Task 1. Create a Firestore database


* We have to Create a Firestore database.
* Click on Search > Firestore > Choose Native Store > Add location Nam5 (United States)

## Task - 2: Firestore Database Populate

```cmd
cd pet-theory/lab06/firebase-import-csv/solution
npm install
node index.js netflix_titles_original.csv
```

## Task - 3: Cloud Build Rest API Staging

```cmd
cd ~/pet-theory/lab06/firebase-rest-api/solution-01
npm install
gcloud builds submit --tag gcr.io/$GOOGLE_CLOUD_PROJECT/rest-api:0.1
gcloud beta run deploy <Dataset_Service_Name> --image gcr.io/$GOOGLE_CLOUD_PROJECT/rest-api:0.1 --allow-unauthenticated
```

* Choose `29` and `us-central1`


## Task - 4: Cloud Build Rest API Production

```cmd
cd ~/pet-theory/lab06/firebase-rest-api/solution-02
npm install
gcloud builds submit --tag gcr.io/$GOOGLE_CLOUD_PROJECT/rest-api:0.2
gcloud beta run deploy <Dataset_Service_Name> --image gcr.io/$GOOGLE_CLOUD_PROJECT/rest-api:0.2 --allow-unauthenticated
```

### Goto `Cloud Run` and Click `Netflix-Dataset-Service` then copy *the url*.

```cmd
SERVICE_URL=<copy url from your netflix-dataset-service>
curl -X GET $SERVICE_URL/2019
```

## Task - 5: Cloud Build Frontend Staging

```cmd
cd ~/pet-theory/lab06/firebase-frontend/public
nano app.js # comment line 3 and uncomment line 4, insert your netflix-dataset-service url
npm install
cd ~/pet-theory/lab06/firebase-frontend
gcloud builds submit --tag gcr.io/$GOOGLE_CLOUD_PROJECT/frontend-staging:0.1
gcloud beta run deploy <Cloud Run Service Name> --image gcr.io/$GOOGLE_CLOUD_PROJECT/frontend-staging:0.1
```

* Choose `29` and `us-central1`


## Task - 6: Cloud Build Frontend Production

```cmd
gcloud builds submit --tag gcr.io/$GOOGLE_CLOUD_PROJECT/frontend-production:0.1
gcloud beta run deploy <Cloud Run Service Name> --image gcr.io/$GOOGLE_CLOUD_PROJECT/frontend-production:0.1
```

* Choose `29` and `us-central1`

# Congratulations🎉! You're all done with this Challenge lab.