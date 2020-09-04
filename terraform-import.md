# Terraform importing cheatsheet

The default Terraform workflow assumes you create brand new Terraform infrstructure from the very beginning. However, this often isn't the case.

The `terraform import` command doesn't automatically create configurations, though it does load `.tfstate`.

Importing infrastructure has 5 main steps:
1. Identify the existing infrastructure to be imported.
2. Import infrastructure into the Terraform state.
3. Write Terraform configuration that matches that infrastructure.
4. Review the Terraform plan to ensure the configuration matches the expected state and infrstructure.
5. Apply the configuration to update your Terraform state.