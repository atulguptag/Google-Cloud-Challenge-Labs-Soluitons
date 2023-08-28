## `Lab Name` - *Migrating a Monolithic Website to Microservices on Google Kubernetes Engine [GSP699]*

## `Lab Link` - [*Click Here*](https://www.cloudskillsboost.google/focuses/11953?parent=catalog)

## [YouTube Solution Link](https://youtu.be/OT4jdnRzMMw)
Run the following commands in the Cloud Shell Terminal.

## Task 1. Clone the source repository

```
gcloud config set compute/zone <ZONE>
```

```
cd ~
git clone https://github.com/googlecodelabs/monolith-to-microservices.git
cd ~/monolith-to-microservices
./setup.sh
```

## Task 2. Create a GKE cluster

```
gcloud services enable container.googleapis.com

gcloud container clusters create fancy-cluster --num-nodes 3 --machine-type=e2-standard-4

gcloud compute instances list
```

## Task 3. Deploy the existing monolith

```
cd ~/monolith-to-microservices
./deploy-monolith.sh

kubectl get service monolith
```

## Task 4. Migrate orders to a microservice

```
cd ~/monolith-to-microservices/microservices/src/orders
gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/orders:1.0.0 .

kubectl create deployment orders --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/orders:1.0.0

kubectl get all

kubectl expose deployment orders --type=LoadBalancer --port 80 --target-port 8081

kubectl get service orders

cd ~/monolith-to-microservices/react-app
nano .env.monolith
```

* Replace the `REACT_APP_ORDERS_URL` to the new format 

```
REACT_APP_ORDERS_URL=http://<ORDERS_IP_ADDRESS>/api/orders
REACT_APP_PRODUCTS_URL=/service/products
```

* Press `CTRL+O, press ENTER, then CTRL+X` to save the file in the nano editor.

```
npm run build:monolith

cd ~/monolith-to-microservices/monolith
gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/monolith:2.0.0 .

kubectl set image deployment/monolith monolith=gcr.io/${GOOGLE_CLOUD_PROJECT}/monolith:2.0.0
```

## Task 5. Migrate Products to microservice

```
cd ~/monolith-to-microservices/microservices/src/products
gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/products:1.0.0 .

kubectl create deployment products --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/products:1.0.0

kubectl expose deployment products --type=LoadBalancer --port 80 --target-port 8082

kubectl get service products

cd ~/monolith-to-microservices/react-app
nano .env.monolith
```

* Replace the `REACT_APP_PRODUCTS_URL` to the new format

```
REACT_APP_ORDERS_URL=http://<ORDERS_IP_ADDRESS>/api/orders
REACT_APP_PRODUCTS_URL=http://<PRODUCTS_IP_ADDRESS>/api/products
```

* Press `CTRL+O, press ENTER, then CTRL+X` to save the file.

```
npm run build:monolith

cd ~/monolith-to-microservices/monolith
gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/monolith:3.0.0 .

kubectl set image deployment/monolith monolith=gcr.io/${GOOGLE_CLOUD_PROJECT}/monolith:3.0.0
```

## Task 6. Migrate frontend to microservice

```
cd ~/monolith-to-microservices/react-app
cp .env.monolith .env
npm run build

cd ~/monolith-to-microservices/microservices/src/frontend
gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/frontend:1.0.0 .

kubectl create deployment frontend --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/frontend:1.0.0

kubectl expose deployment frontend --type=LoadBalancer --port 80 --target-port 8080
```

# Congratulation🎉! You're all done with this lab.