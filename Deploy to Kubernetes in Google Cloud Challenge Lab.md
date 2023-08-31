## `Lab Name` - *Deploy to Kubernetes in Google Cloud: Challenge Lab [GSP318]*

## `Lab Link` - [*Click Here*](https://www.cloudskillsboost.google/focuses/10457?parent=catalog)

<!-- ## [YouTube Solution Link]() -->


## Task 1. Create a Docker image and store the Dockerfile

```
export DOCKER_IMAGE_WITH_TAG_NAME=

export REPOSITORY=

export PROJECT_ID=
```

```
gcloud auth list

gsutil cat gs://cloud-training/gsp318/marking/setup_marking_v2.sh | bash

gcloud source repos clone valkyrie-app

cd valkyrie-app
cat > Dockerfile <<EOF
FROM golang:1.10
WORKDIR /go/src/app
COPY source .
RUN go install -v
ENTRYPOINT ["app","-single=true","-port=8080"]
EOF

docker build -t $DOCKER_IMAGE_WITH_TAG_NAME .

cd ..

cd marking

./step1_v2.sh
```

## Task 2. Test the created Docker image

```
cd ..

cd valkyrie-app

docker run -p 8080:8080 $DOCKER_IMAGE_WITH_TAG_NAME &

cd ..

cd marking

./step2_v2.sh

bash ~/marking/step2_v2.sh
```

## Task 3. Push the Docker image to the Artifact Registry

```
cd ..

cd valkyrie-app

gcloud artifacts repositories create $REPOSITORY \
    --repository-format=docker \
    --location=us-central1 \
    --description="SUBSCRIBE to Atul Gupta" \
    --async 

gcloud auth configure-docker us-central1-docker.pkg.dev

docker images
```

* Replace `<Image_ID>` with the *IMAGE-ID* which you got from the above command.

```
docker tag <Image_ID> us-central1-docker.pkg.dev/$PROJECT_ID/$REPOSITORY/$DOCKER_IMAGE_WITH_TAG_NAME
```

```
docker push us-central1-docker.pkg.dev/$PROJECT_ID/$REPOSITORY/$DOCKER_IMAGE_WITH_TAG_NAME

sed -i s#IMAGE_HERE#us-central1-docker.pkg.dev/$PROJECT_ID/$REPOSITORY/$DOCKER_IMAGE_WITH_TAG_NAME#g k8s/deployment.yaml
```

## Task 4. Create and expose a deployment in Kubernetes

```
gcloud container clusters get-credentials valkyrie-dev --zone us-east1-d
kubectl create -f k8s/deployment.yaml
kubectl create -f k8s/service.yaml
```

# Congratulations🎉! You're all done with this Challenge Lab.