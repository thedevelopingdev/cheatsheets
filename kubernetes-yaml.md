### `Pod`
```yaml
apiVersion: v1
kind: Pod
spec:
  containers:
    - name: kubia
      ports:
        - name: http            # named ports
          containerPort: 8080
  volumes:
    - name: a_git_repo
      gitRepo:
        repository: https://github.com/luksa/kubia-website-example.git
        revision: master
        directory: .  # clone into the root of the volume
    - name: a_gce_disk
      gcePersistentDisk:
        pdName: gce_disk_name
        fsType: ext4
```

### Liveness Probe
```yaml
# pod-liveness.yaml
apiVersion: v1
kind: Pod
spec:
  containers:
    - image: user/image
      name: some-name
      livenessProbe:
        httpGet:
          path: /
          port: 8080
        initialDelaySeconds: 60
```

### `ReplicationController`
```yaml
spec:
  replicas: 3
  # optional; kubernetes can extract the selector from the pod template
  selector:
    label_key: label_value
  template:
    metadata:
      labels:
        label_key: label_value
    spec:
      containers:
        - name: container_name
          image: user/image
```

### `ReplicaSet`
```yaml
# ... metadata etc ...
spec:
  replicas: 3
  selector:
    matchExpressions:
      - key: label_name
        operator: In # In, NotIn, Exists, DoesNotExist
        values:
          - label_value
# ... template stuff ...
```

### `Service`
```yaml
spec:
  selector:
    app: app_name_label
  ports:
    - name: port_name
      port: 80            # the port the service will be available on
      targetPort: 8080    # the container port the service will forward to
    - name: port_name_2
      port: 443
      targetPort: 8443
```

### `PersistentVolume`
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mongodb-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
    - ReadOnlyMany
  persistentVolumeReclaimPolicy: Retain
  gcePersistentDisk:
    pdName: gce_disk_name
    fsType: ext4
```

### `StorageClass`
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast
provisioner: kubernetes.io/gce-pd     # the provisioner to use
parameters:                           # parameters passed to the provisioner
  type: pd-ssd
  zone: us-central1-c
```