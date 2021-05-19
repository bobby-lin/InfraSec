# Terraform Basics

## Commands

Run `terraform init` if there are changes in configuration files or pulling changes from version control.

Run `terraform plan` to see what are the resources that will be created.

Run `terraform plan --out=[output plan name]` to output a plan that you can apply.

Run `terraform apply` to execute the plan in the terraform files.

Run `terraform apply [plan name]` to execute a specific plan.

Run `terraform destroy` to remove the created resources from the terraform applied actions. Note that you should not run this command in `PROD` environment.