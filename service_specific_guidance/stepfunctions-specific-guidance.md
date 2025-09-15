# Service-specific guidance: AWS Step Functions


This document outlines service-specific guidance for implementing a data perimeter for AWS Step Functions. 


AWS Step Functions is a serverless workflow orchestration service that allows you to coordinate multiple AWS services into streamlined workflows. It enables you to design and run workflows that stitch together services such as AWS Lambda, Amazon ECS, and Amazon SageMaker for applications that require complex sequences of steps. Step Functions provides a visual interface to design and monitor workflow execution, making it easier to build and manage distributed applications.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | N |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | Y |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.
 

**Additional consideration 1**

Perimeter type applicability: resource perimeter applied on identity.
        
CreateStateMachine allows you to specify an HTTPS endpoint as the ApiEndpoint value in the definition request parameter. Because the subsequent requests against the endpoints are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Preventative control example:** Consider implementing [states:HTTPEndpoint](https://docs.aws.amazon.com/step-functions/latest/dg/call-https-apis.html) in an SCP to help prevent requests to untrusted HTTPS endpoints. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.
* **Detective control example:** Consider using CloudTrail management events to monitor the [CreateStateMachine](https://docs.aws.amazon.com/step-functions/latest/apireference/API_CreateStateMachine.html#API_CreateStateMachine_RequestSyntax) API calls in your environment. If necessary, remediate with the responsive controls of your choice.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Definition](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-stepfunctions-statemachine.html#cfn-stepfunctions-statemachine-definition) property with the ApiEndpoint that does not belong to your organization for the [AWS::StepFunctions::StateMachine](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-stepfunctions-statemachine.html) resource.


**Additional consideration 2**

Perimeter type applicability: resource perimeter applied on identity.
        
UpdateStateMachine allows you to specify an HTTPS endpoint as the ApiEndpoint value in the definition request parameter. Because the subsequent requests against the endpoints are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Preventative control example:** Consider implementing [states:HTTPEndpoint](https://docs.aws.amazon.com/step-functions/latest/dg/call-https-apis.html) in an SCP to help prevent requests to untrusted HTTPS endpoints. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.
* **Detective control example:** Consider using CloudTrail management events to monitor the [UpdateStateMachine](https://docs.aws.amazon.com/step-functions/latest/apireference/API_UpdateStateMachine.html) API calls in your environment. If necessary, remediate with the responsive controls of your choice.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Definition](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-stepfunctions-statemachine.html#cfn-stepfunctions-statemachine-definition) property with the ApiEndpoint that does not belong to your organization for the [AWS::StepFunctions::StateMachine](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-stepfunctions-statemachine.html) resource.


**List of service APIs reviewed against data perimeter control objectives**
* CreateActivity
* CreateStateMachine
* CreateStateMachineAlias
* DeleteActivity
* DeleteStateMachine
* DeleteStateMachineAlias
* DeleteStateMachineVersion
* DescribeActivity
* DescribeExecution
* DescribeMapRun
* DescribeStateMachine
* DescribeStateMachineAlias
* DescribeStateMachineForExecution
* GetExecutionHistory
* ListActivities
* ListExecutions
* ListMapRuns
* ListStateMachineAliases
* ListStateMachines
* ListStateMachineVersions
* ListTagsForResource
* PublishStateMachineVersion
* RedriveExecution
* StartExecution
* StartSyncExecution
* StopExecution
* TagResource
* TestState
* UntagResource
* UpdateStateMachine
* UpdateStateMachineAlias
* ValidateStateMachineDefinition
