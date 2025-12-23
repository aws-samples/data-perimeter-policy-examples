# Service-specific guidance: AWS Serverless Application Repository


This document outlines service-specific guidance for implementing a data perimeter for AWS Serverless Application Repository. 


AWS Serverless Application Repository is a managed service that enables developers to easily discover, deploy, and publish serverless applications. It provides a centralized repository for sharing reusable serverless application components, allowing developers to quickly build and deploy applications using pre-built components from the community or their own organization. The service simplifies the process of finding, configuring, and deploying serverless architectures, accelerating development and promoting best practices.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | Y |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | N |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | Y |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | Y |

*Y – Additional considerations apply. N – No additional considerations apply.
 



**Additional consideration 1**

Perimeter type applicability: network perimeter.
        
CreateCloudFormationTemplate returns an Amazon S3 presigned URL that users can use to download a CloudFormation template from a service-owned bucket. The presigned URL is created with an AWS Serverless Application Repository service principal.

If you want to restrict access to expected networks, consider implementing these additional controls:

* **Preventative control example:** Consider restricting [CreateCloudFormationTemplate](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid-templates.html#applications-applicationid-templatespost) permissions to administrators or specific applications only using an SCP. See [restrict_presignedURL_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.

* **Detective control example:** Consider using CloudTrail management events to monitor the [CreateCloudFormationTemplate](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid-templates.html#applications-applicationid-templatespost) API calls in your environment. If necessary, remediate with the responsive controls of your choice.


**Additional consideration 2**

Perimeter type applicability: network perimeter.
        
GetApplication returns Amazon S3 presigned URLs that users can use to download an application's readme and license files from a service-owned bucket. The presigned URL is created with an AWS Serverless Application Repository service principal.

If you want to restrict access to expected networks, consider implementing these additional controls:

* **Preventative control example:** Consider restricting [GetApplication](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid.html#applications-applicationidget) permissions to administrators or specific applications only using an SCP. See [restrict_presignedURL_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.

* **Detective control example:** Consider using CloudTrail management events to monitor the [GetApplication](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid.html#applications-applicationidget) API calls in your environment. If necessary, remediate with the responsive controls of your choice.


**Additional consideration 3**

Perimeter type applicability: network perimeter.
        
GetCloudFormationTemplate returns an Amazon S3 presigned URL that users can use to download a CloudFormation template from a service-owned bucket. The presigned URL is created with an AWS Serverless Application Repository service principal.

If you want to restrict access to expected networks, consider implementing these additional controls:

* **Preventative control example:** Consider restricting [GetCloudFormationTemplate](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid-templates-templateid.html#applications-applicationid-templates-templateidget) permissions to administrators or specific applications only using an SCP. See [restrict_presignedURL_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.

* **Detective control example:** Consider using CloudTrail management events to monitor the [GetCloudFormationTemplate](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid-templates-templateid.html#applications-applicationid-templates-templateidget) API calls in your environment. If necessary, remediate with the responsive controls of your choice.


**Additional consideration 4**

Perimeter type applicability: identity and network perimeter applied on resource.
        
PutApplicationPolicy allows you to apply a resource-based policy to grant access to an application. The service currently does not support RCPs.

If you want to restrict access to trusted identities and expected networks, consider implementing these additional controls:

* **Preventative control example**: Consider restricting [PutApplicationPolicy](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid-policy.html) permissions to administrators only using an SCP. See [restrict_resource_policy_configurations_scp.json](../service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutApplicationPolicy](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid-policy.html) API calls in your environment (specifically, the [principals](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid-policy.html#applications-applicationid-policy-prop-applicationpolicystatement-principals) and [principalOrgIDs](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/applications-applicationid-policy.html#applications-applicationid-policy-prop-applicationpolicystatement-principalorgids) request parameters). If necessary, remediate with the responsive controls of your choice. 




**List of service APIs reviewed against data perimeter control objectives**
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
