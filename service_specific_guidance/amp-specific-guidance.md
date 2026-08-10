# Service-specific guidance: Amazon Managed Service for Prometheus


This document outlines service-specific guidance for implementing a data perimeter for Amazon Managed Service for Prometheus.

Amazon Managed Service for Prometheus is a serverless, Prometheus-compatible monitoring service for container metrics that makes it easier to securely monitor container environments at scale. It automatically scales the ingestion, storage, and querying of operational metrics as workloads scale up and down, and integrates with AWS security services to enable fast and secure access to data.

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

## PutResourcePolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutResourcePolicy](https://docs.aws.amazon.com/prometheus/latest/APIReference/API_PutResourcePolicy.html) allows you to apply a resource-based policy to grant access to a workspace. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutResourcePolicy](https://docs.aws.amazon.com/prometheus/latest/APIReference/API_PutResourcePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [PolicyDocument](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-aps-resourcepolicy.html#cfn-aps-resourcepolicy-policydocument) property for the [AWS::APS::ResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-aps-resourcepolicy.html) resource that grants permissions to untrusted identities.
* Detective control example: Consider using CloudTrail management events to monitor the [PutResourcePolicy](https://docs.aws.amazon.com/prometheus/latest/APIReference/API_PutResourcePolicy.html) API calls in your environment (specifically, the [policyDocument](https://docs.aws.amazon.com/prometheus/latest/APIReference/API_PutResourcePolicy.html#API_PutResourcePolicy_RequestSyntax) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutResourcePolicy](https://docs.aws.amazon.com/prometheus/latest/APIReference/API_PutResourcePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [PolicyDocument](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-aps-resourcepolicy.html#cfn-aps-resourcepolicy-policydocument) property for the [AWS::APS::ResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-aps-resourcepolicy.html) resource that grants permissions to unexpected networks.
* Detective control example: Consider using CloudTrail management events to monitor the [PutResourcePolicy](https://docs.aws.amazon.com/prometheus/latest/APIReference/API_PutResourcePolicy.html) API calls in your environment (specifically, the [policyDocument](https://docs.aws.amazon.com/prometheus/latest/APIReference/API_PutResourcePolicy.html#API_PutResourcePolicy_RequestSyntax) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

* CreateLoggingConfiguration
* CreateQueryLoggingConfiguration
* CreateRuleGroupsNamespace
* CreateScraper
* CreateWorkspace
* DeleteLoggingConfiguration
* DeleteQueryLoggingConfiguration
* DeleteResourcePolicy
* DeleteRuleGroupsNamespace
* DeleteScraper
* DeleteScraperLoggingConfiguration
* DeleteWorkspace
* DescribeLoggingConfiguration
* DescribeQueryLoggingConfiguration
* DescribeResourcePolicy
* DescribeRuleGroupsNamespace
* DescribeScraper
* DescribeScraperLoggingConfiguration
* DescribeWorkspace
* DescribeWorkspaceConfiguration
* GetDefaultScraperConfiguration
* ListRuleGroupsNamespaces
* ListScrapers
* ListTagsForResource
* ListWorkspaces
* PutResourcePolicy
* PutRuleGroupsNamespace
* TagResource
* UntagResource
* UpdateLoggingConfiguration
* UpdateQueryLoggingConfiguration
* UpdateScraper
* UpdateScraperLoggingConfiguration
* UpdateWorkspaceAlias
* UpdateWorkspaceConfiguration
