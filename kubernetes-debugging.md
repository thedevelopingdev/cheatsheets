
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

## Watching progress

```sh
k get events --watch
```


## Unsolved Kubernetes Bugs/Issues

### `ImagePullBackOff` and `NodeNotReady`

* Containers stuck on image pull (probably because network not available?)
  - https://github.com/kubernetes/kubernetes/issues/83471
  - https://github.com/kubernetes/kubernetes/issues/44273
  * Temporary solution: Node was unhealthy (figured this out by ssh'ing into
    it, and seeing that commands took a long time to respond.). Deleting the
    node and creating a new one fixed the problem.
- Related issue: pulling image fails, and then we receive `NodeNotReady` event
- Related issue: `FrequentDockerRestart = true`

```
Normal    Pulling                   pod/kubia-manual                                   Pulling image "mattfeng/kubia:0.1.0"
Normal    NodeNotReady              node/gke-kubeinaction-default-pool-a1156362-rqmk   Node gke-kubeinaction-default-pool-a1156362-rqmk status is now: NodeNotReady
Normal    Starting                  node/gke-kubeinaction-default-pool-a1156362-rqmk   Starting kubelet.
Normal    NodeHasSufficientPID      node/gke-kubeinaction-default-pool-a1156362-rqmk   Node gke-kubeinaction-default-pool-a1156362-rqmk status is now: NodeHasSufficientPID
Normal    NodeHasSufficientMemory   node/gke-kubeinaction-default-pool-a1156362-rqmk   Node gke-kubeinaction-default-pool-a1156362-rqmk status is now: NodeHasSufficientMemory
Normal    NodeHasNoDiskPressure     node/gke-kubeinaction-default-pool-a1156362-rqmk   Node gke-kubeinaction-default-pool-a1156362-rqmk status is now: NodeHasNoDiskPressure
Normal    NodeNotReady              node/gke-kubeinaction-default-pool-a1156362-rqmk   Node gke-kubeinaction-default-pool-a1156362-rqmk status is now: NodeNotReady
Normal    NodeHasSufficientMemory   node/gke-kubeinaction-default-pool-a1156362-rqmk   Node gke-kubeinaction-default-pool-a1156362-rqmk status is now: NodeHasSufficientMemory
Normal    NodeHasNoDiskPressure     node/gke-kubeinaction-default-pool-a1156362-rqmk   Node gke-kubeinaction-default-pool-a1156362-rqmk status is now: NodeHasNoDiskPressure
Normal    NodeHasSufficientPID      node/gke-kubeinaction-default-pool-a1156362-rqmk   Node gke-kubeinaction-default-pool-a1156362-rqmk status is now: NodeHasSufficientPID
Normal    NodeAllocatableEnforced   node/gke-kubeinaction-default-pool-a1156362-rqmk   Updated Node Allocatable limit across pods
Normal    NodeReady                 node/gke-kubeinaction-default-pool-a1156362-rqmk   Node gke-kubeinaction-default-pool-a1156362-rqmk status is now: NodeReady
Warning   FailedMount               pod/kubia-manual                                   MountVolume.SetUp failed for volume "default-token-p7s2x" : couldn't propagate object cache: timed out waiting for the condition
Normal    Pulling                   pod/kubia-manual                                   Pulling image "mattfeng/kubia:0.1.0"
```

- `Error dialing backend: EOF` when using `kubectl exec -it <pod> sh`
  - Reason: still unknown, related to inability to connect to node.
  - Related: https://gitlab.com/gitlab-org/gitlab-runner/-/issues/3247
  - Resolution: `gcloud compute instances reset <node_name>`


## Questions

- GKE Console: What does `Status: Unknown` mean in the node pools section?
