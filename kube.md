# `kubectl` Cheatsheet

## Installation

#### Add autocomplete to `kubectl` on `zsh`

```bash
# add the following lines to ~/.zshrc
alias k=kubectl
source <(kubectl completion zsh)
```

## Commands

### Monitoring

```bash
# monitoring a Kubernetes cluster
kubectl top node
kubectl top pod
kubectl top pod --containers

# get current version of Kubernetes
kubectl version
```

### Get custom columns

* Find the custom column names by using `-o json`
* Sort by using `--sort-by`

```sh
$ k get po \
  -o custom-columns='NAME:metadata.name,IP:status.hostIP' \
  --sort-by 'spec.nodeName'
```

### Pods

```bash
# List pods with labels
kubectl get po --show-labels
kubectl get po -L <label names>

# Add labels to pods
kubectl label po <pod name> <labels> [--overwrite]

# Filter pods by label
kubectl get po -l <label>=<value>

# create a debugging pod
# source: https://archive.ph/Miwsz
kubectl run DESIRED_DEBUG_POD_DEPLOYMENT_NAME --rm -i --tty --image ubuntu -- bash

# Port-forwarding to a pod for debugging
kubectl port-forward <pod name> <local port>:<pod port>

# Execute a command in a pod
#   the `--` signifies the end of command options for `kubectl`
kubectl exec <pod name> -- <command> [args]

# Get an interactive shell
#   we use /bin/sh because /bin/bash is generally not available)
kubectl exec -it POD_NAME [-c CONTAINER_NAME] -- /bin/sh
```

#### Explain a resource and its API fields

```sh
$ k explain <resource>
```

### Namespaces

```bash
# create a namespace
kubectl create ns DESIRED_NAMESPACE_NAME
```

#### Edit a running resource

```sh
$ kubectl edit <resource type> <resource name>
```

#### Delete a `ReplicationController` without deleting `Pod`s

```sh
$ k delete rc kubia --cascade=false
```

#### Get a list of all available Kubernetes resources

```sh
$ k api-resources -o wide
```

#### Get metadata about the cluster

```sh
$ k cluster-info
```

#### Accessing the Kubernetes API from within a `Pod`
```sh
$ k describe po <pod name>
# ... output trimmed ...
# Mounts:
#  /var/run/secrets/kubernetes.io/serviceaccount
# ...

# from within a pod
CURL_CA_BUNDLE=/var/run/secrets/kubernetes.io/serviceaccount/ca.crt
TOKEN=$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)
NS=$(cat /var/run/secrets/kubernetes.io/serviceaccount/namespace)
curl -H "Authorization: Bearer $TOKEN" \
  https://kubernetes/api/v1/namespaces/$NS/pods
```

* Alternative methods to reduce the complexity of querying the Kubernetes API
  include:
  * using a client library (Go or Python)
  * using an ambassador container as a proxy

### Deployments

```bash
# List rollout history of a deployment
kubectl rollout history deployment <name>

# Roll back a deployment
kubectl rollout undo deployment <name> [--to-revision=<number>]
```

#### Watch events emitted by the `API Server`

```sh
$ k get events --watch
```

## Configuration

#### Create a `ConfigMap` on the command line

```sh
# from literals
$ k create configmap <name> --from-literal=<key>=<value> \
  [--from-literal=<key 2>=<value 2>]

# from files
$ k create configmap <name> --from-file=<key>=<path to file> \
  [--from-file=<key 2>=<path to file 2>]
```

### Secrets

```bash
# create a Secret from a literal
kubectl create secret generic DESIRED_SECRET_NAME --from-literal=<key1>=<value1>

# create a Secret from an environment file
# source: https://archive.ph/46UEE
kubectl create secret generic <secretName> --from-env-file=<file>

# read a Secret 
kubectl get secret SECRET_NAME -o jsonpath="{.data.<DATA>}" | base64 --decode
```

## Security and `ServiceAccounts`

#### Create a `ServiceAccount`

```sh
$ k create serviceaccount <svc account name>
```

#### Create a `Role`

```sh
$ k create role <role_name>       \
  --verb=<a_valid_verb>         \
  --resource=<a_valid_resource> \
  -n <namespace>
```

#### Create a `RoleBinding`

```sh
$ k create rolebinding <rolebinding_name>         \
  --role=<role_name>                            \
  --serviceaccount=<namespace>:<account_name>   \
  -n <namespace>
```

## Debugging

#### Obtain application log of a crash container

```sh
$ kubectl logs <podname> --previous
```

#### Watch the logs of a running container

```sh
$ k logs -f <podname> -c <container>
```

#### Check the status of a rollout

```sh
$ k rollout status deployment <deployment name>
```

#### Create a temporary pod for debugging

```sh
$ k run <pod name> --image=<image> --generator=run-pod/v1 --command -- sleep infinity
```

* Useful images include
    * `tutum/dnsutils`. for DNS related debugging

#### Increase output verbosity

```sh
$ k [-v={0..9}>] <command>
```

#### Accessing the Kubernetes API server from a development machine

```sh
$ k proxy 
```


## Helm

```bash
# Install a Helm chart with custom values
helm install --values=<my-values.yaml> <chartname>

# Get the values that can be customized
helm show values <chart>

# Keep track of a release's state
helm status <release name>
```
