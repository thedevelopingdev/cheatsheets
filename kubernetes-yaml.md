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

* Find the full set of Linux kernel capabilities [here](https://man7.org/linux/man-pages/man7/capabilities.7.html).

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-name
spec:
  serviceAccountName: default
  restartPolicy: Always
  containers:
    - name: kubia
      image: some/image
      command: ["<override command>"]  # equivalent to ENTRYPOINT
      args: ["[arg1]", "[arg2]"]       # equivalent to CMD
      ports:
        - name: http                   # named ports
          containerPort: 8080
      env:                             # environment variables
        - name: FIRST_VAR
          value: "first"
        - name: SECOND_VAR
          valueFrom:
            configMapKeyRef:
              optional: false
              name: <config map name>
              key: <key in config map>
      volumeMounts:                     # volume mounts
        - name: <++>
          mountPath: <++>
      resources:                        # resource requests
        requests:
          cpu: 200m
          memory: 10Mi
        limits:
          cpu: 1
          memory: 20Mi
      livenessProbe:                    # liveness probes
        httpGet:
          path: /
          port: 8080
        initialDelaySeconds: 60
      securityContext:                  # settings such as runAsUser
        runAsUser: 405                  # 405 = guest user on Alpine linux
        runAsNonRoot: true
        readOnlyRootFilesystem: true    # disallow writing to container
        capabilities:                   # Linux kernel capabilities
          add:
            - SYS_PTRACE
  volumes:
    - name: <++>
      gitRepo:
        repository: <++>
        revision: <++>    # the branch
        directory: <++>   # where in the volume to clone into
    - name: <++>
      gcePersistentDisk:
        pdName: <++>
        fsType: ext4
    - name: <++>
      persistentVolumeClaim:
        claimName: <++>
```

### `ReplicationController`

```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: <++>
  labels:
    <++>: <++>
    <++>: <++>
spec:
  replicas: 3
  selector:                     # selector is optional; kubernetes can extract
    label_key: label_value      # the selector from the pod template
  template:
    # same as Pod spec here.
```

### `ReplicaSet`

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: <++>
  labels:
    <++>: <++>
    <++>: <++>
spec:
  replicas: 3
  selector:
    matchExpressions:
      - key: label_name
        operator: In        # In, NotIn, Exists, DoesNotExist
        values:
          - label_value
  template:
    # same as Pod spec here.
```

### `Service`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: <++>
spec:
  type: <++>              # NodePort, LoadBalancer
  selector:
    <++>: <++>
  ports:
    - name: <++>          # Remember that a Service is ip + port
      port: <++>          # port is the port the Service is associated with
      targetPort: <++>    # targetPort is the port the app is listening on 
```

### `PersistentVolume`

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: <++>
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

### `PersistentVolumeClaim`

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: <++>
spec:
  storageClassName: <++>
  resources:
    requests:
      storage: <++>
  accessModes:
    - <++>                  # ReadWriteOnce, ReadOnlyMany, ReadWriteMany
```

### `StorageClass`

See [StorageClass#Parameters](https://kubernetes.io/docs/concepts/storage/storage-classes/#parameters) for details on the parameters available for each provisioner.

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: <++>
provisioner: <++>     # the provisioner to use
parameters:           # parameters passed to the provisioner
  <++>: <++>
```

#### Google Cloud

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: <++>
provisioner: kubernetes.io/gce-pd
parameters:
  type: <++>              # pd-ssd or pd-standard
  fstype: <++>            # ext4 or xfs
```

### `Deployment`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: kubia
spec:
  replicas: 3
  selector:
    matchLabels:
      <++>: <++>
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
* **Note that improperly formatted keys will not have associated environment
  variables.**

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

Mounting configurations as volumes via `ConfigMaps` has the benefit that the
containers will receive these updates, although the process has to  be
programmed to detect these changes and apply them.

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

Because these files don't get automatically updated by Kubernetes, it's better
to mount full volumes, and then use symlinks to place the config files in the
directories where they are needed.

Alternatively, it is better to use immutable infrastructure and not modify
`ConfigMaps` of running `Pods`.

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

### Cluster security: `Roles` and `RoleBindings`

#### `Roles`

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: <namespace here>
  name: <role name here>
rules:
  - apiGroups: [""]           # part of the core apiGroup
    verbs: ["get", "list"]
    resources: ["services"]   # plurals are required!
```

- Valid verbs:
  - Single resource
    - `get`, `watch`
    - `create`
    - `update`
    - `patch`
    - `delete`
  - Collections
    - `list`
    - `deleteCollection`

#### `RoleBindings`

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: test
  namespace: foo
roleRef:                                  # binds to a single Role
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  # this role refers to the one in the same namespace as the rolebinding
  name: service-reader
subjects:
- kind: ServiceAccount
  name: default
  namespace: foo
```
