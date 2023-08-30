## `Lab Name` - *Google Cloud Essential Skills: Challenge Lab [GSP101]*

## `Lab Link` - [*Click Here*](https://www.cloudskillsboost.google/focuses/1734?parent=catalog)

## [YouTube Solution Link](https://youtu.be/WCAxqZcU70g)


### Let's start with defining some variables given by Cloud Skill Boosts

```
export VM_NAME=
```

```
export ZONE=
```

## Task1 Create a Compute Engine instance, add necessary firewall rules.

```
gcloud compute firewall-rules create http-ingress --allow=tcp:80 --source-ranges 0.0.0.0/0 --target-tags http-server --network default
```

## Task2 Configure Apache2 Web Server in your instance

```
gcloud compute instances create $VM_NAME \
--zone=$ZONE \
--machine-type=e2-medium \
--tags=http-server,https-server \
--image=projects/debian-cloud/global/images/debian-10-buster-v20220406 \
--metadata=startup-script=\#\!\ /bin/bash$'\n'apt-get\ update$'\n'apt-get\ install\ apache2\ -y$'\n'service\ --status-all$'\n' 
```

## Task3 Test your server

```
gcloud compute instances describe $VM_NAME \
  --format='get(networkInterfaces[0].accessConfigs[0].natIP)'
```

# Congratulations🎉! You're all done with this Challenge Lab.