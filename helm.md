# Helm cheatsheet

## Common commands

#### Install a Helm chart with custom values

```sh
helm install --values=<my-values.yaml> <chartname>
```

#### Keep track of a release's state

```sh
helm status <release name>
```


#### Get the values that can be customized

```sh
helm show values <chart>
```


