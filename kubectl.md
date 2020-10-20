# `kubectl` Cheatsheet

## Installation
* Add autocomplete to `kubectl` on `zsh`

```sh
# add the following to ~/.zshrc
source <(kubectl completion zsh)
alias k=kubectl
complete -F __start_kubectl k
```

## Commands

#### Get current version of Kubernetes

```sh
$ k version
```

#### Get custom columns

* Find the custom column names by using `-o json`
* Sort by using `--sort-by`

```sh
$ k get po \
  -o custom-columns='NAME:metadata.name,IP:status.hostIP' \
  --sort-by 'spec.nodeName'
```

#### List pods with labels

```sh
$ k get po --show-labels
$ k get po -L <label names>
```

#### Add labels to pods

```sh
$ k label po <pod name> <labels> [--overwrite]
```

#### Filter pods by label

```sh
$ k get po -l <label>=<value>
```

#### Explain a resource and its API fields

```sh
$ k explain <resource>
```

#### Port-forwarding to a pod for debugging

```sh
$ k port-forward <pod name> <local port>:<pod port>
```

#### Create a namespace

```sh
$ k create ns <namespace name>
```

#### Edit a running resource

```sh
$ kubectl edit <resource type> <resource name>
```

#### Delete a `ReplicationController` without deleting `Pod`s

```sh
$ k delete rc kubia --cascade=false
```

#### Execute a command in a pod

```sh
$ k exec <pod name> -- <command> [args]
```

* The `--` signifies the end of command options for `kubectl`

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

#### List rollout history of a deployment

```sh
$ k rollout history deployment <name>
```

#### Roll back a deployment

```sh
$ k rollout undo deployment <name> [--to-revision=<number>]
```

#### Watch events emitted by the `API Server`

```sh
k get events --watch
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

#### Create a `Secret` on the command line

```sh
k create secret generic <secret name> \
  [--from-file=[key=]<file name>]     \
  [--from-literal=<key1>=<value1>]
```

## Debugging

* obtain application log of a crash container

```sh
kubectl logs <podname> --previous
```

* watch the logs of a running container

```sh
k logs -f <podname> -c <container>
```

* check the status of a rollout

```sh
k rollout status deployment <deployment name>
```


## Security and `ServiceAccounts`

* Create a `ServiceAccount`

```sh
k create serviceaccount <svc account name>
```

- Create a `Role`

```sh
k create role <role_name>       \
  --verb=<a_valid_verb>         \
  --resource=<a_valid_resource> \
  -n <namespace>
```

- Create a `RoleBinding`

```sh
k create rolebinding <rolebinding_name>         \
  --role=<role_name>                            \
  --serviceaccount=<namespace>:<account_name>   \
  -n <namespace>
```


## Debugging

* create a temporary pod for debugging
  * Useful images include
    * `tutum/dnsutils`. for DNS related debugging

```sh
$ k run <pod name> --image=<image> --generator=run-pod/v1 --command -- sleep infinity
```

* increase output verbosity

```sh
k [-v={0..9}>] <command>
```

* accessing the Kubernetes API server from a development machine

```sh
$ k proxy 
```
