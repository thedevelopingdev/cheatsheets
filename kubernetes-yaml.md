# Kubernetes manifests (aka `.yaml` files) cheatsheet

## Contents

<!-- vim-markdown-toc GFM -->

* [Document conventions](#document-conventions)
  * [Syntax](#syntax)
* [Applications](#applications)
  * [`Pod`](#pod)
  * [`ReplicationController`](#replicationcontroller)
  * [`ReplicaSet`](#replicaset)
  * [`Deployment`](#deployment)
  * [`StatefulSet`](#statefulset)
  * [`Service`](#service)
  * [`Job`](#job)
  * [`CronJob`](#cronjob)
* [Storage](#storage)
  * [`PersistentVolume`](#persistentvolume)
  * [`StorageClass`](#storageclass)
  * [`PersistentVolumeClaim`](#persistentvolumeclaim)
    * [Google Cloud](#google-cloud)
* [Configuration](#configuration)
  * [`ConfigMap`](#configmap)
  * [`Secrets`](#secrets)
  * [Image pull `Secrets` i.e. accessing private Docker registries](#image-pull-secrets-ie-accessing-private-docker-registries)
* [Cluster security](#cluster-security)
  * [`Roles`](#roles)
  * [`RoleBindings`](#rolebindings)

<!-- vim-markdown-toc -->

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
        defaultMode: 0700              # file permissions for mounted files
        items:
          - key: <configmap key>       # mount specific keys
            path: <mounted file>
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
  name: my-replica-set
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
  name: my-deployment
spec:
  minReadySeconds: 60    # the number of seconds the containers need to
                         # stay alive before a Pod is marked as available
  replicas: 3
  selector:
    matchLabels:
      <label key>: <label value>
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
    type: RollingUpdate
  template:
    metadata: (!-- Pod.metadata --)
    spec: (!-- Pod.spec --)
```


### `StatefulSet`

```yaml
apiVersion: apps/v1beta1
kind: StatefulSet
metadata:
  name: my-stateful-set
spec:
  serviceName: (!-- Service.metadata.name --) # valid reference to a headless service
  replicas: 1
  template:
  volumeClaimTemplates:

```

### `Service`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service        # the name the service will be accessed by
spec:
  type: ClusterIP         # NodePort, LoadBalancer
  clusterIP: None         # Create a headless service
  selector:
    <++>: <++>
  ports:
    - name: <++>          # Remember that a Service is ip + port
      port: <++>          # port is the port the Service is associated with
      targetPort: <++>    # targetPort is the port the app is listening on 
```


### `Job`

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: my-batch-job
spec:
  template:
    metadata:
      labels:
        app: batch-job    # A label is needed for the Job to know whether or not it has already been scheduled.
    spec:
      restartPolicy: OnFailure
      containers:
        - name: main
          image: repo/myimage
```

### `CronJob`

```yaml
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: my-cron-job
spec:
  schedule: "* * * * *"
  jobTemplate:
    spec: include(Job.spec)
```

## Storage

### `PersistentVolume`

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-persistent-volume
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

- Note that `PersistentVolumes` require manually provisioned disks from the
  cluster administrator, which undesirably ties the software configuration to
  the infrastructure. The recommended way to attach persistent disks is through
  `StorageClasses` (which automatically provision the disks) and
  `PersistentVolumeClaims` (a level of abstraction of volume names that allows
  for different kinds of volumes in the backend).

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

- See **`StorageClass#Parameters`**
  ([original](https://kubernetes.io/docs/concepts/storage/storage-classes/#parameters),
  [archive.is](https://archive.is/XI3ib)) for details on the parameters
  available for each provisioner.

### `PersistentVolumeClaim`

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  storageClassName: (!-- StorageClass.metadata.name --)
  resources:
    requests:
      storage: 10Gi
  accessModes:
    - ReadWriteOnce              # ReadWriteOnce, ReadOnlyMany, ReadWriteMany
```

- See the Google Compute Engine documentation
  ([original](https://cloud.google.com/compute/docs/disks),
  [archive.is](https://archive.is/Uc6f6)) for the minimum and maximum allowed
  requests for GCP provisioned persistent disks.

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

## Cluster security

### `Roles`

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

### `RoleBindings`

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
