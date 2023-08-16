## `Lab Name` - *Monitoring in Google Cloud: Challenge Lab [ARC115]*

## `Lab Link` - [Click Here](https://www.cloudskillsboost.google/focuses/63855?parent=catalog)

Run the below task 1 commands in the Cloud Shell Teminal.

## Task 1. Install the Cloud Logging and Monitoring agents

```
gcloud compute ssh <VM_INSTANCE_NAME> --zone=<VM_INSTANCE_ZONE>
```

``` 
curl -sSO https://dl.google.com/cloudagents/add-logging-agent-repo.sh
sudo bash add-logging-agent-repo.sh --also-install
```

```
curl -sSO https://dl.google.com/cloudagents/add-monitoring-agent-repo.sh
sudo bash add-monitoring-agent-repo.sh --also-install
```

```
(cd /etc/stackdriver/collectd.d/ && sudo curl -O https://raw.githubusercontent.com/Stackdriver/stackdriver-agent-service-configs/master/etc/collectd.d/apache.conf)
sudo service stackdriver-agent restart
```

## Task 2. Add an uptime check for Apache Web Server on the VM

* In Google Cloud console, select *Navigation menu* > *Monitoring* > *Uptime Check* > click `Create Uptime Check`.


* For *Protocol*, select `HTTP`.
* For *Resource Type*, select `Instance`.
* For *Instance*, select `VM_NAME`.
* Click `Continue`.

* In *Response Validation*, accept the defaults and then click `Continue`.
* In *Alert & Notification*, accept the defaults, and then click `Continue`.
* For *Title*, type `Subscribe_to_Atul_Gupta`
* Click `Create`.

## Task 3. Add an alert policy for Apache Web Server

* In the Cloud Console, in the left menu, Click *Alerting* > click `+Create Policy`.
* Select the time series to be monitored:
    * Click *Select a metric* and enter `VM instance` into the Filter Bar.
    * In the *Active metric categories list*, select `Apache`.
    * In the *Active metrics list*, select `traffic`.
    
    * In the Transform data section, select the following values:
        * *Rolling window*: `1 min`
        * *Rolling window function*: `rate`


* In the Configure alert trigger section, select the following values and click *Next*:
* Alert trigger: `Any time series violates`
* Threshold position: `Above threshold`
* Threshold value: `3072`
 
* Click on the drop down arrow next to Notification Channels, then click on Manage Notification Channels.
    * A Notification channels page will open in a new tab.

* Scroll down the page and click on `ADD NEW for Email`.

* In the *Create Email Channel dialog box*, enter the given `student email address` in the *Email Address field* and for a Display name enter `Student`.

* Click on `Save`.

* Go back to the previous Create alerting policy tab.

* Click on *Notification Channels again*, then click on the `Refresh icon` to get the display name you mentioned in the previous step.

* Click on Notification Channels again if necessary, select your Display name and click OK.

* Mention the Alert name as `Subscribe_to_Atul_Gupta`.

* Click *Next*.

* Review the alert and click `Create Policy`.


## Task 4. Create a dashboard and charts for Apache Web Server on the VM

* In the left menu select Dashboards, and then +Create Dashboard.
* Name the dashboard - `Subscribe_to_Atul_Gupta`

* Add the *first chart*

* Click the Line option in the Chart library.

* Name the chart title - `CPU Load`.

* Click on *Resource & Metric dropdown*. Disable the `Show only active resources & metrics`.

* Type CPU load (1m) in filter by resource and metric name and click on VM instance > Cpu. Select CPU load (1m) and click Apply. Leave all other fields at the default value. Refresh the tab to view the graph.

* Add the second chart

* Click `+ Add Chart` and select `Line` option in the Chart library.

* Name this chart - `Received Packets`.

* Click on *Resource & Metric dropdown*. Disable the `Show only active resources & metrics`.

* Type `Requests` in filter by *resource and metric name* and click on *VM instance* > `Apache` > Select `Requests` and click `Apply`.

* Refresh the tab to view the graph.

* Leave the other fields at their default values. You see the chart data.


## Task 5. Create a log-based metric

* In the Console, select *Navigation menu* > *Logging* > `Log-based Metrics`.
* *The Cloud Logging opens in the Console.*
* Click `Create metric`.
* In the *Create logs metric*:
* Change the *Metric Type* to `Distribution`.
* Name your *metric* `Subscribe_to_Atul_Gupta`
* Paste the below code in the `Build Filter`.

```
resource.type="gce_instance"
logName="projects/PROJECT_ID/logs/apache-access"
textPayload:"200"
```
* REPLACE `PROJECT_ID` with Your *GCP Project-Id*.

* For Field name > Enter `textPayload`. 

* Enter the below text in the `Regular Expression field`:

```
execution took (\d+)
```

# Congratulations🎉! You're all done with this Challenge Lab.