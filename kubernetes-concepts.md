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

## `PersistentVolume` and `PersistentVolumeClaim`

### Manual provisioning
Manual provisioning consists of creating `PersistentVolumes` manually, and then creating `PersistentVolumeClaims` to bind volumes to `Pods`.

![manual provisioning](./kube-assets/pv-pvc.png)

### Dynamic provisioning
Dynamic provision entails the dynamic creation of `PersistentVolumes` by a provisioner as `PersistentVolumeClaims` are created. The `PersistentVolumeClaims` reference a `StorageClass` in order to dictate what type of `PersistentVolume` should be created.

![dynamic provisioning](./kube-assets/pv-pvc-dp.png)

## References
* All images are figures taken from [Marko Luksa's wonderful book, **Kubernetes in Action**](https://www.amazon.com/Kubernetes-Action-Marko-Luksa/dp/1617293725/).