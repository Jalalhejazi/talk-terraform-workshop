# Terraform Hello World

## Module Overview

In this module, you will write your first Terrafrorm configuration. A Terraform configuration is a file or set of files that declare infrastructure resource to be created. Terraform configurations are written in Hashicorp Configuration Language (HCL) or JSON. For this workshop, you will work exclusively in HCL.

## Create a simple Terraform configuration

Create a directory named `terraform-modules` and then a directory under that named `hello-world`.

Create a file named `main.tf` inside of the `hello-world` directory, and copy in the following configuration. This configuration uses the Azure Terraform provider to create an Azure Resource Group.

```
resource "azurerm_resource_group" "hello-world" {
  name     = "hello-world"
  location = "eastus"
}
```

Before creating the resource, the `hello-world` directory must be initialized. The initialization process ensures that the proper Terraform provider plugins, Azure, in this case, have been downloaded.

Use `terraform init` to initialize the directory.

```
terraform init
```

Use the `terraform apply` command to run the configuration. Terraform will produce a plan that indicates all resources that will be created, modified, or destroyed. You can then accept the plan to created the resources.

```
terraform apply
```

To validate resource creation, use the Azure CLI `az group list` command.

```
$ az group list -o table

Name             Location    Status
---------------  ----------  ---------
hello-world      eastus      Succeeded
```

Now that the Terraform configuration has been applied, the configuration can be destroyed using the `terraform destroy` command.

```
terraform destroy
```

## Next Module

In the next module, you will learn about how to link resources together using String Interpolation. You will also learn about the `terraform plan` command.

Module 3: [String Interpolation](../03-string-interpolation)