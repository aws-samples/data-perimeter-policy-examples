# Service-specific guidance: AWS CloudFormation


This document outlines service-specific guidance for implementing a data perimeter for AWS CloudFormation.

AWS CloudFormation is a service that helps you model and set up your AWS resources so that you can spend less time managing those resources and more time focusing on your applications that run in AWS. You create a template that describes all the AWS resources that you want (like Amazon EC2 instances or Amazon RDS DB instances), and CloudFormation takes care of provisioning and configuring those resources for you.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | Y |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## CreateChangeSet
### S3 presigned URLs created by service principals

**Perimeter type applicability**: network perimeter applied on resource.

**Description**: [CreateChangeSet](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_CreateChangeSet.html) allows you to specify a custom resource or a wait condition in your template. AWS CloudFormation delivers an Amazon S3 presigned URL that the custom resource or wait condition uses to send its response to a service-owned bucket. The presigned URL is created with the AWS CloudFormation service principal.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [CreateChangeSet](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_CreateChangeSet.html) permissions to select principals using an SCP. See [restrict_presignedURL_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateChangeSet](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_CreateChangeSet.html) API calls in your environment. If necessary, remediate with the responsive controls of your choice.

### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [CreateChangeSet](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_CreateChangeSet.html) might require access to service-owned transforms, which are transforms that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned transforms in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [cloudformation_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/cloudformation_endpoint_policy.json), which lists service-owned transforms in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## CreateStack
### S3 presigned URLs created by service principals

**Perimeter type applicability**: network perimeter applied on resource.

**Description**: [CreateStack](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_CreateStack.html) allows you to specify a custom resource or a wait condition in your template. AWS CloudFormation delivers an Amazon S3 presigned URL that the custom resource or wait condition uses to send its response to a service-owned bucket. The presigned URL is created with the AWS CloudFormation service principal.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [CreateStack](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_CreateStack.html) permissions to select principals using an SCP. See [restrict_presignedURL_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateStack](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_CreateStack.html) API calls in your environment. If necessary, remediate with the responsive controls of your choice.

## CreateStackSet
### S3 presigned URLs created by service principals

**Perimeter type applicability**: network perimeter applied on resource.

**Description**: [CreateStackSet](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_CreateStackSet.html) allows you to specify a custom resource or a wait condition in your template. AWS CloudFormation delivers an Amazon S3 presigned URL that the custom resource or wait condition uses to send its response to a service-owned bucket. The presigned URL is created with the AWS CloudFormation service principal.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [CreateStackSet](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_CreateStackSet.html) permissions to select principals using an SCP. See [restrict_presignedURL_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateStackSet](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_CreateStackSet.html) API calls in your environment. If necessary, remediate with the responsive controls of your choice.

## UpdateStack
### S3 presigned URLs created by service principals

**Perimeter type applicability**: network perimeter applied on resource.

**Description**: [UpdateStack](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_UpdateStack.html) allows you to specify a custom resource or a wait condition in your template. AWS CloudFormation delivers an Amazon S3 presigned URL that the custom resource or wait condition uses to send its response to a service-owned bucket. The presigned URL is created with the AWS CloudFormation service principal.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [UpdateStack](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_UpdateStack.html) permissions to select principals using an SCP. See [restrict_presignedURL_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateStack](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_UpdateStack.html) API calls in your environment. If necessary, remediate with the responsive controls of your choice.

## UpdateStackSet
### S3 presigned URLs created by service principals

**Perimeter type applicability**: network perimeter applied on resource.

**Description**: [UpdateStackSet](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_UpdateStackSet.html) allows you to specify a custom resource or a wait condition in your template. AWS CloudFormation delivers an Amazon S3 presigned URL that the custom resource or wait condition uses to send its response to a service-owned bucket. The presigned URL is created with the AWS CloudFormation service principal.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [UpdateStackSet](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_UpdateStackSet.html) permissions to select principals using an SCP. See [restrict_presignedURL_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateStackSet](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_UpdateStackSet.html) API calls in your environment. If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* CreateChangeSet
* CreateGeneratedTemplate
* CreateStack
* CreateStackInstances
* CreateStackRefactor
* CreateStackSet
* DeleteChangeSet
* DeleteGeneratedTemplate
* DeleteStack
* DeleteStackInstances
* DeleteStackSet
* DescribeAccountLimits
* DescribeEvents
* DescribeGeneratedTemplate
* DescribeResourceScan
* DescribeStackDriftDetectionStatus
* DescribeStackEvents
* DescribeStackInstance
* DescribeStackRefactor
* DescribeStackResource
* DescribeStackResourceDrifts
* DescribeStackResources
* DescribeStackSet
* DescribeStackSetOperation
* DescribeStacks
* DescribeTypeRegistration
* DetectStackDrift
* DetectStackResourceDrift
* DetectStackSetDrift
* EstimateTemplateCost
* GetHookResult
* GetStackPolicy
* GetTemplate
* GetTemplateSummary
* ListChangeSets
* ListExports
* ListGeneratedTemplates
* ListHookResults
* ListResourceScanRelatedResources
* ListResourceScanResources
* ListResourceScans
* ListStackInstances
* ListStackRefactorActions
* ListStackRefactors
* ListStackResources
* ListStackSetOperationResults
* ListStackSetOperations
* ListStackSets
* ListStacks
* ListTypeRegistrations
* ListTypes
* RegisterType
* SetStackPolicy
* StartResourceScan
* UpdateGeneratedTemplate
* UpdateStack
* UpdateStackInstances
* UpdateStackSet
* UpdateTerminationProtection
* ValidateTemplate
