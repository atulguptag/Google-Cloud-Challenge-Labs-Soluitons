# Lab Name - Deploy a Compute Instance with a Remote Startup Script

# Lab Link - [Click Here](https://www.cloudskillsboost.google/focuses/1735?parent=catalog)

### Run the below commands in the cloud shell terminal.

## Task 1: Confirm that a Google Cloud Storage bucket exists that contains a file

```cmd
export Zone=<YOUR_ZONE>
```

```cmd
gsutil mb -b off gs://$DEVSHELL_PROJECT_ID

wget https://raw.githubusercontent.com/atulguptag/Google-Cloud-Challenge-Labs-Soluitons/main/Deploy%20a%20Compute%20Instance%20with%20a%20Remote%20Startup%20Script/resources-install-web.sh

gsutil cp resources-install-web.sh gs://$DEVSHELL_PROJECT_ID
```


## Task 2: Confirm that a compute instance has been created that has a remote startup script called install-web.sh configured

```cmd
gcloud compute instances add-metadata lab-monitor --metadata startup-script-url=gs://$DEVSHELL_PROJECT_ID/resources-install-web.sh --zone $Zone
```

## Task 3: Confirm that a HTTP access firewall rule exists with tag that applies to that virtual machine

```cmd
gcloud compute firewall-rules create http-fw-rule --allow=tcp:80 --source-ranges 0.0.0.0/0 --target-tags allow-http-traffic --network default

gcloud compute instances add-tags lab-monitor --tags=allow-http-traffic --zone $Zone

gcloud compute instances reset lab-monitor --zone $Zone
```

## Task 4: Connect to the server ip-address using HTTP and get a non-error response

```cmd
export VM_EXTERNAL_IP=$(gcloud compute instances describe lab-monitor --zone $Zone --format='get(networkInterfaces[0].accessConfigs[0].natIP)')

curl http://$VM_EXTERNAL_IP
```

# Congratulations🎉! You are done with this Challenge Lab.
