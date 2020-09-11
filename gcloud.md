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

* SSH into a node
```
gcloud compute ssh <node name>
```

## Google Kubernetes Engine
* Get cluster credentials and create `kubectl` config
```
gcloud container clusters get-credentials <cluster name>
```

* Visibility into autoscaling: https://cloud.google.com/kubernetes-engine/docs/how-to/cluster-autoscaler-visibility

## Google Cloud Platform Authentication
### IAM
* https://cloud.google.com/iam/docs/overview

### Roles
* https://cloud.google.com/iam/docs/granting-changing-revoking-access

* List existing roles.
```
gcloud iam roles list | grep <filter>
```

* List role details.
```
gcloud iam roles describe <role>
```

### OAuth2
* https://developers.google.com/identity/protocols/oauth2/scopes#storage
* https://stackoverflow.com/questions/27275063/gsutil-copy-returning-accessdeniedexception-403-insufficient-permission-from/49961489


## Quotas
* List project-wide and regional quotas, both of which set usage limits.
```
# project specific quotas
gcloud compute project-info describe --project <project name>

# regional quotas
gcloud compute regions describe <region>
```
