# Setting up Kubernetes

## Solo dev production (CD) k3s cluster

### Prerequisites
1. Create DigitalOcean droplet.
2. Install Tailscale and join tailnet.

### Installing k3s

```bash
# install k3s
# may need to add tailnet/public IP: https://github.com/k3s-io/k3s/issues/1381
# curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="--tls-san x.x.x.x" sh -s -

curl -sfL https://get.k3s.io | sh -

# https://docs.k3s.io/cluster-access

```

### Setting up `cert-manager`

```bash
# https://cert-manager.io/docs/installation/kubectl
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.3/cert-manager.yaml

# create staging and prod certificate ClusterIssuers
# https://opensource.com/article/20/3/ssl-letsencrypt-k3s
# note: apiVersion cert-manager.io/v1alpha2 -> cert-manager.io/v1
```


### Setting up ArgoCD

```bash
kubectl create namespace argocd

# stable = 2.9.3; installing core (no UI)
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/core-install.yaml

# install argocd CLI tool
brew install argocd 
argocd login --core
```

### ArgoCD with ArgoCD Vault Plugin (AVP)

- Backends (authenticating with Vault): https://argocd-vault-plugin.readthedocs.io/en/stable/backends/
- Usage: https://argocd-vault-plugin.readthedocs.io/en/stable/usage/
    - Refreshing values from Secrets Managers
- ArgoCD Repo Credentials Declarative Setup: https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#repository-credentials

```bash

```


### Configuring access to external Vault server

```bash

```


