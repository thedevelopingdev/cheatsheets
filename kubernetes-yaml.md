## General
* Refer to other environment variables in a variable's value by using the `$(VAR)` syntax.
```yaml
- name: FIRST_VAR
  value: "foo"
- name: SECOND_VAR
  value: "$(FIRST_VAR)bar"
```

## API resources

### `Pod`
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-name
spec:
  containers:
    - name: kubia
      image: some/image
      command: ["<override command>"]  # equivalent to ENTRYPOINT
      args: ["[arg1]", "[arg2]"]       # equivalent to CMD
      ports:
        - name: http                   # named ports
          containerPort: 8080
      env:
        - name: FIRST_VAR
          value: "first"
        - name: SECOND_VAR
          valueFrom:
            configMapKeyRef:
              optional: false
              name: <config map name>
              key: <key in config map>
      volumeMounts:
        - name: a_git_repo
          mountPath: /git_repo
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

### `Deployment`
```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: kubia
spec:
  replicas: 3
  template:
    metadata:
      name: kubia
      labels:
        app: kubia
    spec:
      containers:
        - image: some/image
          name: container_name
  minReadySeconds: 60
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
    type: rollingUpdate
```

## Configuration

### Defining a `ConfigMap`
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: fortune-config
  namespace: default
data:
  sleep-interval: "25"
```

### Defining `Secrets`
The reason for base64 encoding was not for encryption, but to support binary configuration files.

```yaml
apiVersion: v1
kind: Secret
# stringData is write only; Secrets are always base64 encoded
stringData:
  foo: plain text
data:
  key1: value2
  key2: value2
```

### Environment variables
* **Note that improperly formatted keys will not have associated environment variables.**

```yaml
spec:
  containers:
    - image: some-image
      envFrom:            # import all environment variables from `ConfigMap`
        - prefix: CONFIG_
          configMapRef:
            name: <config map name>
      args:
        - "$(CONFIG_SOME_VALUE)"
```

### Mounting configuration as a volume

Mounting configurations as volumes via `ConfigMaps` has the benefit that the containers will receive these updates, although the process has to  be programmed to detect these changes and apply them.

#### Selecting specific keys to mount in volume
```yaml
spec:
  volumes:
    - name: <config name>
      defaultMode: "6600"
      configMap:
        name: <configmap name>
        items:
          - key: <configmap key>
            path: <mounted file>
    - name: <secret name>
      secret:
        secretName: <secrets name>
```

#### Mounting the config volume without hiding default contents
```yaml
spec:
  containers:
    - image: some-image
      volumeMounts:
        - name: some-volume
          mountPath: /etc/specific-file.conf
          # specific file designated in volume definition
          subPath: specific-file-in-volume.conf
```
Because these files don't get automatically updated by Kubernetes, it's better to mount full volumes, and then use symlinks to place the config files in the directories where they are needed.

Alternatively, it is better to use immutable infrastructure and not modify `ConfigMaps` of running `Pods`.

### Image pull `Secrets` i.e. accessing private Docker registries
```yaml
apiVersion: v1
kind: Pod
spec:
  imagePullSecrets:
    - name: <docker-registry secret name>
  containers:
    - image: me/private:tag
      name: main
```
