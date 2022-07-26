<!-- BEGIN_TF_DOCS -->
# app-service

This module manages Azure App Service.

<-- This file is autogenerated, please do not change. -->

## Requirements

| Name | Version |
|------|---------|
| terraform | >=1.0 |
| azurerm | >=3.11.0 |

## Providers

| Name | Version |
|------|---------|
| azurerm | >=3.11.0 |

## Resources

| Name | Type |
|------|------|
| azurerm_linux_function_app.linux_function_app | resource |
| azurerm_service_plan.service_plan | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| linux_function_app | resource definition, default settings are defined within locals and merged with var settings | `any` | `{}` | no |
| service_plan | resource definition, default settings are defined within locals and merged with var settings | `any` | `{}` | no |

## Outputs

| Name | Description |
|------|-------------|
| linux_function_app | azurerm_linux_function_app |
| service_plan | azurerm_service_plan |

## Examples

```hcl
module "app_service" {
  source = "registry.terraform.io/T-Systems-MMS/app-service/azurerm"
  service_plan = {
    spl-service-web = {
      location            = "westeurope"
      resource_group_name = "service-env-rg"
      sku_name            = "B1"
      tags = {
        service = "service_name"
      }
    }
  }
  linux_function_app = {
      app-service-web = {
      location            = "westeurope"
      resource_group_name = "service-env-rg"
      service_plan_id             = module.app_service.service_plan["spl-service-web"].id
      storage_account_name        = module.storage.storage_account["service"].name
      storage_account_access_key  = module.storage.storage_account["service"].primary_access_key
      functions_extension_version = "~3"
      https_only                  = false
      app_settings                = {
        WEBSITE_NODE_DEFAULT_VERSION = "10.14.1"
      }
      site_config                 = {
        always_on = true
      }
      tags = {
        service = "service_name"
      }
    }
  }
}


```
<!-- END_TF_DOCS -->