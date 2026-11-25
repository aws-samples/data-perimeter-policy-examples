# Service-specific guidance: Amazon Kinesis Analytics V2


This document outlines service-specific guidance for implementing a data perimeter for Amazon Kinesis Analytics V2.

Amazon Managed Service for Apache Flink (previously known as Amazon Kinesis Data Analytics for Apache Flink, and exposed through the Kinesis Analytics V2 API) is a fully managed service that you can use to process and analyze streaming data using Java, Python, SQL, or Scala. It provides the underlying infrastructure for Apache Flink applications, handling compute provisioning, AZ failover resilience, parallel computation, automatic scaling, and application backups, so you can run code against streaming and static sources to perform time-series analytics, feed real-time dashboards, and create real-time metrics.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | Y |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | Y |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | Y |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## CreateApplication
### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [CreateApplication](https://docs.aws.amazon.com/managed-flink/latest/apiv2/API_CreateApplication.html) allows you to associate a Managed Service for Apache Flink application with an Amazon VPC to run your code.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help ensure that developers specify the [ApplicationConfiguration.VpcConfigurations](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-kinesisanalyticsv2-application-applicationconfiguration.html#cfn-kinesisanalyticsv2-application-applicationconfiguration-vpcconfigurations) property of the [AWS::KinesisAnalyticsV2::Application](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-kinesisanalyticsv2-application.html) resource.
* Detective control example: Consider implementing a custom AWS Config rule to help detect Managed Service for Apache Flink applications not associated with a VPC or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateApplication](https://docs.aws.amazon.com/managed-flink/latest/apiv2/API_CreateApplication.html) API calls in your environment (specifically, the [ApplicationConfiguration.VpcConfigurations](https://docs.aws.amazon.com/managed-flink/latest/apiv2/API_VpcConfiguration.html) request parameter). If necessary, remediate with the responsive controls of your choice.

### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [CreateApplication](https://docs.aws.amazon.com/managed-flink/latest/apiv2/API_CreateApplication.html) allows you to specify a service role, referred to as the service execution role, for your Managed Service for Apache Flink application operations. The role's credentials are provided to the compute environment, which uses them to call AWS APIs on your behalf.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the service execution role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## DeleteApplicationVpcConfiguration
### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [DeleteApplicationVpcConfiguration](https://docs.aws.amazon.com/managed-flink/latest/apiv2/API_DeleteApplicationVpcConfiguration.html) allows you to dissociate a Managed Service for Apache Flink application from an Amazon VPC.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider restricting [DeleteApplicationVpcConfiguration](https://docs.aws.amazon.com/managed-flink/latest/apiv2/API_DeleteApplicationVpcConfiguration.html) permissions to select principals using an SCP. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help ensure that developers specify the [ApplicationConfiguration.VpcConfigurations](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-kinesisanalyticsv2-application-applicationconfiguration.html#cfn-kinesisanalyticsv2-application-applicationconfiguration-vpcconfigurations) property of the [AWS::KinesisAnalyticsV2::Application](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-kinesisanalyticsv2-application.html) resource.
* Detective control example: Consider implementing a custom AWS Config rule to help detect Managed Service for Apache Flink applications not associated with a VPC or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [DeleteApplicationVpcConfiguration](https://docs.aws.amazon.com/managed-flink/latest/apiv2/API_DeleteApplicationVpcConfiguration.html) API calls in your environment (specifically, the [VpcConfigurationId](https://docs.aws.amazon.com/managed-flink/latest/apiv2/API_DeleteApplicationVpcConfiguration.html#APIReference-DeleteApplicationVpcConfiguration-request-VpcConfigurationId) request parameter). If necessary, remediate with the responsive controls of your choice.

## UpdateApplication
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [UpdateApplication](https://docs.aws.amazon.com/managed-flink/latest/apiv2/API_UpdateApplication.html) allows you to specify a service role, referred to as the service execution role, for your Managed Service for Apache Flink application operations. The role's credentials are provided to the compute environment, which uses them to call AWS APIs on your behalf.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the service execution role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* AddApplicationCloudWatchLoggingOption
* AddApplicationVpcConfiguration
* CreateApplication
* CreateApplicationPresignedUrl
* CreateApplicationSnapshot
* DeleteApplication
* DeleteApplicationCloudWatchLoggingOption
* DeleteApplicationSnapshot
* DeleteApplicationVpcConfiguration
* DescribeApplication
* DescribeApplicationOperation
* DescribeApplicationSnapshot
* DescribeApplicationVersion
* DiscoverInputSchema
* ListApplicationOperations
* ListApplicationSnapshots
* ListApplicationVersions
* ListApplications
* ListTagsForResource
* StartApplication
* StopApplication
* TagResource
* UntagResource
* UpdateApplication
* UpdateApplicationMaintenanceConfiguration
