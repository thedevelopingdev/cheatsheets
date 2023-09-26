# Google Cloud cheatsheet

## Pricing
- https://cloud.google.com/pricing
- https://cloud.google.com/compute/gpus-pricing
- https://cloud.google.com/compute/docs/gpus
- https://cloud.google.com/compute/docs/machine-types

## CLI

- Google Cloud SDK CLI reference: https://cloud.google.com/sdk/docs

### CLI config

```bash
# Get current configuration
gcloud config list
```

### Google Compute Engine (GCE)

```bash
# Create a GCE persistent disk
gcloud compute disks create --size=[number]{GB|TB} --zone=[zone] <name>

# SSH into a node
gcloud compute ssh <node name>

# List all public images
# can also be found in the console: https://console.cloud.google.com/compute/images?tab=images&imagessize=50
gcloud compute images list
```

### Google Kubernetes Engine (GKE)

```bash
# Get cluster credentials and create `kubectl` config
gcloud container clusters get-credentials <cluster name>

```

* Visibility into autoscaling: https://cloud.google.com/kubernetes-engine/docs/how-to/cluster-autoscaler-visibility

### Google Cloud Platform Authentication

#### IAM
- Overview: https://cloud.google.com/iam/docs/overview
- IAM Permissions reference: https://cloud.google.com/iam/docs/permissions-reference
- Compute Engine IAM roles and permissions: https://cloud.google.com/compute/docs/access/iam
- Chaging roles and access: https://cloud.google.com/iam/docs/granting-changing-revoking-access
- REST API (which IAM refers to): https://cloud.google.com/compute/docs/reference/rest/v1/instances/get

```bash
# List existing roles
gcloud iam roles list | grep <filter>

# List role details.
gcloud iam roles describe <role>
```

#### OAuth2
- https://developers.google.com/identity/protocols/oauth2/scopes#storage
- https://stackoverflow.com/questions/27275063/gsutil-copy-returning-accessdeniedexception-403-insufficient-permission-from/49961489
- OAuth2 Scopes: https://developers.google.com/identity/protocols/oauth2/scopes

### Quotas

```bash
# Two kinds of quotas exist:
# 1. project-wide, and
# 2. regional.
# Both set usage limits, so you need to check you have quota for both.

# List project-specific quotas
gcloud compute project-info describe --project <project name>

# List regional quotas
gcloud compute regions describe <region>
```

### API

### Instance metadata
- https://cloud.google.com/compute/docs/storing-retrieving-metadata

- Necessary permissions
  - compute.instances.deleteAccessConfig
  - compute.instances.addAccessConfig

Why is there "service accounts" and then "IAM"? I first create a service
account, and then add it to IAM? Why? Can't it just appear as a account with no
roles?

### Debugging startup scripts

```sh
sudo journalctl -u google-startup-scripts.service
```
