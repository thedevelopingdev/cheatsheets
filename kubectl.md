# `kubectl` Cheatsheet

* Get custom columns; this one will show which nodes pods are running on.
* Find the custom column names by using "-o json"
```bash
$ k get po -o custom-columns='NAME:metadata.name,IP:status.hostIP'
```

* List pods with labels
```bash
$ k get po --show-labels
$ k get po -L <label names>
```

* Add labels to pods
```bash
$ k label po <pod name> <labels> [--overwrite]
```

* Filter pods by label
```bash
$ k get po -l <label>=<value>
```

* explain a resource and its API fields
```bash
$ k explain <resource>
```

* port-forwarding to a pod for debugging
```bash
$ k port-forward <pod name> <local port>:<pod port>
```

* creating a namespace
```bash
$ k create ns <namespace name>
```

* Edit a running resource
```bash
$ kubectl edit <resource type> <resource name>
```

* Delete a `ReplicationController` without deleting `Pod`s
```
$ k delete rc kubia --cascade=false
```

* execute a command in a pod
  * the `--` signifies the end of command options for `kubectl`
```
$ k exec <pod name> -- <command> [args]
```

* create a temporary pod for debugging
  * Useful images include
    * `tutum/dnsutils`. for DNS related debugging
```
$ k run <pod name> --image=<image> --generator=run-pod/v1 --command -- sleep infinity
```

* get a list of all available Kubernetes resources
```
$ k api-resources -o wide
```

* get metadata about the cluster
```
$ k cluster-info
```

* accessing the Kubernetes API server from a development machine
```
$ k proxy 
```

* accessing the Kubernetes API from within a `Pod`
  * the alternative methods are
    * use a client library (Go or Python)
    * use an ambassador container as a proxy
```
$ k describe po <pod name>
...
Mounts:
  /var/run/secrets/kubernetes.io/serviceaccount
...

$ k exec -it <pod name> -- bash
# CURL_CA_BUNDLE=/var/run/secrets/kubernetes.io/serviceaccount/ca.crt
# TOKEN=$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)
# NS=$(cat /var/run/secrets/kubernetes.io/serviceaccount/namespace)
# curl -H "Authorization: Bearer $TOKEN" \
  https://kubernetes/api/v1/namespaces/$NS/pods
```

* listing rollout history of a deployment
```
$ k rollout history deployment <name>
```

* rolling back a deployment
```
$ k rollout undo deployment <name> [--to-revision=<number>]
```

## Configuration

* create a `ConfigMap` on the command line
```
# from literals
$ k create configmap <name> --from-literal=<key>=<value> --from-literal=<key 2>=<value 2>

# from files
$ k create configmap <name> --from-file=<key>=<path to file> --from-file=<key 2>=<path to file 2>
```

* create a `Secret` on the command line
```
k create secret generic <secret name> --from-file=[key=]<file name>
```

## Debugging

* obtain application log of a crash container
```bash
$ kubectl logs <podname> --previous
```

* watch the logs of a running container
```bash
$ k logs -f <podname> -c <container>
```

* check the status of a rollout
```
k rollout status deployment <deployment name>
```

* increase output verbosity
```
k [-v={0..9}>] <command>
```