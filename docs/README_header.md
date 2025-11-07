# Workspaces Factory Terraform Module

Workspaces Factory module which manages configuration and life-cycle
of your Terraform workspaces.

## Permissions

### HCP Terraform Permissions

To manage resources, provide a user token from an account with
appropriate permissions. This user should have the `Manage Projects`, `Manage Workspaces`,
permission. Alternatively, you can use a token from a team instead of a user token.

## Authentication

### HCP Terraform Authentication

The HCP Terraform provider requires a HCP Terraform/Terraform Enterprise API token in
order to manage resources.

There are several ways to provide the required token:

* Set the `token` argument in the provider configuration. You can set the token argument in the provider configuration. Use an
input variable for the token.
* Set the `TFE_TOKEN` environment variable. The provider can read the TFE\_TOKEN environment variable and the token stored there
to authenticate.

## Features

Manages configuration and life-cycle of HCP Terraform resources:

* Workspaces
  * Workspace Settings
  * Workspace Run Rask
* Variables

## Usage example

```hcl
module "project" {
  source            = "app.terraform.io/benoitblais-hashicorp/workspacesfactory/tfe"
  version           = "0.0.0"
  name              = "workspace_name"
  organization_name = "organization_name"
  project_name      = "project_name"
}
```
