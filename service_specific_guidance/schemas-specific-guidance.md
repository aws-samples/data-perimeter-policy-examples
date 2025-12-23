# Service-specific guidance: Amazon EventBridge schemas


This document outlines service-specific guidance for implementing a data perimeter for Amazon EventBridge schemas. 


Amazon EventBridge Schemas is a service that helps developers discover, create, and manage schemas for events in EventBridge. It automatically infers schemas from events passing through EventBridge, making it easier to build applications that react to these events. The service also provides a registry for storing and sharing schemas, enabling developers to maintain consistency across their event-driven applications.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | Y |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | N |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | Y |

*Y – Additional considerations apply. N – No additional considerations apply.
 
**Additional consideration 1**

Perimeter type applicability: identity and network perimeter applied on resource.
        
PutResourcePolicy allows you to apply a resource-based policy to grant access to a registry. The service currently does not support RCPs.

If you want to restrict access to trusted identities and expected networks, consider implementing these additional controls:

* **Preventative control example**: Consider restricting [PutResourcePolicy](https://docs.aws.amazon.com/eventbridge/latest/schema-reference/v1-policy.html#PutResourcePolicy) permissions to administrators only using an SCP. See [restrict_resource_policy_configurations_scp.json](../service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying [Policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-eventschemas-registrypolicy.html#cfn-eventschemas-registrypolicy-policy) properties for the [AWS::EventSchemas::RegistryPolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-eventschemas-registrypolicy.html) resource that grants permissions to untrusted identities or unexpected networks. 
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutResourcePolicy](https://docs.aws.amazon.com/eventbridge/latest/schema-reference/v1-policy.html#PutResourcePolicy) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/eventbridge/latest/schema-reference/v1-policy.html#v1-policy-prop-putresourcepolicyinput-policy) request parameter). If necessary, remediate with the responsive controls of your choice. 


**List of service APIs reviewed against data perimeter control objectives**
* CreateDiscoverer
* CreateRegistry
* CreateSchema
* DeleteDiscoverer
* DeleteRegistry
* DeleteResourcePolicy
* DeleteSchema
* DescribeCodeBinding
* DescribeDiscoverer
* DescribeRegistry
* DescribeSchema
* ExportSchema
* GetCodeBindingSource
* GetDiscoveredSchema
* GetResourcePolicy
* ListDiscoverers
* ListRegistries
* ListSchemaVersions
* ListSchemas
* ListTagsForResource
* PutCodeBinding
* PutResourcePolicy
* SearchSchemas
* StartDiscoverer
* StopDiscoverer
* TagResource
* UntagResource
* UpdateDiscoverer
* UpdateRegistry
* UpdateSchema
