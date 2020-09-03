# `kubectl` Cheatsheet

* Get custom columns; this one will show which nodes pods are running on.
* Find the custom column names by using "-o json"
```bash
$ k get po -o custom-columns='NAME:metadata.name,IP:status.hostIP'
```

* List pods with labels
```bash
$ k get po --show-labels
$ k get po -L <label names>
```

* Add labels to pods
```bash
$ k label po <pod name> <labels> [--overwrite]
```

* Filter pods by label
```bash
$ k get po -l <label>=<value>
```

* explain a resource and its API fields
```bash
$ k explain <resource>
```

* port-forwarding to a pod for debugging
```bash
$ k port-forward <pod name> <local port>:<pod port>
```

* creating a namespace
```bash
$ k create ns <namespace name>
```

* obtain application log of a crash container
```bash
$ kubectl logs <podname> --previous
```

* Edit a running resource
```bash
$ kubectl edit <resource type> <resource name>
```

* Delete a `ReplicationController` without deleting `Pod`s
```
$ k delete rc kubia --cascade=false
```

* execute a command in a pod
  * the `--` signifies the end of command options for `kubectl`
```
$ k exec <pod name> -- <command> [args]
```

* create a temporary pod for debugging
  * Useful images include
    * `tutum/dnsutils`. for DNS related debugging
```
$ k run <pod name> --image=<image> --generator=run-pod/v1 --command -- sleep infinity
```

