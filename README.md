## Manage Resources in Terraform State
In this tutorial, you will create an AWS instance and security group, examine a state file, and then manipulate resources to observe how vital state is to your Terraform operations.

### Examine State with CLI
- Run `terraform show` to get a human-friendly output of the resources contained in your state.
- Run `terraform state list` to get the list of resource names and local identifiers in your state file.

### Replace a resource with CLI
The -replace flag allows you to target specific resources and avoid destroying all the resources in your workspace just to fix one of them.
- `terraform plan -replace="aws_instance.example"`
- `terraform apply -replace="aws_instance.example`

### Move a resource to a different state file
HashiCorp recommends only performing these advanced operations as the last resort.
- `terraform state mv -state-out=../terraform.tfstate aws_instance.example_new aws_instance.example_new`
- `terraform state list`

### Remove a resource from state
The terraform state rm subcommand removes specific resources from your state file. This does not remove the resource from your configuration or destroy the infrastructure itself.
- `terraform state rm aws_security_group.sg_8080`
- `terraform state list`

Run `terraform import` to bring this security group back into your state file. Removing the security group from state did not remove the output value with its ID, so you can use it for the import.

### Refresh modified infrastructure
The `terraform refresh` command updates the state file when physical resources change outside of the Terraform workflow.
- `aws ec2 terminate-instances --instance-ids $(terraform output -raw instance_id)`

Terraform automatically performs a refresh during the plan, apply, and destroy operations. All of these commands will reconcile state by default, and have the potential to modify your state file.

<img width="1603" alt="aws" src="https://user-images.githubusercontent.com/33342822/150648933-4845418b-937d-4cd5-93df-596d8306d528.png">

### NOTE
- .terraform folder is created after `terraform init`
- terraform.tfstate file is created after `terraform apply`

### Reference
https://learn.hashicorp.com/tutorials/terraform/state-cli
