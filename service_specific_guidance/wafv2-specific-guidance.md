# Service-specific guidance: AWS WAF V2


This document outlines service-specific guidance for implementing a data perimeter for AWS WAF V2.

AWS WAF V2 is the latest version of the AWS WAF API: a web application firewall that lets you monitor the HTTP and HTTPS requests forwarded to protected resources such as Amazon CloudFront distributions, Amazon API Gateway REST APIs, Application Load Balancers, AWS AppSync GraphQL APIs, Amazon Cognito user pools, AWS App Runner services, Amazon Bedrock AgentCore Gateways, AWS Verified Access instances, and AWS Amplify applications. Based on conditions you specify, such as the IP addresses that requests originate from or the values of query strings, AWS WAF controls access to your content so the protected resource responds with the requested content, an HTTP 403 (Forbidden) status code, or a custom response.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | N |

*Y - Additional considerations apply. N - No additional considerations apply.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* AssociateWebACL
* CheckCapacity
* CreateAPIKey
* CreateIPSet
* CreateRegexPatternSet
* CreateRuleGroup
* CreateWebACL
* DeleteAPIKey
* DeleteFirewallManagerRuleGroups
* DeleteIPSet
* DeleteLoggingConfiguration
* DeletePermissionPolicy
* DeleteRegexPatternSet
* DeleteRuleGroup
* DeleteWebACL
* DescribeAllManagedProducts
* DescribeManagedProductsByVendor
* DescribeManagedRuleGroup
* DisassociateWebACL
* GetDecryptedAPIKey
* GetIPSet
* GetLoggingConfiguration
* GetPermissionPolicy
* GetRegexPatternSet
* GetRuleGroup
* GetSampledRequests
* GetTopPathStatisticsByTraffic
* GetWebACL
* GetWebACLForResource
* ListAPIKeys
* ListAvailableManagedRuleGroupVersions
* ListAvailableManagedRuleGroups
* ListIPSets
* ListLoggingConfigurations
* ListManagedRuleSets
* ListRegexPatternSets
* ListResourcesForWebACL
* ListRuleGroups
* ListTagsForResource
* ListWebACLs
* PutLoggingConfiguration
* PutPermissionPolicy
* TagResource
* UntagResource
* UpdateIPSet
* UpdateRegexPatternSet
* UpdateRuleGroup
* UpdateWebACL
