# Service-specific guidance: AWS Elastic Load Balancing V2


This document outlines service-specific guidance for implementing a data perimeter for AWS Elastic Load Balancing V2.

Elastic Load Balancing automatically distributes your incoming traffic across multiple targets, such as EC2 instances, containers, and IP addresses, in one or more Availability Zones, routing traffic only to healthy targets and scaling load balancer capacity automatically as traffic changes. The Elastic Load Balancing V2 API (version 2015-12-01) manages the current-generation Application Load Balancers, Network Load Balancers, and Gateway Load Balancers.

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

**Description**: [ModifyLoadBalancerAttributes](https://docs.aws.amazon.com/elasticloadbalancing/latest/APIReference/API_ModifyLoadBalancerAttributes.html) allows you to specify an S3 bucket that does not belong to your organization as the value for the `Attributes.member.N` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [LoadBalancerAttributes](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-elasticloadbalancingv2-loadbalancer.html#cfn-elasticloadbalancingv2-loadbalancer-loadbalancerattributes) (`access_logs.s3.bucket`, `connection_logs.s3.bucket`, and `health_check_logs.s3.bucket` attribute key) property that does not belong to your organization for the [AWS::ElasticLoadBalancingV2::LoadBalancer](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-elasticloadbalancingv2-loadbalancer.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [ModifyLoadBalancerAttributes](https://docs.aws.amazon.com/elasticloadbalancing/latest/APIReference/API_ModifyLoadBalancerAttributes.html) API calls in your environment (specifically, the `access_logs.s3.bucket`, `connection_logs.s3.bucket`, and `health_check_logs.s3.bucket` attribute keys of the `LoadBalancerAttribute` attribute in the [Attributes.member.N](https://docs.aws.amazon.com/elasticloadbalancing/latest/APIReference/API_ModifyLoadBalancerAttributes.html#API_ModifyLoadBalancerAttributes_RequestParameters) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* AddListenerCertificates
* AddTags
* CreateListener
* CreateLoadBalancer
* CreateRule
* CreateTargetGroup
* CreateTrustStore
* DeleteListener
* DeleteLoadBalancer
* DeleteRule
* DeleteTargetGroup
* DeleteTrustStore
* DeregisterTargets
* DescribeAccountLimits
* DescribeCapacityReservation
* DescribeListenerAttributes
* DescribeListenerCertificates
* DescribeListeners
* DescribeLoadBalancerAttributes
* DescribeLoadBalancers
* DescribeRules
* DescribeSSLPolicies
* DescribeTags
* DescribeTargetGroupAttributes
* DescribeTargetGroups
* DescribeTargetHealth
* DescribeTrustStoreAssociations
* DescribeTrustStoreRevocations
* DescribeTrustStores
* GetResourcePolicy
* GetTrustStoreCaCertificatesBundle
* ModifyIpPools
* ModifyListener
* ModifyLoadBalancerAttributes
* ModifyRule
* ModifyTargetGroup
* ModifyTargetGroupAttributes
* ModifyTrustStore
* RegisterTargets
* RemoveListenerCertificates
* RemoveTags
* SetIpAddressType
* SetRulePriorities
* SetSecurityGroups
* SetSubnets
