
## `PersistentVolumes`
* `ProvisioningFailed` on the `PersistentVolume`
  * Culprit 1: Non-existent storage class.


## Communicating with Pods

```sh
# Communcating with the API server to access a pod
# Tip: `-L` tells curl to follow redirects
# Tip: Use `kubectl proxy` to avoid authentication

SERVER=<api server host>
PORT=<api server port>
NS=<namespace>
POD=<pod name>
PATH=<path to access>
curl -L $SERVER:$PORT/api/v1/namespaces/$NS/pods/$POD/proxy/$PATH

```

## Unsolved Kubernetes Bugs/Issues

* Containers stuck on image pull (probably because network not available?)
  - https://github.com/kubernetes/kubernetes/issues/83471
  - https://github.com/kubernetes/kubernetes/issues/44273
  * Temporary solution: Node was unhealthy (figured this out by ssh'ing into
    it, and seeing that commands took a long time to respond.). Deleting the
    node and creating a new one fixed the problem.


