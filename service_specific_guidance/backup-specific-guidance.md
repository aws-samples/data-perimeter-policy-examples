# Service-specific guidance: AWS Backup


This document outlines service-specific guidance for implementing a data perimeter for AWS Backup. 


AWS Backup is a fully managed backup service that simplifies and centralizes the backup of data across AWS services, including Amazon EBS volumes, Amazon RDS databases, Amazon DynamoDB tables, Amazon EFS file systems, and more. It provides a unified way to create, manage, and automate backup policies, ensuring data protection and compliance with regulatory requirements.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | N |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | N |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.
 

**List of service APIs reviewed against data perimeter control objectives**
* CancelLegalHold
* CreateBackupPlan
* CreateBackupSelection
* CreateBackupVault
* CreateFramework
* CreateLegalHold
* CreateLogicallyAirGappedBackupVault
* CreateReportPlan
* CreateRestoreTestingPlan
* CreateRestoreTestingSelection
* DeleteBackupPlan
* DeleteBackupSelection
* DeleteBackupVault
* DeleteBackupVaultAccessPolicy
* DeleteBackupVaultLockConfiguration
* DeleteBackupVaultNotifications
* DeleteFramework
* DeleteRecoveryPoint
* DeleteReportPlan
* DeleteRestoreTestingPlan
* DeleteRestoreTestingSelection
* DescribeBackupJob
* DescribeBackupVault
* DescribeCopyJob
* DescribeFramework
* DescribeGlobalSettings
* DescribeProtectedResource
* DescribeRecoveryPoint
* DescribeRegionSettings
* DescribeReportJob
* DescribeReportPlan
* DescribeRestoreJob
* ExportBackupPlanTemplate
* GetBackupPlan
* GetBackupPlanFromJSON
* GetBackupPlanFromTemplate
* GetBackupSelection
* GetBackupVaultAccessPolicy
* GetBackupVaultNotifications
* GetLegalHold
* GetRecoveryPointRestoreMetadata
* GetRestoreJobMetadata
* GetRestoreTestingInferredMetadata
* GetRestoreTestingPlan
* GetRestoreTestingSelection
* GetSupportedResourceTypes
* ListBackupJobs
* ListBackupJobSummaries
* ListBackupPlans
* ListBackupPlanTemplates
* ListBackupPlanVersions
* ListBackupSelections
* ListBackupVaults
* ListCopyJobs
* ListCopyJobSummaries
* ListFrameworks
* ListLegalHolds
* ListProtectedResources
* ListProtectedResourcesByBackupVault
* ListRecoveryPointsByBackupVault
* ListRecoveryPointsByLegalHold
* ListRecoveryPointsByResource
* ListReportJobs
* ListReportPlans
* ListRestoreJobs
* ListRestoreJobsByProtectedResource
* ListRestoreJobSummaries
* ListRestoreTestingPlans
* ListRestoreTestingSelections
* ListTags
* PutBackupVaultAccessPolicy
* PutBackupVaultLockConfiguration
* PutBackupVaultNotifications
* PutRestoreValidationResult
* StartBackupJob
* StartCopyJob
* StartReportJob
* StartRestoreJob
* TagResource
* UntagResource
* UpdateBackupPlan
* UpdateFramework
* UpdateGlobalSettings
* UpdateRecoveryPointLifecycle
* UpdateRegionSettings
* UpdateReportPlan
* UpdateRestoreTestingPlan
* UpdateRestoreTestingSelection
