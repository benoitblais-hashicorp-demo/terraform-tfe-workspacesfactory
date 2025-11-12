<!-- BEGIN_TF_DOCS -->
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
* Set the `TFE_TOKEN` environment variable. The provider can read the TFE\\_TOKEN environment variable and the token stored there
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

## Documentation

## Requirements

The following requirements are needed by this module:

- <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) (>= 1.13.0)

- <a name="requirement_tfe"></a> [tfe](#requirement\_tfe) (~>0.70)

## Modules

No modules.

## Required Inputs

The following input variables are required:

### <a name="input_name"></a> [name](#input\_name)

Description: (Required) Name of the workspace.

Type: `string`

### <a name="input_project_name"></a> [project\_name](#input\_project\_name)

Description: (Required) Name of the project where the workspace should be created.

Type: `string`

### <a name="input_vcs_repo_identifier"></a> [vcs\_repo\_identifier](#input\_vcs\_repo\_identifier)

Description: (Required) A reference to your VCS repository in the format '<vcs organization>/<repository>'.

Type: `string`

## Optional Inputs

The following input variables are optional (have default values):

### <a name="input_agent_pool_id"></a> [agent\_pool\_id](#input\_agent\_pool\_id)

Description: (Optional) The ID of an agent pool to assign to the workspace. Requires `execution_mode` to be set to `agent`. This value must not be provided if `execution_mode` is set to any other value.

Type: `string`

Default: `null`

### <a name="input_allow_destroy_plan"></a> [allow\_destroy\_plan](#input\_allow\_destroy\_plan)

Description: (Optional) Whether destroy plans can be queued on the workspace.

Type: `bool`

Default: `true`

### <a name="input_assessments_enabled"></a> [assessments\_enabled](#input\_assessments\_enabled)

Description: (Optional) Whether to regularly run health assessments such as drift detection on the workspace.

Type: `bool`

Default: `false`

### <a name="input_auto_apply"></a> [auto\_apply](#input\_auto\_apply)

Description: (Optional) Whether to automatically apply changes when a Terraform plan is successful.

Type: `bool`

Default: `true`

### <a name="input_auto_apply_run_trigger"></a> [auto\_apply\_run\_trigger](#input\_auto\_apply\_run\_trigger)

Description: (Optional) Whether to automatically apply changes for runs that were created by run triggers from another workspace.

Type: `bool`

Default: `true`

### <a name="input_description"></a> [description](#input\_description)

Description: (Optional) A description for the workspace.

Type: `string`

Default: `null`

### <a name="input_execution_mode"></a> [execution\_mode](#input\_execution\_mode)

Description: (Optional) Which execution mode to use. Using Terraform Cloud, valid values are `remote`, `local` or `agent`. When set to `local`, the workspace will be used for state storage only. Important: If you omit this attribute, the resource configures the workspace to use your organization's default execution mode (which in turn defaults to `remote`), removing any explicit value that might have previously been set for the workspace.

Type: `string`

Default: `null`

### <a name="input_file_triggers_enabled"></a> [file\_triggers\_enabled](#input\_file\_triggers\_enabled)

Description: (Optional) Whether to filter runs based on the changed files in a VCS push. If enabled, the working directory and trigger prefixes describe a set of paths which must contain changes for a VCS push to trigger a run. If disabled, any push will trigger a run.

Type: `bool`

Default: `false`

### <a name="input_global_remote_state"></a> [global\_remote\_state](#input\_global\_remote\_state)

Description: (Optional) Whether the workspace allows all workspaces in the organization to access its state data during runs. If false, then only specifically approved workspaces can access its state (`remote_state_consumer_ids`).

Type: `bool`

Default: `false`

### <a name="input_oauth_client_name"></a> [oauth\_client\_name](#input\_oauth\_client\_name)

Description: (Optional) Name of the OAuth client.

Type: `string`

Default: `"GitHub"`

### <a name="input_organization"></a> [organization](#input\_organization)

Description: (Optional) Name of the organization.

Type: `string`

Default: `null`

### <a name="input_queue_all_runs"></a> [queue\_all\_runs](#input\_queue\_all\_runs)

Description: (Optional) Whether the workspace should start automatically performing runs immediately after its creation. When set to `false`, runs triggered by a webhook (such as a commit in VCS) will not be queued until at least one run has been manually queued. Note: This default differs from the Terraform Cloud API default, which is `false`. The provider uses `true` as any workspace provisioned with `false` would need to then have a run manually queued out-of-band before accepting webhooks.

Type: `bool`

Default: `true`

### <a name="input_remote_state_consumer_ids"></a> [remote\_state\_consumer\_ids](#input\_remote\_state\_consumer\_ids)

Description: (Optional) The set of workspace IDs set as explicit remote state consumers for the given workspace.

Type: `set(string)`

Default: `[]`

### <a name="input_run_tasks"></a> [run\_tasks](#input\_run\_tasks)

Description:   (Optional) A list of run tasks to be executed on the workspace.  
    task\_id           : (Required) The id of the Run task to associate to the workspace.  
    enforcement\_level : (Optional) The enforcement level of the task. Valid values are `advisory` and `mandatory`.  
    stages            : (Optional) The stages to run the task in. Valid values are one or more of `pre_plan`, `post_plan`, `pre_apply` and `post_apply`.

Type:

```hcl
list(object({
    task_id           = string
    enforcement_level = optional(string, "advisory")
    stages            = optional(list(string), [])
  }))
```

Default: `[]`

### <a name="input_source_name"></a> [source\_name](#input\_source\_name)

Description: (Optional) A friendly name for the application or client creating this workspace. If set, this will be displayed on the workspace as 'Created via '. Requires `source_url` to also be set.

Type: `string`

Default: `null`

### <a name="input_source_url"></a> [source\_url](#input\_source\_url)

Description: (Optional) A URL for the application or client creating this workspace. This can be the URL of a related resource in another app, or a link to documentation or other info about the client. Requires `source_name` to also be set. Note: The API does not (currently) allow this to be updated after a workspace has been created, so modifying this value will result in the workspace being replaced. To disable this, use an ignore changes lifecycle meta-argument

Type: `string`

Default: `null`

### <a name="input_speculative_enabled"></a> [speculative\_enabled](#input\_speculative\_enabled)

Description: (Optional) Whether this workspace allows speculative plans. Setting this to `false` prevents Terraform Cloud or the Terraform Enterprise instance from running plans on pull requests, which can improve security if the VCS repository is public or includes untrusted contributors.

Type: `bool`

Default: `true`

### <a name="input_ssh_key_id"></a> [ssh\_key\_id](#input\_ssh\_key\_id)

Description: (Optional) The ID of an SSH key to assign to the workspace.

Type: `string`

Default: `null`

### <a name="input_structured_run_output_enabled"></a> [structured\_run\_output\_enabled](#input\_structured\_run\_output\_enabled)

Description: (Optional) Whether this workspace should show output from Terraform runs using the enhanced UI when available. Setting this to `false` ensures that all runs in this workspace will display their output as text logs.

Type: `bool`

Default: `false`

### <a name="input_tags"></a> [tags](#input\_tags)

Description: (Optional) A map of key value tags for this workspace.

Type: `map(string)`

Default: `null`

### <a name="input_terraform_version"></a> [terraform\_version](#input\_terraform\_version)

Description: (Optional) The version of Terraform to use for this workspace. This can be either an exact version or a version constraint (like `~> 1.0.0`); if you specify a constraint, the workspace will always use the newest release that meets that constraint.

Type: `string`

Default: `"latest"`

### <a name="input_trigger_patterns"></a> [trigger\_patterns](#input\_trigger\_patterns)

Description: (Optional) List of glob patterns that describe the files Terraform Cloud monitors for changes. Trigger patterns are always appended to the root directory of the repository. Mutually exclusive with `trigger-prefixes`.

Type: `list(string)`

Default: `null`

### <a name="input_trigger_prefixes"></a> [trigger\_prefixes](#input\_trigger\_prefixes)

Description: (Optional) List of repository-root-relative paths which describe all locations to be tracked for changes.

Type: `list(string)`

Default: `null`

### <a name="input_variable"></a> [variable](#input\_variable)

Description:   (Optional) The variable is a list of object supporting the following attributes:  
    key         : (Required) Name of the variable.  
    value       : (Required) Value of the variable.  
    category    : (Required) Whether this is a Terraform or environment variable. Valid values are `terraform` or `env`.  
    description : (Optional) Description of the variable.  
    hcl         : (Optional) Whether to evaluate the value of the variable as a string of HCL code. Has no effect for environment variables. Defaults to `false`.  
    sensitive   : (Optional) Whether the value is sensitive. If true then the variable is written once and not visible thereafter. Defaults to `false`.

Type:

```hcl
list(object({
    key         = string
    value       = string
    category    = string
    description = optional(string)
    hcl         = optional(bool, false)
    sensitive   = optional(bool, false)
  }))
```

Default: `[]`

### <a name="input_vcs_repo_branch"></a> [vcs\_repo\_branch](#input\_vcs\_repo\_branch)

Description: (Optional) The repository branch that Terraform will execute from. This defaults to the repository's default branch (e.g. main).

Type: `string`

Default: `null`

### <a name="input_vcs_repo_github_app_installation_id"></a> [vcs\_repo\_github\_app\_installation\_id](#input\_vcs\_repo\_github\_app\_installation\_id)

Description: (Optional) The installation id of the Github App. This conflicts with 'oauth\_token\_id' and can only be used if 'oauth\_token\_id' is not used.

Type: `string`

Default: `null`

### <a name="input_vcs_repo_ingress_submodules"></a> [vcs\_repo\_ingress\_submodules](#input\_vcs\_repo\_ingress\_submodules)

Description: (Optional) Whether submodules should be fetched when cloning the VCS repository.

Type: `bool`

Default: `false`

### <a name="input_vcs_repo_oauth_token_id"></a> [vcs\_repo\_oauth\_token\_id](#input\_vcs\_repo\_oauth\_token\_id)

Description: (Optional) The VCS Connection (OAuth Connection + Token) to use. This ID can be obtained from a 'tfe\_oauth\_client' resource. This conflicts with 'github\_app\_installation\_id' and can only be used if 'github\_app\_installation\_id' is not used.

Type: `string`

Default: `null`

### <a name="input_vcs_repo_tags_regex"></a> [vcs\_repo\_tags\_regex](#input\_vcs\_repo\_tags\_regex)

Description: (Optional) A regular expression used to trigger a Workspace run for matching Git tags. This option conflicts with 'trigger\_patterns' and 'trigger\_prefixes'. Should only set this value if the former is not being used.

Type: `string`

Default: `null`

### <a name="input_working_directory"></a> [working\_directory](#input\_working\_directory)

Description: (Optional) A relative path that Terraform will execute within. Defaults to the root of your repository.

Type: `string`

Default: `null`

## Resources

The following resources are used by this module:

- [tfe_variable.this](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) (resource)
- [tfe_workspace.this](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/workspace) (resource)
- [tfe_workspace_run_task.this](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/workspace_run_task) (resource)
- [tfe_workspace_settings.this](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/workspace_settings) (resource)
- [tfe_oauth_client.client](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/data-sources/oauth_client) (data source)
- [tfe_project.this](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/data-sources/project) (data source)

## Outputs

The following outputs are exported:

### <a name="output_html_url"></a> [html\_url](#output\_html\_url)

Description: The URL to the browsable HTML overview of the workspace.

### <a name="output_id"></a> [id](#output\_id)

Description: The workspace ID.

### <a name="output_resource_count"></a> [resource\_count](#output\_resource\_count)

Description: The number of resources managed by the workspace.

### <a name="output_workspace"></a> [workspace](#output\_workspace)

Description: Terraform Cloud workspace resource.

### <a name="output_workspace_run_tasks"></a> [workspace\_run\_tasks](#output\_workspace\_run\_tasks)

Description: The workspace run tasks.

### <a name="output_workspace_run_tasks_id"></a> [workspace\_run\_tasks\_id](#output\_workspace\_run\_tasks\_id)

Description: The workspace run tasks id.

<!-- markdownlint-enable -->
<!-- END_TF_DOCS -->