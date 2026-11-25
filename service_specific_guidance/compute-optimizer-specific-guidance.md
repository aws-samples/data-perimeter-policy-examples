# Service-specific guidance: AWS Compute Optimizer


This document outlines service-specific guidance for implementing a data perimeter for AWS Compute Optimizer.

AWS Compute Optimizer analyzes the configuration and utilization metrics of your AWS resources to provide rightsizing recommendations and identify idle resources. It reports whether your resources are optimal and recommends changes that lower your costs and improve the performance of your workloads.

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

## ExportAutoScalingGroupRecommendations
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [ExportAutoScalingGroupRecommendations](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportAutoScalingGroupRecommendations.html) allows you to specify an S3 bucket that does not belong to your organization as the value for the `s3DestinationConfig` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Detective control example: Consider using CloudTrail management events to monitor the [ExportAutoScalingGroupRecommendations](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportAutoScalingGroupRecommendations.html) API calls in your environment (specifically, the [s3DestinationConfig](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportAutoScalingGroupRecommendations.html#computeoptimizer-ExportAutoScalingGroupRecommendations-request-s3DestinationConfig) request parameter). If necessary, remediate with the responsive controls of your choice.

## ExportEBSVolumeRecommendations
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [ExportEBSVolumeRecommendations](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportEBSVolumeRecommendations.html) allows you to specify an S3 bucket that does not belong to your organization as the value for the `s3DestinationConfig` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Detective control example: Consider using CloudTrail management events to monitor the [ExportEBSVolumeRecommendations](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportEBSVolumeRecommendations.html) API calls in your environment (specifically, the [s3DestinationConfig](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportEBSVolumeRecommendations.html#computeoptimizer-ExportEBSVolumeRecommendations-request-s3DestinationConfig) request parameter). If necessary, remediate with the responsive controls of your choice.

## ExportEC2InstanceRecommendations
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [ExportEC2InstanceRecommendations](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportEC2InstanceRecommendations.html) allows you to specify an S3 bucket that does not belong to your organization as the value for the `s3DestinationConfig` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Detective control example: Consider using CloudTrail management events to monitor the [ExportEC2InstanceRecommendations](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportEC2InstanceRecommendations.html) API calls in your environment (specifically, the [s3DestinationConfig](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportEC2InstanceRecommendations.html#computeoptimizer-ExportEC2InstanceRecommendations-request-s3DestinationConfig) request parameter). If necessary, remediate with the responsive controls of your choice.

## ExportECSServiceRecommendations
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [ExportECSServiceRecommendations](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportECSServiceRecommendations.html) allows you to specify an S3 bucket that does not belong to your organization as the value for the `s3DestinationConfig` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Detective control example: Consider using CloudTrail management events to monitor the [ExportECSServiceRecommendations](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportECSServiceRecommendations.html) API calls in your environment (specifically, the [s3DestinationConfig](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportECSServiceRecommendations.html#computeoptimizer-ExportECSServiceRecommendations-request-s3DestinationConfig) request parameter). If necessary, remediate with the responsive controls of your choice.

## ExportIdleRecommendations
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [ExportIdleRecommendations](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportIdleRecommendations.html) allows you to specify an S3 bucket that does not belong to your organization as the value for the `s3DestinationConfig` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Detective control example: Consider using CloudTrail management events to monitor the [ExportIdleRecommendations](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportIdleRecommendations.html) API calls in your environment (specifically, the [s3DestinationConfig](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportIdleRecommendations.html#computeoptimizer-ExportIdleRecommendations-request-s3DestinationConfig) request parameter). If necessary, remediate with the responsive controls of your choice.

## ExportLambdaFunctionRecommendations
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [ExportLambdaFunctionRecommendations](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportLambdaFunctionRecommendations.html) allows you to specify an S3 bucket that does not belong to your organization as the value for the `s3DestinationConfig` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Detective control example: Consider using CloudTrail management events to monitor the [ExportLambdaFunctionRecommendations](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportLambdaFunctionRecommendations.html) API calls in your environment (specifically, the [s3DestinationConfig](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportLambdaFunctionRecommendations.html#computeoptimizer-ExportLambdaFunctionRecommendations-request-s3DestinationConfig) request parameter). If necessary, remediate with the responsive controls of your choice.

## ExportLicenseRecommendations
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [ExportLicenseRecommendations](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportLicenseRecommendations.html) allows you to specify an S3 bucket that does not belong to your organization as the value for the `s3DestinationConfig` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Detective control example: Consider using CloudTrail management events to monitor the [ExportLicenseRecommendations](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportLicenseRecommendations.html) API calls in your environment (specifically, the [s3DestinationConfig](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportLicenseRecommendations.html#computeoptimizer-ExportLicenseRecommendations-request-s3DestinationConfig) request parameter). If necessary, remediate with the responsive controls of your choice.

## ExportRDSDatabaseRecommendations
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [ExportRDSDatabaseRecommendations](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportRDSDatabaseRecommendations.html) allows you to specify an S3 bucket that does not belong to your organization as the value for the `s3DestinationConfig` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Detective control example: Consider using CloudTrail management events to monitor the [ExportRDSDatabaseRecommendations](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportRDSDatabaseRecommendations.html) API calls in your environment (specifically, the [s3DestinationConfig](https://docs.aws.amazon.com/compute-optimizer/latest/APIReference/API_ExportRDSDatabaseRecommendations.html#computeoptimizer-ExportRDSDatabaseRecommendations-request-s3DestinationConfig) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* DeleteRecommendationPreferences
* DescribeRecommendationExportJobs
* ExportAutoScalingGroupRecommendations
* ExportEBSVolumeRecommendations
* ExportEC2InstanceRecommendations
* ExportECSServiceRecommendations
* ExportIdleRecommendations
* ExportLambdaFunctionRecommendations
* ExportLicenseRecommendations
* ExportRDSDatabaseRecommendations
* GetAutoScalingGroupRecommendations
* GetEBSVolumeRecommendations
* GetEC2InstanceRecommendations
* GetEC2RecommendationProjectedMetrics
* GetECSServiceRecommendationProjectedMetrics
* GetECSServiceRecommendations
* GetEffectiveRecommendationPreferences
* GetEnrollmentStatus
* GetIdleRecommendations
* GetLambdaFunctionRecommendations
* GetLicenseRecommendations
* GetRDSDatabaseRecommendationProjectedMetrics
* GetRDSDatabaseRecommendations
* GetRecommendationPreferences
* GetRecommendationSummaries
* PutRecommendationPreferences
* UpdateEnrollmentStatus
