# Kubernetes manifests (aka `.yaml` files) cheatsheet

## Document conventions

### Syntax
- `<++>`. Angle brackets imply that the value inside (e.g. `++`) is not a
literal and is instead a placeholder describing what values should replace
the entire tag (e.g. `name: <str>` would become `name: foo`, where `foo` is
the value that replaced the entire `<++>` tag, including the angle brackets).

## Applications

### `Pod`

- Because the `Pod` is the fundamental unit of computation in Kubernetes, its
  manifest is the most complete, since most other resources simply build upon
  it.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod                         # a descriptive name for the Pod
spec:
  serviceAccountName: default
  restartPolicy: Always

  # --- CONTAINERS ---
  containers:
    - name: ubuntu-container           # name of the container
      image: ubuntu:18.04              # Docker image

      # --- ENTRYPOINT ---
      command:                         # Docker ENTRYPOINT override
        - sleep
      args:                            # Dockerfile CMD override
        - "1"                          # must be strings
        - "$(ENV_MANUAL) world"        # variable interpolation works

      # --- OPEN PORTS ---
      ports:
        - name: http                   # named ports
          containerPort: 8080

      # --- ENVIRONMENT VARIABLES ---
      env:
        - name: ENV_MANUAL             # name of the env var
          value: "hello"               # value of the env var (must be string)
        - name: ENV_FROM_CM            # env vars can also be taken from ConfigMaps
          valueFrom:
            configMapKeyRef:
              optional: false
              name: some-cm            # name of ConfigMap
              key: key-in-some-cm      # key in ConfigMap "some-cm"
      envFrom:                         # a list of env vars can be loaded from a ConfigMap
        - prefix: CONFIG_              # a prefix will be appended to the env vars
          configMapRef:
            name: another-cm           # name of the ConfigMap to load from

      # --- VOLUME MOUNTS ---
      volumeMounts:
        - name: <++>
          mountPath: <++>
      
      # --- RESOURCE REQUESTS ---
      resources:
        requests:
          cpu: 200m
          memory: 10Mi
        limits:
          cpu: 1
          memory: 20Mi

      # --- HEALTH CHECKS ---
      livenessProbe:                   # liveness probe (only one!)
        initialDelaySeconds: 60
        periodSeconds: 20
        httpGet:
          path: /
          port: 8080
      # exec:                          # alt: execute a command
      #   command:
      #     - cat
      #     - /tmp/healthy
      # tcpSocket:                     # alt: open a TCP socket
      #   port: 8080

      # --- SECURITY ---
      securityContext:                 # settings such as runAsUser
        runAsUser: 405                 # 405 = guest user on Alpine linux
        runAsNonRoot: true
        readOnlyRootFilesystem: true   # disallow writing to container
        capabilities:                  # Linux kernel capabilities
          add:
            - SYS_PTRACE
  
  # --- VOLUMES ---
  volumes:
    - name: git-volume                 # name of the volume
      gitRepo:                         # git repositories can be volumes
        repository: orgname/config     # name of the repository
        revision: master               # branch to pull
        directory: .                   # where to save the repository in the volume
    - name: gce-pd-manual-volume
      gcePersistentDisk:
        pdName: my-gce-disk            # use a specific GCP disk
        fsType: ext4
    - name: pvc-volume
      persistentVolumeClaim:
        claimName: my-pvc              # name of the PVC
    - name: secrets-volume
      secret:                          # load secrets as volume, where keys are files
        secretName: my-secrets
    - name: configmap-volume
      configMap:
        name: my-configmap
```

- Find the full set of Linux kernel capabilities [here](https://man7.org/linux/man-pages/man7/capabilities.7.html).

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
  selector:              # selector is optional; kubernetes can extract
    <++>: <++>           # the selector from the pod template
  template:
    # (!-- Pod.spec --)
```

### `ReplicaSet`

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: <++>
  labels:
    <++>: <++>
spec:
  replicas: 3
  selector:
    matchExpressions:
      - key: <++>
        operator: In        # In, NotIn, Exists, DoesNotExist
        values:
          - <++>
  template:
    # (!-- Pod.spec --)
```

### `Deployment`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: kubia
spec:
  minReadySeconds: 60
  replicas: 3
  selector:
    matchLabels:
      <++>: <++>
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
    type: rollingUpdate
  template:
    # (!-- Pod.spec --)
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

## Storage

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

### `StorageClass`

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: <++>
provisioner: <++>     # the provisioner to use
parameters:           # parameters passed to the provisioner
  <++>: <++>
```

See **`StorageClass#Parameters`** ([original](https://kubernetes.io/docs/concepts/storage/storage-classes/#parameters), [archive.is](https://archive.is/XI3ib)) for details on the parameters available for each provisioner.

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

See the Google Compute Engine documentation ([original](https://cloud.google.com/compute/docs/disks), [archive.is](https://archive.is/Uc6f6)) for the minimum and maximum allowed requests for GCP provisioned persistent disks.

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

## Configuration

### `ConfigMap`
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: fortune-config
  namespace: default
data:
  sleep-interval: "25"
```

### `Secrets`

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

Trivia: the reason for base64 encoding was not for encryption, but to support binary configuration files.

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

### Cluster security

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
