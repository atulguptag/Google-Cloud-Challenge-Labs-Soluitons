### Lab Name - The Basics of Google Cloud Compute: Challenge Lab [ARC120]
### Lab Link - [CLICK HERE](https://www.cloudskillsboost.google/focuses/65384?parent=catalog)

## [YouTube Solution Link](https://youtu.be/7OAMnqoLSuU)

Run the below commands in the Cloud Shell Terminal.

```cmd  
export ZONE=
```

## Task 1. Create a Cloud Storage bucket

```cmd
export REGION=${ZONE::-2}
export BUCKET="gs://$DEVSHELL_PROJECT_ID"
gsutil mb -l $REGION gs://$DEVSHELL_PROJECT_ID
```

## Task 2. Create and attach a persistent disk to a Compute Engine instance 

```cmd
gcloud compute instances create my-instance --project=$DEVSHELL_PROJECT_ID --zone=$ZONE --machine-type=e2-medium --tags=http-server

gcloud compute disks create mydisk --size=200GB \
--zone=$ZONE

gcloud compute instances attach-disk my-instance --disk mydisk --zone=$ZONE
```


## Task 3. Install a NGINX web server

```cmd
gcloud compute ssh my-instance --zone=$ZONE
```

```cmd
sudo apt-get update

sudo apt-get install -y nginx

ps auwx | grep nginx
```

# Congratulations🎉! You are done with this Challenge Lab.