## `Lab Name` - *Cloud Functions: 3 Ways: Challenge Lab [ARC104]*

## `Lab Link` - [*Click Here*](https://www.cloudskillsboost.google/focuses/61974?parent=catalog)

<!-- ## [YouTube Solution Link]() -->

Run the below commands in the Cloud Shell Terminal.

* Enabling an APIs.

``` 
gcloud services enable \
  artifactregistry.googleapis.com \
  cloudfunctions.googleapis.com \
  cloudbuild.googleapis.com \
  eventarc.googleapis.com \
  run.googleapis.com \
  logging.googleapis.com \
  pubsub.googleapis.com
```

* Store values in a variables.

```
export REGION=

export FUNCTION_NAME=

export HTTP_FUNCTION=

export BUCKET="gs://$DEVSHELL_PROJECT_ID"
```

## Task 1. Create a Cloud Storage bucket

```
PROJECT_NUMBER=$(gcloud projects list --filter="project_id:$DEVSHELL_PROJECT_ID" --format='value(project_number)')

SERVICE_ACCOUNT=$(gsutil kms serviceaccount -p $PROJECT_NUMBER)

gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID \
  --member serviceAccount:$SERVICE_ACCOUNT \
  --role roles/pubsub.publisher

gsutil mb -l $REGION gs://$DEVSHELL_PROJECT_ID
```

## Task 2. Create, deploy, and test a Cloud Storage function (2nd gen)

```
mkdir ~/$FUNCTION_NAME && cd $_

touch index.js && touch package.json

cat > index.js <<EOF
const functions = require('@google-cloud/functions-framework');
functions.cloudEvent('$FUNCTION_NAME', (cloudevent) => {
  console.log('A new event in your Cloud Storage bucket has been logged!');
  console.log(cloudevent);
});
EOF

cat > package.json <<EOF
{
  "name": "nodejs-functions-gen2-codelab",
  "version": "0.0.1",
  "main": "index.js",
  "dependencies": {
    "@google-cloud/functions-framework": "^2.0.0"
  }
}
EOF
```

* Creating Cloud Storage Function.

```
gcloud functions deploy $FUNCTION_NAME \
  --gen2 \
  --runtime nodejs16 \
  --entry-point $FUNCTION_NAME \
  --source . \
  --region $REGION \
  --trigger-bucket $BUCKET \
  --trigger-location $REGION \
  --max-instances 2
```

## Task 3. Create and deploy a HTTP function (2nd gen) with minimum instances

```
cd ..

mkdir ~/HTTP_FUNCTION && cd $_

touch index.js && touch package.json

cat > index.js <<EOF
const functions = require('@google-cloud/functions-framework');
functions.http('$HTTP_FUNCTION', (req, res) => {
  res.status(200).send('subscribe to quikclab');
});
EOF

cat > package.json <<EOF
{
  "name": "nodejs-functions-gen2-codelab",
  "version": "0.0.1",
  "main": "index.js",
  "dependencies": {
    "@google-cloud/functions-framework": "^2.0.0"
  }
}
EOF
```

* Creating HTTP FUNCTION.

```
gcloud functions deploy $HTTP_FUNCTION \
  --gen2 \
  --runtime nodejs16 \
  --entry-point $HTTP_FUNCTION \
  --source . \
  --region $REGION \
  --trigger-http \
  --timeout 600s \
  --max-instances 2 \
  --min-instances 1
```

# Congratulations🎉! You're done with this Challenge Lab.