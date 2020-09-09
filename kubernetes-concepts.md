# Cheatsheet of core Kubernetes concepts

## Taxonomy of Kubernetes resources (a.k.a. components)
* `Pod`
* `ReplicationController`
* `ReplicaSet`
* `DaemonSet`
* `Job`
* `Endpoints`
* `Service`
* `Deployment`
* `Namespace`
* `PersistentVolume`
* `PersistentVolumeClaim`
* `StorageClass`
* `ConfigMap`
* `Secret`
  * `generic`
  * `tls`
* `List`

## Kubernetes internals

### Architecture

#### Control Plane
- The `Control Plane` stores and manages the state of the cluster.
- Components
  - `etcd` distributed persistent storage
    - A fast, distributed, and consistent key-value store.
    - Uses an optimistic locking mechanism for concurrency control.
    - Uses RAFT for consensus.
  - The `API Server`
    - When a request wants to create, modify, or delete a resource, the
      request must go through `Admission Control`, which will update the
      missing fields in the request, modify fields as necessary, and
      perhaps reject the request.
    - `Admission Control` plugins include `ServiceAccount` (adding a default
      service account).
  - The `Scheduler`
    - Watches updates for any newly created `Pods`, and then modifies the `Pod`
      definition with which node it should run on. Then, the `Kubelets` will
      receive these updates and run `Pods` as necessary.
    - Two steps to scheduling: **filtering** to find acceptable nodes, then
      **prioritizing** the select the best node.
    - It is possible to use multiple different schedulers.
  - The `Controller Manager`
    - Ensures that the actual state of the system converges toward the desired
      state
    - Examples (not comprehensive):
      - ReplicaSet, DaemonSet, Job controllers
      - Deployment controller
      - StatefulSet controller
      - Node controller
      - Service controller
      - Endpoints controller
      - Namespace controller
      - PersistentVolume controller

#### Worker nodes
- Components (on each worker node)
  - The `Kubelet`
    - Responsible for everything running on a worker node.
      - Registering the node with the `API Server`
      - Watches the `API Server` and starts `Pod` containers.
      - Monitors running `Pods` and reports status.
      - Runs the readiness and liveness probes.
      - Terminates `Pods` when instructed.
  - The `Kubernetes Service Proxy` (aka `kube-proxy`)
  - The `Container Runtime` (e.g. Docker, rkt)

#### Add-on components
- The `Kubernetes DNS Server`
- The `Dashboard`
- An `Ingress Controller`
- `Heapster`
- The `Container Network Interface` network plugin

### How controllers cooperate: An example `Deployment`

![components watching](./kube-assets/components-watching.png)
![chain of events deployment](./kube-assets/chain-of-events-deployment.png)

### How Pods work
- `Pods` require an infrastructure container to hold all the shared namespaces
  between the containers, in particular the network.
- This infrastructure container is called `pause`.
- If this infrastructure container is killed, the `Pod` needs to be recreated.

### Inter-pod networking
- Kubernetes does not provide inter-pod networking functionality; this is
  provided by the `Container Network Interface` plugin.
- Kubernetes, however, mandates that this network ensure that no NAT occurs
  with inter-pod and inter-node communication.
- Questions: How does bridge networking work exactly? How are packets
  encapsulated and de-encapsulated with software defined networks?

![container networking](./kube-assets/container-networking.png)

### How `Services` work
- Everything related to `Services` is handled by the `kube-proxy`.
- The IP address used by a `Service` is virtual, and is never listed as the
  source or the destination in a network packet that leaves a node.

![what kube proxy does](./kube-assets/kube-proxy.png)

### Running highly available clusters
- Use **leader election** for non-horizontally scalable apps.

## `PersistentVolumes` and `PersistentVolumeClaims`

### Manual provisioning
Manual provisioning consists of creating `PersistentVolumes` manually, and then
creating `PersistentVolumeClaims` to bind volumes to `Pods`.

![manual provisioning](./kube-assets/pv-pvc.png)

### Dynamic provisioning
Dynamic provision entails the dynamic creation of `PersistentVolumes` by
a provisioner as `PersistentVolumeClaims` are created. The
`PersistentVolumeClaims` reference a `StorageClass` in order to dictate what
type of `PersistentVolume` should be created.

![dynamic provisioning](./kube-assets/pv-pvc-dp.png)

## References
* All images are figures taken from Marko Luksa's book, **Kubernetes
  in Action**.
