# azure-samples

## Terraform Block
The *terraform {}* block contains Terraform settings, including the required providers Terraform will use to provision your infrastructure. For each provider, the source attribute defines an optional hostname, a namespace, and the provider type. Terraform installs providers from the Terraform Registry by default. In this example configuration, the azurerm provider's source is defined as *hashicorp/azurerm*, which is shorthand for *registry.terraform.io/hashicorp/azurerm*.
---
You can also define a version constraint for each provider in the required_providers block. The version attribute is optional, but we recommend using it to enforce the provider version. Without it, Terraform will always use the latest version of the provider, which may introduce breaking changes.

To learn more, reference the [provider source documentation.](https://developer.hashicorp.com/terraform/language/providers/requirements)

## Providers
The provider block configures the specified provider, in this case azurerm. A provider is a plugin that Terraform uses to create and manage your resources. You can define multiple provider blocks in a Terraform configuration to manage resources from different providers.

## Resource
Use resource blocks to define components of your infrastructure. A resource might be a physical component such as a server, or it can be a logical resource such as a Heroku application.

Resource blocks have two strings before the block: the resource type and the resource name. In this example, the resource type is azurerm_resource_group and the name is rg. The prefix of the type maps to the name of the provider. In the example configuration, Terraform manages the azurerm_resource_group resource with the azurerm provider. Together, the resource type and resource name form a unique ID for the resource. For example, the ID for your network is azurerm_resource_group.rg.

## Terraform must authenticate to Azure to create infrastructure.
Create a Service Principal
```bash
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<SUBSCRIPTION_ID>"
```

Set your environment variables
```bash
 export ARM_CLIENT_ID="<APPID_VALUE>"
 export ARM_CLIENT_SECRET="<PASSWORD_VALUE>"
 export ARM_SUBSCRIPTION_ID="<SUBSCRIPTION_ID>"
 export ARM_TENANT_ID="<TENANT_VALUE>"
```

## Terraform Commands Cheat Sheet:

### Format and validate the configuration.

Format your Terraform configuration files using the HCL language standard.

```bash
terraform fmt
```
You can also make sure your configuration is syntactically valid and internally consistent by using the terraform validate command.
Validate the configuration files in your directory and does not access any remote state or services. terraform init should be run before this command.

```bash
terraform validate
```
Useful in automation CI/CD pipelines, the check flag can be used to ensure the configuration files are formatted correctly, if not the exit status will be non-zero. If files are formatted correctly, the exit status will be zero
```bash
terraform fmt --check
```

### Initialize Your Directory

In order to prepare the working directory for use with Terraform, the terraform init command performs Backend Initialization, Child Module Installation, and Plugin Installation.

```bash
terraform init
```
 Initialize the working directory, don’t hold a state lock during backend migration.
```bash
terraform init -lock=false
```
Reconfigure a backend, and attempt to migrate any existing state.
```bash
terraform init -migrate-state
```

### Download and Install Modules

Note this is usually not required as this is part of the terraform init command.

Download and installs modules needed for the configuration.

```bash
terraform get
```
Check the versions of the already installed modules against the available modules and installs the newer versions if available
```bash
terraform get -update
```

### Plan Your Infrastructure

Plan will generate an execution plan, showing you what actions will be taken without actually performing the planned actions.

```bash
terraform plan
```
Save the plan file to a given path. Can then be passed to the terraform apply command.

```bash
terraform plan -out=<path>
```

Create a plan to destroy all objects rather than the usual actions

```bash
terraform plan -destroy
```
### Deploy Your Infrastructure