
# Service-specific guidance: Amazon EMR Serverless


This document outlines service-specific guidance for implementing a data perimeter for Amazon EMR Serverless. 

Amazon EMR Serverless is a managed service that allows you to run big data analytics applications without the need to configure, manage, or scale clusters and servers. It provides a serverless runtime environment for Apache Spark and Apache Hive, enabling you to process data quickly and cost-effectively while automatically scaling resources based on workload demands.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | Y |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | Y |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | Y |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | Y |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | Y |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | Y |

*Y – Additional considerations apply. N – No additional considerations apply.
 



**Additional consideration 1**

Perimeter type applicability: all.
        
UpdateApplication allows you to associate applications with an Amazon VPC to run your code.

If you want to achieve data perimeter control objectives, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help ensure that developers specify the [NetworkConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-emrserverless-application.html#cfn-emrserverless-application-networkconfiguration) property of the [AWS::EMRServerless::Application](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-emrserverless-application.html) resource.
* **Detective control example 1:** Consider implementing a custom AWS Config rule to help detect EMR Serverless applications not associated with a VPC, or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* **Detective control example 2:** Consider using CloudTrail management events to monitor the [UpdateApplication](https://docs.aws.amazon.com/emr-serverless/latest/APIReference/API_UpdateApplication.html) API calls in your environment (specifically, the [networkConfiguration](https://docs.aws.amazon.com/emr-serverless/latest/APIReference/API_UpdateApplication.html#emrserverless-UpdateApplication-request-networkConfiguration) request parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 2**

Perimeter type applicability: all.
        
CreateApplication allows you to associate applications with an Amazon VPC to run your code.

If you want to achieve data perimeter control objectives, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help ensure that developers specify the [NetworkConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-emrserverless-application.html#cfn-emrserverless-application-networkconfiguration) property of the [AWS::EMRServerless::Application](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-emrserverless-application.html) resource.
* **Detective control example 1:** Consider implementing a custom AWS Config rule to help detect EMR Serverless applications not associated with a VPC, or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* **Detective control example 2:** Consider using CloudTrail management events to monitor the [CreateApplication](https://docs.aws.amazon.com/emr-serverless/latest/APIReference/API_CreateApplication.html) API calls in your environment (specifically, the [networkConfiguration](https://docs.aws.amazon.com/emr-serverless/latest/APIReference/API_CreateApplication.html#emrserverless-CreateApplication-request-networkConfiguration) request parameter). If necessary, remediate with the responsive controls of your choice.





**List of service APIs reviewed against data perimeter control objectives**

* CancelJobRun
* CreateApplication
* DeleteApplication
* GetApplication
* GetJobRun
* ListApplications
* ListJobRunAttempts
* ListJobRuns
* ListTagsForResource
* StartApplication
* StartJobRun
* StopApplication
* TagResource
* UntagResource
* UpdateApplication
