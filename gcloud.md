# Google Cloud SDK CLI cheatsheet

* Google Cloud SDK CLI reference: https://cloud.google.com/sdk/docs

## Config
* Get current configuration
```
gcloud config list
```

## Google Compute Engine

* Create a GCE persistent disk
```
gcloud compute disks create --size=[number]{GB|TB} --zone=[zone] <name>
```


