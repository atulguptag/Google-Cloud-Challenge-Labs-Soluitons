## `Lab Name` - *Deploy, Scale, and Update Your Website on Google Kubernetes Engine [GSP663]*

## `Lab Link` - [*Click Here*](https://www.cloudskillsboost.google/focuses/10470?parent=catalog)

## Task 1. Create a GKE cluster

```
gcloud services enable container.googleapis.com

gcloud container clusters create fancy-cluster --num-nodes 3

gcloud compute instances list
```

## Task 2. Clone source repository

```cd ~
git clone https://github.com/googlecodelabs/monolith-to-microservices.git

cd ~/monolith-to-microservices
./setup.sh

nvm install --lts

cd ~/monolith-to-microservices/monolith
npm start
```

* Press `Ctrl+C`.

## Task 3. Create Docker container with Cloud Build

```
gcloud services enable cloudbuild.googleapis.com

cd ~/monolith-to-microservices/monolith
gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/monolith:1.0.0 .
```

## Task 4. Deploy container to GKE

```
kubectl create deployment monolith --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/monolith:1.0.0

kubectl get all
```

## Task 5. Expose GKE deployment

```
kubectl expose deployment monolith --type=LoadBalancer --port 80 --target-port 8080

kubectl get service
```

## Task 6. Scale GKE deployment
```
kubectl scale deployment monolith --replicas=3

kubectl get all
```

## Task 7. Make changes to the website

```
cd ~/monolith-to-microservices/react-app/src/pages/Home
mv index.js.new index.js

cat ~/monolith-to-microservices/react-app/src/pages/Home/index.js

cd ~/monolith-to-microservices/react-app
npm run build:monolith

cd ~/monolith-to-microservices/monolith

gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/monolith:2.0.0 .
```

## Task 8. Update website with zero downtime

```
kubectl set image deployment/monolith monolith=gcr.io/${GOOGLE_CLOUD_PROJECT}/monolith:2.0.0

kubectl get pods

npm start
```