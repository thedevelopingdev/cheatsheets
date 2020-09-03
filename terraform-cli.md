# Terraform command line interface cheatsheet

* Refresh Terraform's local state with the cloud's state.
```
terraform refresh
```

* View Terraform output variables.
```
terraform output
```

* Plan out Terraform changes and save plan to a file.
```
terraform plan -out <plan file name>
```

* To force Terraform to delete and recreate a resource, use the `taint` command.
```
terraform taint <resource type>.<resource tf name>
```