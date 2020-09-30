# Terraform cheatsheet

## General
* The `self_link` attribute in resources is a unique link that refers to the resource itself. It can be used as a unique reference by other resources.
* The `depends_on` attribute can be used to list out explicit dependencies between resources that Terraform may not be able to deduce automatically, e.g. those defined in the application logic, rather than in the infrastructure.
* **Destroy provisioners** are provisioners that only run when Terraform runs the `destroy` operation. These are useful for cleanup purposes. The [provisioners documentation](https://www.terraform.io/docs/provisioners/) has more details.

## Output
Outputs can be used to tell Terraform to output certain values, e.g. for logging.

```
output "[tf_output_name]" {
  value = google_compute_address.vm_static_ip.address
}
```

## Google Cloud

### Google Compute Engine

#### VM instances

```
resource "google_compute_instance" "[tf_vm_name]" {
  name = "[gcp_name]"
  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-9"
    }
  }

  network_interface {
    access_config {

    }
  }
}
```

#### Static IP address
```
resource "google_compute_address" "[tf_name]" {
  name = "[gcp_name]"
}
```

### Google Kubernetes Engine

#### Clusters
* Source: https://www.terraform.io/docs/providers/google/r/container_cluster.html

#### Node Pool
* Source: https://www.terraform.io/docs/providers/google/r/container_node_pool.html
- Custom machine types: https://cloud.google.com/compute/docs/instances/creating-instance-with-custom-machine-type#gcloud
  - https://archive.is/e4vGC

```
resource "google_container_node_pool" "[tf_name]" {
  name       = "gke_node_pool_name"
  location   = "us-central1"
  cluster    = google_container_cluster.[tf_name].name
  node_count = 1

  node_config {
    preemptible = true
    machine_type = "[gcp machine type]"

    oauth_scopes = [

    ]
  }
}
```
