# Terraform Hello World

## Module Overview

- Basic Terraform configuration
- Terraform init
- Terraform Apply
- Terraform Destroy

## Create a simple Terraform configuration

A Terraform configuration is a file or set of files that declare the resource to be created. Terraform configurations are written in Hashicorp Configuration Language (HCL) or JSON. For this workshop, we will work exclusively in HCL. For detailed specs on [HCL](https://www.terraform.io/docs/configuration/index.html).

Create a directory named vote-app.

```
mkdir vote-app
cd vote-app
```

Create a file named `main.tf` and copy in the following configuration. This configuration includes a single resource block that creates an Azure Resource Group.

AUTHORING NOTE: Expand on HCL format here.

```
resource "azurerm_resource_group" "vote-app" {
  name     = "vote-app"
  location = "eastus"
}
```

Before creating the resource with Terraform, the current working directory must be initialized. The initialization process ensures that the proper Terraform provider plugins, Azure, in this case, have been downloaded.

Use `terraform init` to initialize the working directory.

```
terraform init
```

Use the `terraform apply` command to run the configuration. Terraform will produce a plan that indicates all resources that will be created, modified, or destroyed. You can then accept the plan to created the resources.


```
terraform apply
```

To validate resource creation, use the Azure CLI `az group list` command.

```
az group list -o table
```

Now that the Terraform configuration has been applied, the configuration can also be destroyed using the `terraform destroy` command.

```
terraform destroy
```

## Next Module

In the next module, you will learn about how to link resources together using String Interpolation. You will also learn about the `terraform plan` command.

Module 3: [String Interpolation](../3-string-interpolation)