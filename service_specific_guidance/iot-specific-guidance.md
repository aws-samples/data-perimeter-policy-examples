# Service-specific guidance: AWS IoT Core


This document outlines service-specific guidance for implementing a data perimeter for AWS IoT Core. 


AWS IoT Core is a managed cloud service that enables secure communication between Internet of Things (IoT) devices and cloud applications. It provides a scalable platform for connecting and managing billions of devices, processing and routing IoT data, and facilitating interactions between devices and cloud services. AWS IoT Core supports various protocols and offers features like device authentication, data encryption, and rules engine for data processing and integration with other AWS services.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | N |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | Y |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | Y |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.
 






**Additional consideration 1**

Perimeter type applicability: resource perimeter.
        
ListManagedJobTemplates might require access to service-owned job templates, which are job templates managed in service accounts.

See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access. 
If you want to restrict access to trusted resources, see [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for information about how the default resource perimeter SCP accounts for such an access pattern. Use the service-specific policy, [iot_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/iot_endpoint_policy.json) to enforce the resource perimeter on your network.


**Additional consideration 2**

Perimeter type applicability: resource perimeter.
        
DescribeManagedJobTemplate might require access to service-owned job templates, which are job templates managed in service accounts

See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access. 
If you want to restrict access to trusted resources, see [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for information about how the default resource perimeter SCP accounts for such an access pattern. Use the service-specific policy, [iot_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/iot_endpoint_policy.json) to enforce the resource perimeter on your network.





**List of service APIs reviewed against data perimeter control objectives**
* AddThingToBillingGroup
* AddThingToThingGroup
* AssociateTargetsWithJob
* AttachPolicy
* AttachPrincipalPolicy
* AttachSecurityProfile
* AttachThingPrincipal
* CancelAuditMitigationActionsTask
* CancelAuditTask
* CancelJob
* CancelJobExecution
* ClearDefaultAuthorizer
* CreateAuditSuppression
* CreateAuthorizer
* CreateBillingGroup
* CreateCertificateFromCsr
* CreateCertificateProvider
* CreateCustomMetric
* CreateDimension
* CreateDomainConfiguration
* CreateDynamicThingGroup
* CreateFleetMetric
* CreateJob
* CreateJobTemplate
* CreateKeysAndCertificate
* CreateMitigationAction
* CreateOTAUpdate
* CreatePackage
* CreatePackageVersion
* CreatePolicy
* CreatePolicyVersion
* CreateProvisioningClaim
* CreateProvisioningTemplate
* CreateProvisioningTemplateVersion
* CreateRoleAlias
* CreateScheduledAudit
* CreateSecurityProfile
* CreateStream
* CreateThing
* CreateThingGroup
* CreateThingType
* CreateTopicRule
* CreateTopicRuleDestination
* DeleteAuditSuppression
* DeleteAuthorizer
* DeleteBillingGroup
* DeleteCACertificate
* DeleteCertificate
* DeleteCertificateProvider
* DeleteCustomMetric
* DeleteDimension
* DeleteDomainConfiguration
* DeleteDynamicThingGroup
* DeleteFleetMetric
* DeleteJob
* DeleteJobExecution
* DeleteJobTemplate
* DeleteMitigationAction
* DeleteOTAUpdate
* DeletePackage
* DeletePackageVersion
* DeletePolicy
* DeletePolicyVersion
* DeleteProvisioningTemplate
* DeleteProvisioningTemplateVersion
* DeleteRegistrationCode
* DeleteRoleAlias
* DeleteScheduledAudit
* DeleteSecurityProfile
* DeleteStream
* DeleteThing
* DeleteThingGroup
* DeleteThingType
* DeleteTopicRule
* DeleteTopicRuleDestination
* DeleteV2LoggingLevel
* DeprecateThingType
* DescribeAccountAuditConfiguration
* DescribeAuditFinding
* DescribeAuditMitigationActionsTask
* DescribeAuditSuppression
* DescribeAuditTask
* DescribeAuthorizer
* DescribeBillingGroup
* DescribeCACertificate
* DescribeCertificate
* DescribeCertificateProvider
* DescribeCustomMetric
* DescribeDefaultAuthorizer
* DescribeDimension
* DescribeDomainConfiguration
* DescribeEndpoint
* DescribeEventConfigurations
* DescribeFleetMetric
* DescribeIndex
* DescribeJob
* DescribeJobExecution
* DescribeJobTemplate
* DescribeManagedJobTemplate
* DescribeMitigationAction
* DescribeProvisioningTemplate
* DescribeProvisioningTemplateVersion
* DescribeRoleAlias
* DescribeScheduledAudit
* DescribeSecurityProfile
* DescribeStream
* DescribeThing
* DescribeThingGroup
* DescribeThingRegistrationTask
* DescribeThingType
* DetachPolicy
* DetachSecurityProfile
* DetachThingPrincipal
* DisableTopicRule
* EnableTopicRule
* GetBehaviorModelTrainingSummaries
* GetBucketsAggregation
* GetCardinality
* GetEffectivePolicies
* GetIndexingConfiguration
* GetJobDocument
* GetLoggingOptions
* GetOTAUpdate
* GetPackage
* GetPackageConfiguration
* GetPackageVersion
* GetPercentiles
* GetPolicy
* GetPolicyVersion
* GetRegistrationCode
* GetStatistics
* GetTopicRule
* GetTopicRuleDestination
* GetV2LoggingOptions
* ListActiveViolations
* ListAttachedPolicies
* ListAuditFindings
* ListAuditMitigationActionsTasks
* ListAuditSuppressions
* ListAuditTasks
* ListAuthorizers
* ListBillingGroups
* ListCACertificates
* ListCertificateProviders
* ListCertificates
* ListCertificatesByCA
* ListCustomMetrics
* ListDetectMitigationActionsExecutions
* ListDetectMitigationActionsTasks
* ListDimensions
* ListDomainConfigurations
* ListFleetMetrics
* ListIndices
* ListJobExecutionsForJob
* ListJobExecutionsForThing
* ListJobTemplates
* ListJobs
* ListManagedJobTemplates
* ListMetricValues
* ListMitigationActions
* ListOTAUpdates
* ListOutgoingCertificates
* ListPackageVersions
* ListPackages
* ListPolicies
* ListPolicyPrincipals
* ListPolicyVersions
* ListPrincipalPolicies
* ListPrincipalThings
* ListProvisioningTemplateVersions
* ListProvisioningTemplates
* ListRelatedResourcesForAuditFinding
* ListRoleAliases
* ListScheduledAudits
* ListSecurityProfiles
* ListSecurityProfilesForTarget
* ListStreams
* ListTagsForResource
* ListTargetsForPolicy
* ListTargetsForSecurityProfile
* ListThingGroups
* ListThingGroupsForThing
* ListThingPrincipals
* ListThingRegistrationTaskReports
* ListThingRegistrationTasks
* ListThingTypes
* ListThings
* ListThingsInBillingGroup
* ListThingsInThingGroup
* ListTopicRuleDestinations
* ListTopicRules
* ListV2LoggingLevels
* ListViolationEvents
* RegisterCACertificate
* RegisterCertificate
* RegisterCertificateWithoutCA
* RegisterThing
* RemoveThingFromBillingGroup
* RemoveThingFromThingGroup
* ReplaceTopicRule
* SearchIndex
* SetDefaultAuthorizer
* SetDefaultPolicyVersion
* SetLoggingOptions
* SetV2LoggingLevel
* SetV2LoggingOptions
* StartAuditMitigationActionsTask
* StartOnDemandAuditTask
* StartThingRegistrationTask
* StopThingRegistrationTask
* TagResource
* TestAuthorization
* TestInvokeAuthorizer
* TransferCertificate
* UntagResource
* UpdateAccountAuditConfiguration
* UpdateAuditSuppression
* UpdateAuthorizer
* UpdateBillingGroup
* UpdateCACertificate
* UpdateCertificate
* UpdateCertificateProvider
* UpdateCustomMetric
* UpdateDimension
* UpdateDomainConfiguration
* UpdateDynamicThingGroup
* UpdateEventConfigurations
* UpdateFleetMetric
* UpdateIndexingConfiguration
* UpdateJob
* UpdateMitigationAction
* UpdatePackage
* UpdatePackageConfiguration
* UpdatePackageVersion
* UpdateProvisioningTemplate
* UpdateRoleAlias
* UpdateScheduledAudit
* UpdateSecurityProfile
* UpdateStream
* UpdateThing
* UpdateThingGroup
* UpdateThingGroupsForThing
* UpdateTopicRuleDestination
* ValidateSecurityProfileBehaviors
