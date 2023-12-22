This module helps you to deploy a Helm Release. See below an example of deployment.


```
module jenkins {
  source  = "git@github.com:integromat/make-infra.git//modules/helm_release"

  namespace  = "app-namespace"
  repository =  "https://charts.helm.sh/stable"

  app = {
    name          = "jenkins"
    version       = "1.5.0"
    chart         = "jenkins"
    force_update  = true
    wait          = false
    recreate_pods = false
    deploy        = 1
  }
  values = [templatefile("jenkins.yml", {
    region                = var.region
    storage               = "4Gi"
  })]

  set = [
    {
      name  = "labels.kubernetes\\.io/name"
      value = "jenkins"
    },
    {
      name  = "service.labels.kubernetes\\.io/name"
      value = "jenkins"
    },
  ]

  set_sensitive = [
    {
      path  = "master.adminUser"
      value = "jenkins"
    },
  ]
}

```

Use [main.tf](https://github.com/terraform-module/terraform-helm-release/blob/master/main.tf) file for reference with all parameters available and use [official Helm Release resource documentation](https://registry.terraform.io/providers/hashicorp/helm/latest/docs/resources/release#argument-reference) for argument reference.

<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 0.13 |
| <a name="requirement_helm"></a> [helm](#requirement\_helm) | >= 2.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_helm"></a> [helm](#provider\_helm) | >= 2.0 |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [helm_release.this](https://registry.terraform.io/providers/hashicorp/helm/latest/docs/resources/release) | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_app"></a> [app](#input\_app) | an application to deploy | `map(any)` | n/a | yes |
| <a name="input_namespace"></a> [namespace](#input\_namespace) | namespace where to deploy an application | `string` | n/a | yes |
| <a name="input_repository"></a> [repository](#input\_repository) | Helm repository | `string` | n/a | yes |
| <a name="input_repository_config"></a> [repository\_config](#input\_repository\_config) | repository configuration | `map(any)` | `{}` | no |
| <a name="input_set"></a> [set](#input\_set) | Value block with custom STRING values to be merged with the values yaml. | <pre>list(object({<br>    name  = string<br>    value = string<br>  }))</pre> | `null` | no |
| <a name="input_set_sensitive"></a> [set\_sensitive](#input\_set\_sensitive) | Value block with custom sensitive values to be merged with the values yaml that won't be exposed in the plan's diff. | <pre>list(object({<br>    path  = string<br>    value = string<br>  }))</pre> | `null` | no |
| <a name="input_values"></a> [values](#input\_values) | Extra values | `list(string)` | `[]` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_deployment"></a> [deployment](#output\_deployment) | The state of the helm deployment |
<!-- END_TF_DOCS -->