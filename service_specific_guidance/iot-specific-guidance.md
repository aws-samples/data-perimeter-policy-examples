# Service-specific guidance: AWS IoT Core


This document outlines service-specific guidance for implementing a data perimeter for AWS IoT.

AWS IoT provides the cloud services that connect your IoT devices to other devices and AWS cloud services. AWS IoT provides device software that can help you integrate your IoT devices into AWS IoT-based solutions.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | Y |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | N |

*Y - Additional considerations apply. N - No additional considerations apply.

## DescribeManagedJobTemplate
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [DescribeManagedJobTemplate](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeManagedJobTemplate.html) might require access to service-owned job templates, which are job templates that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned job templates in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [iot_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/iot_endpoint_policy.json), which lists service-owned job templates in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## ListManagedJobTemplates
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [ListManagedJobTemplates](https://docs.aws.amazon.com/iot/latest/apireference/API_ListManagedJobTemplates.html) might require access to service-owned job templates, which are job templates that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned job templates in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [iot_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/iot_endpoint_policy.json), which lists service-owned job templates in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## List of service APIs reviewed against data perimeter control objectives

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
* CreateCommand
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
* DeleteCommand
* DeleteCommandExecution
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
* DescribeEncryptionConfiguration
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
* GetCommand
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
* GetThingConnectivityData
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
* ListCommandExecutions
* ListCommands
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
* ListPrincipalThingsV2
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
* ListThingPrincipalsV2
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
* TransferCertificate
* UntagResource
* UpdateAccountAuditConfiguration
* UpdateAuditSuppression
* UpdateAuthorizer
* UpdateBillingGroup
* UpdateCACertificate
* UpdateCertificate
* UpdateCertificateProvider
* UpdateCommand
* UpdateCustomMetric
* UpdateDimension
* UpdateDomainConfiguration
* UpdateDynamicThingGroup
* UpdateEncryptionConfiguration
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
* UpdateThingType
* UpdateTopicRuleDestination
* ValidateSecurityProfileBehaviors
