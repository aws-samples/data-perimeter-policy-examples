# Service-specific guidance: Amazon Route 53 Resolver


This document outlines service-specific guidance for implementing a data perimeter for Amazon Route 53 Resolver.

Amazon Route 53 Resolver (now documented as Route 53 VPC Resolver) responds recursively to DNS queries from AWS resources for public records, Amazon VPC-specific DNS names, and Route 53 private hosted zones, and is available by default in all VPCs. It also provides inbound and outbound Resolver endpoints and forwarding rules for hybrid DNS resolution between VPCs and on-premises networks, Resolver DNS Firewall to filter outbound DNS traffic from VPCs, and query logging for DNS queries that originate in VPCs.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | Y |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## PutFirewallRuleGroupPolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutFirewallRuleGroupPolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutFirewallRuleGroupPolicy.html) allows you to apply a resource-based policy to grant access to a DNS Firewall rule group. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutFirewallRuleGroupPolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutFirewallRuleGroupPolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutFirewallRuleGroupPolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutFirewallRuleGroupPolicy.html) API calls in your environment (specifically, the [FirewallRuleGroupPolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutFirewallRuleGroupPolicy.html#Route53Resolver-route53resolver_PutFirewallRuleGroupPolicy-request-FirewallRuleGroupPolicy) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutFirewallRuleGroupPolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutFirewallRuleGroupPolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutFirewallRuleGroupPolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutFirewallRuleGroupPolicy.html) API calls in your environment (specifically, the [FirewallRuleGroupPolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutFirewallRuleGroupPolicy.html#Route53Resolver-route53resolver_PutFirewallRuleGroupPolicy-request-FirewallRuleGroupPolicy) request parameter). If necessary, remediate with the responsive controls of your choice.

## PutResolverQueryLogConfigPolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutResolverQueryLogConfigPolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutResolverQueryLogConfigPolicy.html) allows you to apply a resource-based policy to grant access to a query logging configuration. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutResolverQueryLogConfigPolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutResolverQueryLogConfigPolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutResolverQueryLogConfigPolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutResolverQueryLogConfigPolicy.html) API calls in your environment (specifically, the [Arn](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutResolverQueryLogConfigPolicy.html#Route53Resolver-route53resolver_PutResolverQueryLogConfigPolicy-request-Arn) and [ResolverQueryLogConfigPolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutResolverQueryLogConfigPolicy.html#Route53Resolver-route53resolver_PutResolverQueryLogConfigPolicy-request-ResolverQueryLogConfigPolicy) request parameters). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutResolverQueryLogConfigPolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutResolverQueryLogConfigPolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutResolverQueryLogConfigPolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutResolverQueryLogConfigPolicy.html) API calls in your environment (specifically, the [ResolverQueryLogConfigPolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutResolverQueryLogConfigPolicy.html#Route53Resolver-route53resolver_PutResolverQueryLogConfigPolicy-request-ResolverQueryLogConfigPolicy) request parameter). If necessary, remediate with the responsive controls of your choice.

## PutResolverRulePolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutResolverRulePolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutResolverRulePolicy.html) allows you to apply a resource-based policy to grant access to a Resolver rule. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutResolverRulePolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutResolverRulePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutResolverRulePolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutResolverRulePolicy.html) API calls in your environment (specifically, the [ResolverRulePolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutResolverRulePolicy.html#Route53Resolver-route53resolver_PutResolverRulePolicy-request-ResolverRulePolicy) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutResolverRulePolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutResolverRulePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutResolverRulePolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutResolverRulePolicy.html) API calls in your environment (specifically, the [ResolverRulePolicy](https://docs.aws.amazon.com/Route53/latest/APIReference/API_route53resolver_PutResolverRulePolicy.html#Route53Resolver-route53resolver_PutResolverRulePolicy-request-ResolverRulePolicy) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* AssociateFirewallRuleGroup
* AssociateResolverEndpointIpAddress
* AssociateResolverQueryLogConfig
* AssociateResolverRule
* BatchCreateFirewallRule
* BatchDeleteFirewallRule
* BatchUpdateFirewallRule
* CreateFirewallDomainList
* CreateFirewallRule
* CreateFirewallRuleGroup
* CreateResolverEndpoint
* CreateResolverQueryLogConfig
* CreateResolverRule
* DeleteFirewallDomainList
* DeleteFirewallRule
* DeleteFirewallRuleGroup
* DeleteOutpostResolver
* DeleteResolverEndpoint
* DeleteResolverQueryLogConfig
* DeleteResolverRule
* DisassociateFirewallRuleGroup
* DisassociateResolverEndpointIpAddress
* DisassociateResolverQueryLogConfig
* DisassociateResolverRule
* GetFirewallConfig
* GetFirewallDomainList
* GetFirewallRuleGroup
* GetFirewallRuleGroupAssociation
* GetFirewallRuleGroupPolicy
* GetOutpostResolver
* GetResolverConfig
* GetResolverDnssecConfig
* GetResolverEndpoint
* GetResolverQueryLogConfig
* GetResolverQueryLogConfigAssociation
* GetResolverQueryLogConfigPolicy
* GetResolverRule
* GetResolverRuleAssociation
* GetResolverRulePolicy
* ImportFirewallDomains
* ListFirewallConfigs
* ListFirewallDomainLists
* ListFirewallDomains
* ListFirewallRuleGroupAssociations
* ListFirewallRuleGroups
* ListFirewallRuleTypes
* ListFirewallRules
* ListOutpostResolvers
* ListResolverConfigs
* ListResolverDnssecConfigs
* ListResolverEndpointIpAddresses
* ListResolverEndpoints
* ListResolverQueryLogConfigAssociations
* ListResolverQueryLogConfigs
* ListResolverRuleAssociations
* ListResolverRules
* ListTagsForResource
* PutFirewallRuleGroupPolicy
* PutResolverQueryLogConfigPolicy
* PutResolverRulePolicy
* TagResource
* UntagResource
* UpdateFirewallConfig
* UpdateFirewallDomains
* UpdateFirewallRule
* UpdateFirewallRuleGroupAssociation
* UpdateResolverConfig
* UpdateResolverDnssecConfig
* UpdateResolverEndpoint
* UpdateResolverRule
