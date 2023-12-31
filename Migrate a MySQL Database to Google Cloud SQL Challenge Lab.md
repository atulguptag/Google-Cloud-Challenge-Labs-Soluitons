## `Lab Link` -  *Migrate a MySQL Database to Google Cloud SQL [GSP306]*
## `Lab Link` - [Click Here](https://www.cloudskillsboost.google/focuses/1740?parent=catalog)

## [YouTube Solution Link](https://www.youtube.com/watch?v=bxI_-W21LHE)
## Task 1: Check that there is a Cloud SQL instance

```cmd
gcloud sql instances create wordpress --tier=db-n1-standard-1 --activation-policy=ALWAYS --zone=us-central1-a --database-version=MYSQL_5_7

gcloud sql users set-password --host % root --instance wordpress --password Password1*
```

## Task 2 & 3: Check that there is a user database on the Cloud SQL instance & Check that the blog instance is authorized to access Cloud SQL

* Storing Zone of your blog_vm_instance into a variable
```cmd
export ZONE=<YOUR_BLOG_VM_INSTANCE_ZONE>
```

* Storing the BLOG_EXTERNAL_IP into a variable.
```cmd
export BLOG_VM_EXTERNAL_IP=<YOUR_BLOG_VM_EXTERNAL_IP>
```

* Authorize blog VM to your cloudSQL instance
```cmd
gcloud sql instances patch wordpress --authorized-networks $BLOG_VM_EXTERNAL_IP/32 --quiet
```

* Connect to blog VM using SSH
```
gcloud compute ssh blog --zone=$ZONE
```

* Storing CloudSQL instance IP into a variable
```cmd
export CLOUD_SQL_IP=$(gcloud sql instances describe wordpress --format 'value(ipAddresses.ipAddress)')
```

* Connecting to the cloudSQL instance
```
mysql --host=$CLOUD_SQL_IP --user=root --password=Password1*
```

* Creating database & user for your cloud SQL database

```
CREATE DATABASE wordpress; CREATE USER 'blogadmin'@'%' IDENTIFIED BY 'Password1*'; GRANT ALL PRIVILEGES ON wordpress.* TO 'blogadmin'@'%'; FLUSH PRIVILEGES;
```

* All done from here, let's exit

```
exit
```

```
sudo mysqldump -u root -pPassword1* wordpress > wordpress_db_backup.sql

mysql --host=$CLOUD_SQL_IP --user=root -pPassword1* --verbose wordpress < wordpress_db_backup.sql
```

## Task 4 & 5: Check that wp-config.php points to the Cloud SQL instance and Check that the blog still responds to requests

* Restart the Apache webserver service

```cmd
sudo service apache2 restart
```

* Replace the Database IP configuration from localhost to our cloudSQL instance IP

```cmd
sudo sed -i "s/localhost/$CLOUD_SQL_IP/g" /var/www/html/wordpress/wp-config.php
```

# Congratulations!🎉 You're all done with this Challenge Lab.