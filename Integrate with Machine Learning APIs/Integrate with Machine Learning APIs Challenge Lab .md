## `Lab Name` - *Integrate with Machine Learning APIs: Challenge Lab [GSP329]*

## `Lab Link` - [*Click Here*](https://www.cloudskillsboost.google/focuses/12704?parent=catalog)

<!-- ## [YouTube Solution Link]() -->

Run the below commands in the Cloud Shell Terminal.


```
export LANGUAGE=

export LOCAL=

export BIGQUERY_ROLE=

export CLOUD_STORAGE_ROLE=
```

## Task 1. Configure a service account to access the Machine Learning APIs, BigQuery, and Cloud Storage

```
gcloud iam service-accounts create sample-sa

gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID --member=serviceAccount:sample-sa@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com --role=$BIGQUERY_ROLE

gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID --member=serviceAccount:sample-sa@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com --role=$CLOUD_STORAGE_ROLE
```

## Task 2. Create and download a credential file for your service account

```
gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID --member=serviceAccount:sample-sa@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com --role=roles/serviceusage.serviceUsageConsumer

gcloud iam service-accounts keys create sample-sa-key.json --iam-account sample-sa@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com

export GOOGLE_APPLICATION_CREDENTIALS=${PWD}/sample-sa-key.json
```

## Task 3. Modify the Python script to extract text from image files

```
wget https://raw.githubusercontent.com/atulguptag/Google-Cloud-Challenge-Labs-Soluitons/main/Integrate%20with%20Machine%20Learning%20APIs/analyze-images-v2.py

sed -i "s/'en'/'${LOCAL}'/g" analyze-images-v2.py
```

## Task 4. Modify the Python script to translate the text using the Translation API

```
python3 analyze-images-v2.py $DEVSHELL_PROJECT_ID $DEVSHELL_PROJECT_ID
```

## Task 5. Identify the most common language used in the signs in the dataset

```
bq query --use_legacy_sql=false "SELECT locale,COUNT(locale) as lcount FROM image_classification_dataset.image_text_detail GROUP BY locale ORDER BY lcount DESC"
```

# Congratulations🎉! You're all done with this Challenge Lab.