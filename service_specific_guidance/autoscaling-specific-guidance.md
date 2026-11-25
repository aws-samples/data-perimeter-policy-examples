# Service-specific guidance: Amazon EC2 Auto Scaling


This document outlines service-specific guidance for implementing a data perimeter for Amazon EC2 Auto Scaling.

Amazon EC2 Auto Scaling helps you ensure that you have the correct number of Amazon EC2 instances available to handle the load for your application. You organize instances into Auto Scaling groups with minimum, maximum, and desired capacity, and Amazon EC2 Auto Scaling launches or terminates instances according to the scaling policies you define and replaces unhealthy instances to maintain that capacity.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | Y |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | N |

*Y - Additional considerations apply. N - No additional considerations apply.

## CreateLaunchConfiguration
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [CreateLaunchConfiguration](https://docs.aws.amazon.com/autoscaling/ec2/APIReference/API_CreateLaunchConfiguration.html) allows you to specify a service role, referred to as the Amazon EC2 instance profile, for your Amazon EC2 instance operations. The role's credentials are provided to the compute environment, which uses them to call AWS APIs on your behalf.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the service-specific SCP, [network_perimeter_ec2_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/network_perimeter_ec2_scp.json). If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## PutNotificationConfiguration
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [PutNotificationConfiguration](https://docs.aws.amazon.com/autoscaling/ec2/APIReference/API_PutNotificationConfiguration.html) allows you to specify an Amazon SNS topic that does not belong to your organization as the value for the `TopicARN` request parameter. Because the subsequent call against the Amazon SNS topic is performed by the service-linked role (SLR), it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [NotificationConfigurations](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-autoscaling-autoscalinggroup.html#cfn-autoscaling-autoscalinggroup-notificationconfigurations) `TopicARN` property that does not belong to your organization for the [AWS::AutoScaling::AutoScalingGroup](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-autoscaling-autoscalinggroup.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [PutNotificationConfiguration](https://docs.aws.amazon.com/autoscaling/ec2/APIReference/API_PutNotificationConfiguration.html) API calls in your environment (specifically, the [TopicARN](https://docs.aws.amazon.com/autoscaling/ec2/APIReference/API_PutNotificationConfiguration.html#API_PutNotificationConfiguration_RequestParameters) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* AttachInstances
* AttachLoadBalancerTargetGroups
* AttachLoadBalancers
* BatchDeleteScheduledAction
* BatchPutScheduledUpdateGroupAction
* CancelInstanceRefresh
* CreateAutoScalingGroup
* CreateLaunchConfiguration
* CreateOrUpdateTags
* DeleteAutoScalingGroup
* DeleteLaunchConfiguration
* DeleteLifecycleHook
* DeleteNotificationConfiguration
* DeletePolicy
* DeleteScheduledAction
* DeleteTags
* DeleteWarmPool
* DescribeAccountLimits
* DescribeAdjustmentTypes
* DescribeAutoScalingGroups
* DescribeAutoScalingInstances
* DescribeAutoScalingNotificationTypes
* DescribeInstanceRefreshes
* DescribeLaunchConfigurations
* DescribeLifecycleHookTypes
* DescribeLifecycleHooks
* DescribeLoadBalancerTargetGroups
* DescribeLoadBalancers
* DescribeMetricCollectionTypes
* DescribeNotificationConfigurations
* DescribePolicies
* DescribeScalingActivities
* DescribeScalingProcessTypes
* DescribeScheduledActions
* DescribeTags
* DescribeTerminationPolicyTypes
* DescribeWarmPool
* DetachInstances
* DetachLoadBalancerTargetGroups
* DetachLoadBalancers
* DisableMetricsCollection
* EnableMetricsCollection
* EnterStandby
* ExecutePolicy
* ExitStandby
* GetPredictiveScalingForecast
* LaunchInstances
* PutLifecycleHook
* PutNotificationConfiguration
* PutScalingPolicy
* PutScheduledUpdateGroupAction
* PutWarmPool
* ResumeProcesses
* SetDesiredCapacity
* SetInstanceHealth
* SetInstanceProtection
* StartInstanceRefresh
* SuspendProcesses
* TerminateInstanceInAutoScalingGroup
* UpdateAutoScalingGroup
