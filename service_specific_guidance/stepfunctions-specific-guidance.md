# Service-specific guidance: AWS Step Functions


This document outlines service-specific guidance for implementing a data perimeter for AWS Step Functions.

AWS Step Functions is a serverless workflow orchestration service that lets you create workflows (also called state machines) to build distributed applications, automate processes, orchestrate microservices, and create data and machine learning pipelines.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | N |

*Y - Additional considerations apply. N - No additional considerations apply.

## CreateStateMachine
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateStateMachine](https://docs.aws.amazon.com/step-functions/latest/apireference/API_CreateStateMachine.html) allows you to specify an HTTPS endpoint as the ApiEndpoint value in the `definition` request parameter. Because the subsequent requests against the HTTPS endpoint are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing `states:HTTPEndpoint` in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Definition](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-stepfunctions-statemachine.html#cfn-stepfunctions-statemachine-definition) property that does not belong to your organization for the [AWS::StepFunctions::StateMachine](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-stepfunctions-statemachine.html) resource.

## UpdateStateMachine
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateStateMachine](https://docs.aws.amazon.com/step-functions/latest/apireference/API_UpdateStateMachine.html) allows you to specify an HTTPS endpoint as the ApiEndpoint value in the `definition` request parameter. Because the subsequent requests against the HTTPS endpoint are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing `states:HTTPEndpoint` in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Definition](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-stepfunctions-statemachine.html#cfn-stepfunctions-statemachine-definition) property that does not belong to your organization for the [AWS::StepFunctions::StateMachine](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-stepfunctions-statemachine.html) resource.

## List of service APIs reviewed against data perimeter control objectives

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
* ListStateMachineVersions
* ListStateMachines
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
