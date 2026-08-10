# Service-specific guidance: AWS Serverless Application Repository


This document outlines service-specific guidance for implementing a data perimeter for AWS Serverless Application Repository.

AWS Serverless Application Repository is a managed repository for serverless applications that makes it easy for developers and enterprises to quickly find, deploy, and publish serverless applications packaged as AWS SAM templates in the AWS Cloud.

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

## CreateCloudFormationTemplate
### S3 presigned URLs created by service principals

**Perimeter type applicability**: network perimeter on resource.

**Description**: [CreateCloudFormationTemplate](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid-templates.html#applications-applicationid-templatespost) returns an Amazon S3 presigned URL that users can use to download a CloudFormation template from a service-owned bucket. The presigned URL is created with the AWS Serverless Application Repository service principal.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [CreateCloudFormationTemplate](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid-templates.html#applications-applicationid-templatespost) permissions to select principals using an SCP. See [restrict_presignedURL_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateCloudFormationTemplate](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid-templates.html#applications-applicationid-templatespost) API calls in your environment. If necessary, remediate with the responsive controls of your choice.

## GetApplication
### S3 presigned URLs created by service principals

**Perimeter type applicability**: network perimeter on resource.

**Description**: [GetApplication](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid.html#applications-applicationidget) returns Amazon S3 presigned URLs that users can use to download an application's readme and license files from a service-owned bucket. The presigned URL is created with the AWS Serverless Application Repository service principal.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [GetApplication](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid.html#applications-applicationidget) permissions to select principals using an SCP. See [restrict_presignedURL_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [GetApplication](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid.html#applications-applicationidget) API calls in your environment. If necessary, remediate with the responsive controls of your choice.

## GetCloudFormationTemplate
### S3 presigned URLs created by service principals

**Perimeter type applicability**: network perimeter on resource.

**Description**: [GetCloudFormationTemplate](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid-templates-templateid.html#applications-applicationid-templates-templateidget) returns an Amazon S3 presigned URL that users can use to download a CloudFormation template from a service-owned bucket. The presigned URL is created with the AWS Serverless Application Repository service principal.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [GetCloudFormationTemplate](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid-templates-templateid.html#applications-applicationid-templates-templateidget) permissions to select principals using an SCP. See [restrict_presignedURL_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [GetCloudFormationTemplate](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid-templates-templateid.html#applications-applicationid-templates-templateidget) API calls in your environment. If necessary, remediate with the responsive controls of your choice.

## PutApplicationPolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutApplicationPolicy](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid-policy.html) allows you to apply a resource-based policy to grant access to an application. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutApplicationPolicy](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid-policy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutApplicationPolicy](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid-policy.html) API calls in your environment (specifically, the [principals](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid-policy.html#applications-applicationid-policy-prop-applicationpolicystatement-principals) and [principalOrgIDs](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid-policy.html#applications-applicationid-policy-prop-applicationpolicystatement-principalorgids) request parameters). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing this additional control:
* Preventative control example: Consider restricting [PutApplicationPolicy](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid-policy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.

## List of service APIs reviewed against data perimeter control objectives

* CreateApplication
* CreateApplicationVersion
* CreateCloudFormationChangeSet
* CreateCloudFormationTemplate
* DeleteApplication
* GetApplication
* GetApplicationPolicy
* GetCloudFormationTemplate
* ListApplicationDependencies
* ListApplicationVersions
* ListApplications
* PutApplicationPolicy
* UpdateApplication
