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

## Google Kubernetes Engine
* Get cluster credentials and create `kubectl` config
```
gcloud container clusters get-credentials <cluster name>
```