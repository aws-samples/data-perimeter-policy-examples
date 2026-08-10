# Service-specific guidance: AWS Backup


This document outlines service-specific guidance for implementing a data perimeter for AWS Backup.

AWS Backup is a fully-managed service that centralizes and automates data protection across AWS services, in the cloud, and on premises. It lets customers configure backup policies and monitor backup activity for their AWS resources in one place.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | N |

*Y - Additional considerations apply. N - No additional considerations apply.

## List of service APIs reviewed against data perimeter control objectives

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
* ListBackupJobSummaries
* ListBackupJobs
* ListBackupPlanTemplates
* ListBackupPlanVersions
* ListBackupPlans
* ListBackupSelections
* ListBackupVaults
* ListCopyJobSummaries
* ListCopyJobs
* ListFrameworks
* ListIndexedRecoveryPoints
* ListLegalHolds
* ListProtectedResources
* ListProtectedResourcesByBackupVault
* ListRecoveryPointsByBackupVault
* ListRecoveryPointsByLegalHold
* ListRecoveryPointsByResource
* ListReportJobs
* ListReportPlans
* ListRestoreAccessBackupVaults
* ListRestoreJobSummaries
* ListRestoreJobs
* ListRestoreJobsByProtectedResource
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
