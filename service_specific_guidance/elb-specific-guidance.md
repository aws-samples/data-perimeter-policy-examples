# Service-specific guidance: AWS Elastic Load Balancing


This document outlines service-specific guidance for implementing a data perimeter for AWS Elastic Load Balancing.

Elastic Load Balancing automatically distributes incoming traffic across multiple targets, such as EC2 instances, containers, and IP addresses, in one or more Availability Zones, monitoring the health of registered targets and routing traffic only to healthy ones. It supports Application, Network, Gateway, and Classic Load Balancers and automatically scales load balancer capacity in response to changes in incoming traffic.

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

## ModifyLoadBalancerAttributes
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [ModifyLoadBalancerAttributes](https://docs.aws.amazon.com/elasticloadbalancing/2012-06-01/APIReference/API_ModifyLoadBalancerAttributes.html) allows you to specify an S3 bucket that does not belong to your organization as the value for the `S3BucketName` field of the `AccessLog` attribute in the `LoadBalancerAttributes` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [AccessLoggingPolicy.S3BucketName](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-elasticloadbalancing-loadbalancer-accessloggingpolicy.html#cfn-elasticloadbalancing-loadbalancer-accessloggingpolicy-s3bucketname) property that does not belong to your organization for the [AWS::ElasticLoadBalancing::LoadBalancer](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-elasticloadbalancing-loadbalancer.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [ModifyLoadBalancerAttributes](https://docs.aws.amazon.com/elasticloadbalancing/2012-06-01/APIReference/API_ModifyLoadBalancerAttributes.html) API calls in your environment (specifically, the [S3BucketName](https://docs.aws.amazon.com/elasticloadbalancing/2012-06-01/APIReference/API_AccessLog.html) field of the `AccessLog` attribute in the [LoadBalancerAttributes](https://docs.aws.amazon.com/elasticloadbalancing/2012-06-01/APIReference/API_LoadBalancerAttributes.html) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* AddTags
* ApplySecurityGroupsToLoadBalancer
* AttachLoadBalancerToSubnets
* ConfigureHealthCheck
* CreateAppCookieStickinessPolicy
* CreateLBCookieStickinessPolicy
* CreateLoadBalancer
* CreateLoadBalancerListeners
* CreateLoadBalancerPolicy
* DeleteLoadBalancer
* DeleteLoadBalancerListeners
* DeleteLoadBalancerPolicy
* DeregisterInstancesFromLoadBalancer
* DescribeAccountLimits
* DescribeInstanceHealth
* DescribeLoadBalancerAttributes
* DescribeLoadBalancerPolicies
* DescribeLoadBalancerPolicyTypes
* DescribeLoadBalancers
* DescribeTags
* DetachLoadBalancerFromSubnets
* ModifyLoadBalancerAttributes
* RegisterInstancesWithLoadBalancer
* RemoveTags
* SetLoadBalancerListenerSSLCertificate
* SetLoadBalancerPoliciesOfListener
