## `Lab Name` - *VPC Networks - Controlling Access [GSP213]*

## `Lab Link` - [Click Here](https://www.cloudskillsboost.google/focuses/1231?parent=catalog)

## [YouTube Solution Link](https://youtu.be/lqpSxMuyjf0)

Run the following commands in the Cloud Shell Terminal.

```
export ZONE=
```

## Task 1. Create the web servers

```
gcloud compute instances create blue \
    --zone=$ZONE \
    --machine-type=e2-medium \
    --tags=web-server \

gcloud compute instances create green \
    --zone=$ZONE \
    --machine-type=e2-medium
```

* SSH into the blue server.

```
gcloud compute ssh blue --zone=$ZONE
```

```
sudo apt-get install nginx-light -y

sudo nano /var/www/html/index.nginx-debian.html
```

* Replace the *`<h1>Welcome to nginx!</h1>`* line with `<h1>Welcome to the blue server!</h1>`.

* Press `Ctrl+X, Y`, and then hit `ENTER`.

```
cat /var/www/html/index.nginx-debian.html

exit
```

* SSH into the green server.

```
gcloud compute ssh green --zone=$ZONE
```

```
sudo apt-get install nginx-light -y

sudo nano /var/www/html/index.nginx-debian.html
```

* Replace the *`<h1>Welcome to nginx!</h1>`* line with `<h1>Welcome to the green server!</h1>`.

* Press `Ctrl+X, Y`, and then hit `ENTER`.

```
cat /var/www/html/index.nginx-debian.html

exit
```

## Task 2. Create the firewall rule

```
gcloud compute firewall-rules create allow-http-web-server --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80,icmp --source-ranges=0.0.0.0/0 --target-tags=web-server

gcloud compute instances create test-vm --machine-type=e2-micro --subnet=default --zone=$ZONE
```

## Task 3. Explore the Network and Security Admin roles

### Create a service account

* In the Console, navigate to *Navigation menu* > *IAM & admin* > `Service Accounts`.

* Notice the Compute Engine default service account.

* Click `Create service account`.

* Set the Service account name to `Network-admin` and click `CREATE AND CONTINUE`.

* For Select a role, select *Compute Engine* > `Compute Network Admin` and click `CONTINUE` then click `DONE`.

# Congratulation🎉! You're all done with this lab.