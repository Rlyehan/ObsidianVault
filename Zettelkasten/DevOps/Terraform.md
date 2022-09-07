222022-08-03

Style: #Research 

Domain: #Iac

Tags: #DevOps 

# Terraform
### Installation MacOS with homebew
```
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
brew update
brew upgrade hashicorp/tap/terraform
terraform -help
```

if Docker is installed, you can try it out like:
```
open -a Docker
mkdir learn-terraform-docker-container
cd learn-terraform-docker-container
```

in there we create a main.tf file with the following content:
```
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 2.13.0"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "tutorial"
  ports {
    internal = 80
    external = 8000
  }
}

```
we can build the infrastructure with:
```
terraform init
terraform apply
```

after we are done we can destroy the infrastructure with:
```
terraform destroy
```

### Testing Terraform
[Unit tests, integration tests, end-to-end tests](https://terratest.gruntwork.io/docs/testing-best-practices/unit-integration-end-to-end-test/)
[Quick start](https://terratest.gruntwork.io/docs/getting-started/quick-start/)



## Terraform Sections

**Terraform Block**
The `terraform {}` block contains Terraform settings, including the required providers Terraform will use to provision your infrastructure. For each provider, the `source` attribute defines an optional hostname, a namespace, and the provider type. Terraform installs providers from the [Terraform Registry](https://registry.terraform.io/) by default. In this example configuration, the `azurerm` provider's source is defined as `hashicorp/azurerm`, which is shorthand for `registry.terraform.io/hashicorp/azurerm`.

You can also define a version constraint for each provider in the `required_providers` block. The `version`attribute is optional, but we recommend using it to enforce the provider version. Without it, Terraform will always use the latest version of the provider, which may introduce breaking changes.

To learn more, reference the [provider source documentation](https://www.terraform.io/docs/language/providers/requirements.html).

**Providers**
The `provider` block configures the specified provider, in this case `azurerm`. A provider is a plugin that Terraform uses to create and manage your resources. You can define multiple provider blocks in a Terraform configuration to manage resources from different providers.

**Resource**
Use `resource` blocks to define components of your infrastructure. A resource might be a physical component such as a server, or it can be a logical resource such as a Heroku application.

Resource blocks have two strings before the block: the resource type and the resource name. In this example, the resource type is `azurerm_resource_group` and the name is `rg`. The prefix of the type maps to the name of the provider. In the example configuration, Terraform manages the `azurerm_resource_group` resource with the`azurerm` provider. Together, the resource type and resource name form a unique ID for the resource. For example, the ID for your network is `azurerm_resource_group.rg`.

Resource blocks contain arguments which you use to configure the resource. The [Azure provider documentation](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs)documents supported resources and their configuration options, including [azurerm_resource_group](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/resource_group) and its supported arguments.


___
# References
