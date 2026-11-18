# Service-specific guidance: AWS Artifact


This document outlines service-specific guidance for implementing a data perimeter for AWS Artifact.

AWS Artifact provides on-demand downloads of AWS security and compliance documents, such as ISO certifications, PCI Security Standards reports, and System and Organization Controls (SOC) reports. It also lets customers review, accept, and track the status of AWS agreements (such as the Business Associate Addendum) for a single account or across accounts in an organization.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | Y |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | Y |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## All API operations
### No VPC endpoint policy support

**Perimeter type applicability**: identity and resource perimeter applied on network.

**Description**: The service does not currently support VPC endpoint policies.

**Additional controls**:

If you want to restrict access to your networks to trusted identities, consider implementing this additional control:
* Preventative control example: Consider using your existing security appliances such as outbound proxies to inspect service API calls in your environment for the identities making the calls, and restrict the calls accordingly. This type of solution might have implications for security, scalability, latency, and reliability that you should evaluate carefully.

If you want to restrict access to your networks to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing `aws:ResourceOrgID` in an SCP to restrict service API calls so that your identities can only access trusted resources. See [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for an example policy.
* Preventative control example: Consider using your existing security appliances such as outbound proxies to inspect service API calls in your environment for the resources being accessed, and restrict the calls accordingly. This type of solution might have implications for security, scalability, latency, and reliability that you should evaluate carefully.

## GetReport
### S3 presigned URLs created by service principals

**Perimeter type applicability**: network perimeter applied on resource.

**Description**: [GetReport](https://docs.aws.amazon.com/artifact/latest/APIReference/API_GetReport.html) returns an Amazon S3 presigned URL that users can use to download AWS Artifact reports from service-owned buckets. The presigned URL is created with the AWS Artifact service principal.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [GetReport](https://docs.aws.amazon.com/artifact/latest/APIReference/API_GetReport.html) permissions to select principals using an SCP. See [restrict_presignedURL_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [GetReport](https://docs.aws.amazon.com/artifact/latest/APIReference/API_GetReport.html) API calls in your environment. If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* GetAccountSettings
* GetReport
* GetReportMetadata
* GetTermForReport
* ListCustomerAgreements
* ListReportVersions
* ListReports
* PutAccountSettings
