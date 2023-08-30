## `Lab Name` - *App Engine: 3 Ways: Challenge Lab [ARC112]*

## `Lab Link` - [*Click Here*](https://www.cloudskillsboost.google/focuses/63241?parent=catalog)

## [YouTube Video Link](https://youtu.be/GpTpeUGBTsU)

Run the below commands in *SSH on the VM Instance* `lab-setup`.

## Task 1 - Enable the Google App Engine Admin API

```
gcloud services enable appengine.googleapis.com
```

## Task 2 - Download the Hello World app

*Make sure to download the web application on the VM instance "lab-setup"*

```
git clone https://github.com/GoogleCloudPlatform/php-docs-samples.git
cd php-docs-samples/appengine/standard/helloworld
```

* After cloning the above command in the `SSH of VM-Instance`, come back to your `cloud console` and open the `Cloud Shell Terminal`.

* And again, `recloning the sample file` using the *above commands*.


## Task 3 - Deploy your application
	
```
gcloud app deploy
gcloud app browse
```

* Choose *Region* in the list which you have given in your lab. --> (This is the most important thing in this step.)
* If asking "Enter your choice (Y/n)" then --> press y


## Task 4 - Deploy updates to your application

* Here, in the `index.php`, change the code to *Hello, World!* to `Goodbye World!`.
* Now, after editing the code, save and exit from the editor by pressing the `Ctrl+X, Y`, and then hit `Enter`.

```
nano index.php
gcloud app deploy
```

# Congratulations🎉!, You're all done with this Challenge lab.