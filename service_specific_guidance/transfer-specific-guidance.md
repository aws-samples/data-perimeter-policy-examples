# Service-specific guidance: AWS Transfer Family


This document outlines service-specific guidance for implementing a data perimeter for AWS Transfer Family.

AWS Transfer Family is a secure, fully managed transfer service that enables you to transfer files into and out of AWS storage services (Amazon S3 and Amazon EFS) over SFTP, AS2, FTPS, FTP, and web browser-based transfers. It lets you migrate, automate, and monitor file transfer workflows while maintaining existing client-side configurations for authentication, access, and firewalls.

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

## CreateConnector
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateConnector](https://docs.aws.amazon.com/transfer/latest/APIReference/API_CreateConnector.html) allows you to specify partner AS2 and SFTP endpoints as the value for the `Url` request parameter. Because the subsequent requests against the partner AS2 and SFTP endpoints are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Url](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-transfer-connector.html#cfn-transfer-connector-url) property that does not belong to your organization for the [AWS::Transfer::Connector](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-transfer-connector.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateConnector](https://docs.aws.amazon.com/transfer/latest/APIReference/API_CreateConnector.html) API calls in your environment (specifically, the [Url](https://docs.aws.amazon.com/transfer/latest/APIReference/API_CreateConnector.html#TransferFamily-CreateConnector-request-Url) request parameter). If necessary, remediate with the responsive controls of your choice.

## UpdateConnector
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateConnector](https://docs.aws.amazon.com/transfer/latest/APIReference/API_UpdateConnector.html) allows you to specify partner AS2 or SFTP endpoints as the value for the `Url` request parameter. Because the subsequent requests against the partner AS2 or SFTP endpoints are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Url](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-transfer-connector.html#cfn-transfer-connector-url) property that does not belong to your organization for the [AWS::Transfer::Connector](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-transfer-connector.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateConnector](https://docs.aws.amazon.com/transfer/latest/APIReference/API_UpdateConnector.html) API calls in your environment (specifically, the [Url](https://docs.aws.amazon.com/transfer/latest/APIReference/API_UpdateConnector.html#TransferFamily-UpdateConnector-request-Url) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* CreateAccess
* CreateAgreement
* CreateConnector
* CreateProfile
* CreateServer
* CreateWebApp
* CreateWorkflow
* DeleteAccess
* DeleteAgreement
* DeleteCertificate
* DeleteConnector
* DeleteHostKey
* DeleteProfile
* DeleteServer
* DeleteWebApp
* DeleteWebAppCustomization
* DeleteWorkflow
* DescribeAccess
* DescribeAgreement
* DescribeCertificate
* DescribeConnector
* DescribeHostKey
* DescribeProfile
* DescribeSecurityPolicy
* DescribeServer
* DescribeWebApp
* DescribeWebAppCustomization
* DescribeWorkflow
* ImportCertificate
* ImportHostKey
* ListAccesses
* ListAgreements
* ListCertificates
* ListConnectors
* ListExecutions
* ListFileTransferResults
* ListHostKeys
* ListProfiles
* ListSecurityPolicies
* ListServers
* ListTagsForResource
* ListUsers
* ListWebApps
* ListWorkflows
* StartDirectoryListing
* StartFileTransfer
* StartRemoteDelete
* StartRemoteMove
* StartServer
* StopServer
* TagResource
* TestConnection
* TestIdentityProvider
* UntagResource
* UpdateAccess
* UpdateAgreement
* UpdateCertificate
* UpdateConnector
* UpdateHostKey
* UpdateProfile
* UpdateServer
* UpdateWebApp
* UpdateWebAppCustomization
