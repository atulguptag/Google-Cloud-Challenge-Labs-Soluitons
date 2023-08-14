## `Lab Name` - *Build and Deploy a Docker Image to a Kubernetes Cluster [GSP304]*

## `Lab Link` - [Click Here](https://www.cloudskillsboost.google/focuses/1738?parent=catalog)

## YouTube Solution Link - `To be Uploaded Soon`

Run the below commands in the Cloud Shell Terminal.

```
export ZONE=
```

```
export MACHINE_TYPE=
```

* Before starting this lab, we need to download the source code of the application which is given in the lab instruction.

```
wget https://github.com/atulguptag/Google-Cloud-Challenge-Labs-Soluitons/raw/main/Build%20and%20Deploy%20a%20Docker%20Image%20to%20a%20Kubernetes%20Cluster/resources-echo-web.tar.gz
```

* Extract the downloaded application file 

```
tar -xvf resources-echo-web.tar.gz
```

## Task 1: Create a Kubernetes Cluster 

* The below command will take approx 4-5 mints to create cluster, so please be patience.

```
gcloud container clusters create echo-cluster --num-nodes 2 --zone $ZONE --machine-type $MACHINE_TYPE
```

## Task 2: Build a tagged Docker Image & Push the image to the Google Container Registry
```
gcloud builds submit --tag gcr.io/$DEVSHELL_PROJECT_ID/echo-app:v1 .
```

## Task 3: Deploy the application to the Kubernetes Cluster

3.1 Connect to the previously created cluster so deployment of application

```
gcloud container clusters get-credentials echo-cluster --zone $ZONE --project $DEVSHELL_PROJECT_ID
```

3.2 Deploy the sample application image, which we created in the previous step

```
kubectl create deployment echo-web --image=gcr.io/$DEVSHELL_PROJECT_ID/echo-app:v1
```

3.3 Expose your deployment by creating a service

```
kubectl expose deployment echo-web --type=LoadBalancer --port=80 --target-port=8000
```

# Congratulations🎉! You're all done with this Challenge Lab.
