## `Lab Name` - *Build and Secure Networks in Google Cloud: Challenge Lab [GSP322]*

## `Lab Link` - [Click Here](https://www.cloudskillsboost.google/focuses/12068?parent=catalog)

<!-- YouTube Solution Link - To be uploaded soon. -->

Run the following commands in the Cloud Shell Terminal.

### Let's start with defining some variables given by Cloud Skill Boosts

* For exporting the zone, first go to *Compute Engine* > *VM Instances* > Copy `Any Instance Zone`(both instances zone should be in the same.)

```
export ZONE=
```

```
export SSH_IAP_NETWORK_TAG=
```

```
export SSH_INTERNAL_NETWORK_TAG=
```

```
export HTTP_NETWORK_TAG=
```

## Task 1 : Remove the overly permissive rules

```
gcloud compute firewall-rules delete open-access
```

## Task 2 : Start the bastion host instance
```
gcloud compute instances start bastion --zone=$ZONE
```

## Task 3 : Create a firewall rule that allows SSH (tcp/22) from the IAP service and add network tag on bastion

```
gcloud compute firewall-rules create ssh-ingress --allow=tcp:22 --source-ranges 35.235.240.0/20 --target-tags $SSH_IAP_NETWORK_TAG --network acme-vpc

gcloud compute instances add-tags bastion --tags=$SSH_IAP_NETWORK_TAG --zone=$ZONE
```

## Task 4 : Create a firewall rule that allows traffic on HTTP (tcp/80) to any address and add network tag on juice-shop

```
gcloud compute firewall-rules create http-ingress --allow=tcp:80 --source-ranges 0.0.0.0/0 --target-tags $HTTP_NETWORK_TAG --network acme-vpc

gcloud compute instances add-tags juice-shop --tags=$HTTP_NETWORK_TAG --zone=$ZONE
```

## Task 5 : Create a firewall rule that allows traffic on SSH (tcp/22) from acme-mgmt-subnet network address and add network tag on juice-shop

```
gcloud compute firewall-rules create internal-ssh-ingress --allow=tcp:22 --source-ranges 192.168.10.0/24 --target-tags $SSH_INTERNAL_NETWORK_TAG --network acme-vpc

gcloud compute instances add-tags juice-shop --tags=$SSH_INTERNAL_NETWORK_TAG --zone=$ZONE
```

## Task 6 : SSH to bastion host via IAP and juice-shop via bastion

```
gcloud compute ssh --zone "$ZONE" "bastion"  --tunnel-through-iap --project $DEVSHELL_PROJECT_ID
```

* If prompted, type `y` & then hit `Enter Two Times`. You'll see you're successfully login to the bastion VM

```
export JUICE_SHOP_VM_INTERNAL_IP=$(gcloud compute instances describe juice-shop --zone=$ZONE --format='get(networkInterfaces[0].networkIP)')

gcloud compute ssh juice-shop --internal-ip
```

* If prompted, type `y` & then hit `Enter Two Times`. You'll see you're successfully login to the juice-shop VM from bastion VM.

```
ssh $JUICE_SHOP_VM_INTERNAL_IP
```

# Congratulations🎉! You're all done with this Challenge Lab.