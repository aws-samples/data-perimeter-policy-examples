# Service-specific guidance: Amazon Inspector2


This document outlines service-specific guidance for implementing a data perimeter for Amazon Inspector2.

Amazon Inspector is a vulnerability management service that automatically discovers workloads and continually scans them for software vulnerabilities and unintended network exposure. Amazon Inspector discovers and scans Amazon EC2 instances, container images in Amazon ECR, and Lambda functions.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## CreateFindingsReport
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateFindingsReport](https://docs.aws.amazon.com/inspector/v2/APIReference/API_CreateFindingsReport.html) allows you to specify an S3 bucket that does not belong to your organization as the value for the `s3Destination.bucketName` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing the `inspector2:ExpectedBucketOwner` condition key, which filters access by the AWS account ID that must own the destination S3 bucket, in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_resources_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_resources_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateFindingsReport](https://docs.aws.amazon.com/inspector/v2/APIReference/API_CreateFindingsReport.html) API calls in your environment (specifically, the [s3Destination.bucketName](https://docs.aws.amazon.com/inspector/v2/APIReference/API_Destination.html#inspector2-Type-Destination-bucketName) request parameter). If necessary, remediate with the responsive controls of your choice.

### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateFindingsReport](https://docs.aws.amazon.com/inspector/v2/APIReference/API_CreateFindingsReport.html) allows you to specify a KMS key that does not belong to your organization as the value for the `s3Destination.kmsKeyArn` request parameter. Because the subsequent call against the KMS key is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Detective control example: Consider using CloudTrail management events to monitor the [CreateFindingsReport](https://docs.aws.amazon.com/inspector/v2/APIReference/API_CreateFindingsReport.html) API calls in your environment (specifically, the [s3Destination.kmsKeyArn](https://docs.aws.amazon.com/inspector/v2/APIReference/API_Destination.html#inspector2-Type-Destination-kmsKeyArn) request parameter). If necessary, remediate with the responsive controls of your choice.

## CreateSbomExport
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateSbomExport](https://docs.aws.amazon.com/inspector/v2/APIReference/API_CreateSbomExport.html) allows you to specify an S3 bucket that does not belong to your organization as the value for the `s3Destination.bucketName` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing the `inspector2:ExpectedBucketOwner` condition key in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_resources_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_resources_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateSbomExport](https://docs.aws.amazon.com/inspector/v2/APIReference/API_CreateSbomExport.html) API calls in your environment (specifically, the [s3Destination.bucketName](https://docs.aws.amazon.com/inspector/v2/APIReference/API_Destination.html#inspector2-Type-Destination-bucketName) request parameter). If necessary, remediate with the responsive controls of your choice.

### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateSbomExport](https://docs.aws.amazon.com/inspector/v2/APIReference/API_CreateSbomExport.html) allows you to specify a KMS key that does not belong to your organization as the value for the `s3Destination.kmsKeyArn` request parameter. Because the subsequent call against the KMS key is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Detective control example: Consider using CloudTrail management events to monitor the [CreateSbomExport](https://docs.aws.amazon.com/inspector/v2/APIReference/API_CreateSbomExport.html) API calls in your environment (specifically, the [s3Destination.kmsKeyArn](https://docs.aws.amazon.com/inspector/v2/APIReference/API_Destination.html#inspector2-Type-Destination-kmsKeyArn) request parameter). If necessary, remediate with the responsive controls of your choice.

## GetCisScanReport
### S3 presigned URLs created by service principals

**Perimeter type applicability**: network perimeter applied on resource.

**Description**: [GetCisScanReport](https://docs.aws.amazon.com/inspector/v2/APIReference/API_GetCisScanReport.html) returns an Amazon S3 presigned URL that users can use to download CIS scan reports from service-owned buckets. The presigned URL is created with the Amazon Inspector service principal.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [GetCisScanReport](https://docs.aws.amazon.com/inspector/v2/APIReference/API_GetCisScanReport.html) permissions to select principals using an SCP. See [restrict_presignedURL_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [GetCisScanReport](https://docs.aws.amazon.com/inspector/v2/APIReference/API_GetCisScanReport.html) API calls in your environment. If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* BatchAssociateCodeSecurityScanConfiguration
* BatchDisassociateCodeSecurityScanConfiguration
* BatchGetAccountStatus
* BatchGetCodeSnippet
* BatchGetFindingDetails
* BatchGetFreeTrialInfo
* BatchGetMemberEc2DeepInspectionStatus
* BatchUpdateMemberEc2DeepInspectionStatus
* CancelFindingsReport
* CancelSbomExport
* CreateCisScanConfiguration
* CreateCodeSecurityIntegration
* CreateCodeSecurityScanConfiguration
* CreateFilter
* CreateFindingsReport
* CreateSbomExport
* DeleteCisScanConfiguration
* DeleteCodeSecurityIntegration
* DeleteCodeSecurityScanConfiguration
* DeleteFilter
* DescribeOrganizationConfiguration
* Disable
* DisassociateMember
* Enable
* GetCisScanReport
* GetCisScanResultDetails
* GetClustersForImage
* GetCodeSecurityIntegration
* GetCodeSecurityScanConfiguration
* GetConfiguration
* GetEc2DeepInspectionConfiguration
* GetEncryptionKey
* GetFindingsReportStatus
* GetSbomExport
* ListAccountPermissions
* ListCisScanConfigurations
* ListCisScanResultsAggregatedByChecks
* ListCisScanResultsAggregatedByTargetResource
* ListCisScans
* ListCodeSecurityIntegrations
* ListCodeSecurityScanConfigurationAssociations
* ListCodeSecurityScanConfigurations
* ListCoverage
* ListCoverageStatistics
* ListFilters
* ListFindingAggregations
* ListFindings
* ListMembers
* ListTagsForResource
* ListUsageTotals
* ResetEncryptionKey
* SearchVulnerabilities
* TagResource
* UntagResource
* UpdateCisScanConfiguration
* UpdateCodeSecurityScanConfiguration
* UpdateConfiguration
* UpdateEc2DeepInspectionConfiguration
* UpdateFilter
* UpdateOrganizationConfiguration
