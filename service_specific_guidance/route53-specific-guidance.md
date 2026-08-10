# Service-specific guidance: Amazon Route 53


This document outlines service-specific guidance for implementing a data perimeter for Amazon Route 53.

Amazon Route 53 is a highly available and scalable Domain Name System (DNS) web service. You can use Route 53 to perform three main functions in any combination: domain registration, DNS routing, and health checking.

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

## AssociateVPCWithHostedZone
### No ResourceOrgID support

**Perimeter type applicability**: resource perimeter on identity.

**Description**: [AssociateVPCWithHostedZone](https://docs.aws.amazon.com/Route53/latest/APIReference/API_AssociateVPCWithHostedZone.html) does not currently support the `aws:ResourceOrgID` condition key for the `HostedZoneId` request parameter.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider restricting [AssociateVPCWithHostedZone](https://docs.aws.amazon.com/Route53/latest/APIReference/API_AssociateVPCWithHostedZone.html) permissions to select principals using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy and ["Sid":"ProtectActionsNotSupportedByPrimaryDPControls"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidprotectactionsnotsupportedbyprimarydpcontrols) for other APIs that currently don't support the `aws:ResourceOrgID` condition key.
* Detective control example: Consider using CloudTrail management events to monitor the [AssociateVPCWithHostedZone](https://docs.aws.amazon.com/Route53/latest/APIReference/API_AssociateVPCWithHostedZone.html) API calls in your environment (specifically, the [HostedZoneId](https://docs.aws.amazon.com/Route53/latest/APIReference/API_AssociateVPCWithHostedZone.html#Route53-AssociateVPCWithHostedZone-request-uri-HostedZoneId) request parameter). If necessary, remediate with the responsive controls of your choice.

## CreateVPCAssociationAuthorization
### No ResourceOrgID support

**Perimeter type applicability**: resource perimeter on identity.

**Description**: [CreateVPCAssociationAuthorization](https://docs.aws.amazon.com/Route53/latest/APIReference/API_CreateVPCAssociationAuthorization.html) does not currently support the `aws:ResourceOrgID` condition key for the `VPC` request parameter.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider restricting [CreateVPCAssociationAuthorization](https://docs.aws.amazon.com/Route53/latest/APIReference/API_CreateVPCAssociationAuthorization.html) permissions to select principals using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy and ["Sid":"ProtectActionsNotSupportedByPrimaryDPControls"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidprotectactionsnotsupportedbyprimarydpcontrols) for other APIs that currently don't support the `aws:ResourceOrgID` condition key.
* Preventative control example: Consider implementing the `route53:VPCs` condition key in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_resources_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_resources_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateVPCAssociationAuthorization](https://docs.aws.amazon.com/Route53/latest/APIReference/API_CreateVPCAssociationAuthorization.html) API calls in your environment (specifically, the [VPC](https://docs.aws.amazon.com/Route53/latest/APIReference/API_VPC.html) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

* ActivateKeySigningKey
* AssociateVPCWithHostedZone
* ChangeCidrCollection
* ChangeResourceRecordSets
* ChangeTagsForResource
* CreateCidrCollection
* CreateHealthCheck
* CreateHostedZone
* CreateKeySigningKey
* CreateQueryLoggingConfig
* CreateReusableDelegationSet
* CreateTrafficPolicy
* CreateTrafficPolicyInstance
* CreateTrafficPolicyVersion
* CreateVPCAssociationAuthorization
* DeactivateKeySigningKey
* DeleteCidrCollection
* DeleteHealthCheck
* DeleteHostedZone
* DeleteKeySigningKey
* DeleteQueryLoggingConfig
* DeleteReusableDelegationSet
* DeleteTrafficPolicy
* DeleteTrafficPolicyInstance
* DeleteVPCAssociationAuthorization
* DisableHostedZoneDNSSEC
* DisassociateVPCFromHostedZone
* EnableHostedZoneDNSSEC
* GetAccountLimit
* GetChange
* GetCheckerIpRanges
* GetDNSSEC
* GetGeoLocation
* GetHealthCheck
* GetHealthCheckCount
* GetHealthCheckLastFailureReason
* GetHealthCheckStatus
* GetHostedZone
* GetHostedZoneCount
* GetHostedZoneLimit
* GetQueryLoggingConfig
* GetReusableDelegationSet
* GetReusableDelegationSetLimit
* GetTrafficPolicy
* GetTrafficPolicyInstance
* GetTrafficPolicyInstanceCount
* ListCidrBlocks
* ListCidrCollections
* ListCidrLocations
* ListGeoLocations
* ListHealthChecks
* ListHostedZones
* ListHostedZonesByName
* ListHostedZonesByVPC
* ListQueryLoggingConfigs
* ListResourceRecordSets
* ListReusableDelegationSets
* ListTagsForResource
* ListTagsForResources
* ListTrafficPolicies
* ListTrafficPolicyInstances
* ListTrafficPolicyInstancesByHostedZone
* ListTrafficPolicyInstancesByPolicy
* ListTrafficPolicyVersions
* ListVPCAssociationAuthorizations
* TestDNSAnswer
* UpdateHealthCheck
* UpdateHostedZoneComment
* UpdateTrafficPolicyComment
* UpdateTrafficPolicyInstance
