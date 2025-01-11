---
title: Terraform Multiple Provider Blocks on Azure Cloud
description: Learn how to use multiple Terraform provider blocks on Azure Cloud
---

# Multiple Provider Configurations

## Introduction
- Understand and Implement Multiple Provider Configurations

## How to define multiple provider configuration of same Provider?

```t
# Provider-1 for EastUS (Default Provider)
provider "azurerm" {
  features {}
}

# Provider-2 for WestUS Region
provider "azurerm" {
  features {
    virtual_machine {
      delete_os_disk_on_deletion = false # This will ensure when the Virtual Machine is destroyed, Disk is not deleted, default is true and we can alter it at provider level
    }
  }
  alias = "provider2-westus"
  #client_id = "XXXX"
  #client_secret = "YYY"
  #environment = "german"
  #subscription_id = "JJJJ"
}
```

## How to reference the non-default provider configuration in a resource?
```t
# Provider-2: Create a resource group in WestUS region - Uses "provider2-westus" provider
resource "azurerm_resource_group" "myrg2" {
  name = "myrg-2"
  location = "West US"
    #<PROVIDER NAME>.<ALIAS NAME>
  provider = azurerm.provider2-westus
}
```

## Execute Terraform Commands
```t
# Initialize Terraform
terraform init

# Validate Terraform Configuration Files
terraform validate

# Generate Terraform Plan
terraform plan

# Create Resources
terraform apply -auto-approve

# Verify the same
1. Verify the Resource Group created in EastUS region
2. Verify the Resource Group created in WestUS region
```


## References
- [Provider Meta Argument](https://www.terraform.io/docs/configuration/meta-arguments/resource-provider.html)
- [Azure Provider - Argument and Attribute References](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs)
