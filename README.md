# AWS Network Firewall Module
- This module for creating AWS Network Firewall by Terraform
- Source: https://github.com/mattyait/terraform-aws-network-firewall

## Usage Sample:
- Configuration [link](./e2e/main.tf) 
```hcl
module "nfw" {
  source = "../"

  for_each = var.nfw

  nfw_name        = each.value.nfw_name
  vpc_id          = each.value.vpc_id
  subnet_mapping  = each.value.subnet_mapping
  logging_config  = try(each.value.logging_config, {})

  prefix                        = local.app_env_prefix
  # Five Tuple Firewall Rule Group
  fivetuple_stateful_rule_group = try(concat(each.value.fivetuple_stateful_rule_group, var.fivetuple_stateful_rule_group), [])
  
  # Stateless Rule Group
  stateless_rule_group          = try(concat(each.value.stateless_rule_group, var.stateless_rule_group), [])
  
  #Suricate Firewall Rule Group
  suricata_stateful_rule_group  = try(concat(each.value.suricata_stateful_rule_group, var.suricata_stateful_rule_group), [])
  
  #Domain Firewall Rule Group
  domain_stateful_rule_group    = try(concat(each.value.domain_stateful_rule_group, var.domain_stateful_rule_group), [])
  tags = {
    "end_to_end" = "true"
  }
}
locals {
  app_env_prefix = "${lookup(var.default_tags, "component", "-")}-${lookup(var.default_tags, "env", "-")}"
}
```


<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_aws"></a> [aws](#requirement\_aws) | ~> 4.31.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_aws"></a> [aws](#provider\_aws) | ~> 4.31.0 |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [aws_cloudwatch_log_group.nfw](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_log_group) | resource |
| [aws_networkfirewall_firewall.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/networkfirewall_firewall) | resource |
| [aws_networkfirewall_firewall_policy.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/networkfirewall_firewall_policy) | resource |
| [aws_networkfirewall_logging_configuration.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/networkfirewall_logging_configuration) | resource |
| [aws_networkfirewall_rule_group.domain_stateful_group](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/networkfirewall_rule_group) | resource |
| [aws_networkfirewall_rule_group.fivetuple_stateful_group](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/networkfirewall_rule_group) | resource |
| [aws_networkfirewall_rule_group.stateless_group](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/networkfirewall_rule_group) | resource |
| [aws_networkfirewall_rule_group.suricata_stateful_group](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/networkfirewall_rule_group) | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_description"></a> [description](#input\_description) | n/a | `string` | `""` | no |
| <a name="input_domain_stateful_rule_group"></a> [domain\_stateful\_rule\_group](#input\_domain\_stateful\_rule\_group) | Config for domain type stateful rule group | `list(any)` | `[]` | no |
| <a name="input_firewall_policy_change_protection"></a> [firewall\_policy\_change\_protection](#input\_firewall\_policy\_change\_protection) | (optional) we set false because we apply gitops for this | `string` | `false` | no |
| <a name="input_fivetuple_stateful_rule_group"></a> [fivetuple\_stateful\_rule\_group](#input\_fivetuple\_stateful\_rule\_group) | Config for 5-tuple type stateful rule group | `list(any)` | `[]` | no |
| <a name="input_logging_config"></a> [logging\_config](#input\_logging\_config) | (optional) Logging config for network firewall | `map(any)` | `{}` | no |
| <a name="input_nfw_name"></a> [nfw\_name](#input\_nfw\_name) | firewall name | `string` | `"example"` | no |
| <a name="input_prefix"></a> [prefix](#input\_prefix) | The descriptio for each environment, ie: bin-dev | `string` | n/a | yes |
| <a name="input_stateless_default_actions"></a> [stateless\_default\_actions](#input\_stateless\_default\_actions) | Default stateless Action | `string` | `"forward_to_sfe"` | no |
| <a name="input_stateless_fragment_default_actions"></a> [stateless\_fragment\_default\_actions](#input\_stateless\_fragment\_default\_actions) | Default Stateless action for fragmented packets | `string` | `"forward_to_sfe"` | no |
| <a name="input_stateless_rule_group"></a> [stateless\_rule\_group](#input\_stateless\_rule\_group) | Config for stateless rule group | `list(any)` | n/a | yes |
| <a name="input_subnet_change_protection"></a> [subnet\_change\_protection](#input\_subnet\_change\_protection) | (optional) we set false because we apply gitops for this | `string` | `false` | no |
| <a name="input_subnet_mapping"></a> [subnet\_mapping](#input\_subnet\_mapping) | Subnet ids mapping to have individual firewall endpoint | `any` | n/a | yes |
| <a name="input_suricata_stateful_rule_group"></a> [suricata\_stateful\_rule\_group](#input\_suricata\_stateful\_rule\_group) | Config for Suricata type stateful rule group | `list(any)` | `[]` | no |
| <a name="input_tags"></a> [tags](#input\_tags) | The tags for the resources | `map(any)` | `{}` | no |
| <a name="input_vpc_id"></a> [vpc\_id](#input\_vpc\_id) | VPC ID | `string` | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_arn"></a> [arn](#output\_arn) | Created Network Firewall ARN from network\_firewall module |
| <a name="output_endpoint_id"></a> [endpoint\_id](#output\_endpoint\_id) | Created Network Firewall endpoint id |
| <a name="output_id"></a> [id](#output\_id) | Created Network Firewall ID from network\_firewall module |
<!-- END_TF_DOCS -->