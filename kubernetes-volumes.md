# Kubernetes volumes cheatsheet

* Use a `gcePersistentDisk` for a volume.
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: sample-pod
spec:
    containers:
        ...
    volumes:
        - name: sample-volume
          gcePersistentDisk:
              pdName: sample-gce-disk
              fsType: ext4
    
