
# Service-specific guidance: AWS Identity and Access Management Access Analyzer


This document outlines service-specific guidance for implementing a data perimeter for AWS Identity and Access Management Access Analyzer. 

IAM Access Analyzer helps you to set, verify, and refine your IAM policies by providing a suite of capabilities. Its features include findings for external and unused access, basic and custom policy checks for validating policies, and policy generation to generate fine-grained policies. 


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

* ApplyArchiveRule
* CancelPolicyGeneration
* CheckAccessNotGranted
* CheckNoNewAccess
* CheckNoPublicAccess
* CreateAccessPreview
* CreateAnalyzer
* CreateArchiveRule
* DeleteAnalyzer
* DeleteArchiveRule
* GenerateFindingRecommendation
* GetAccessPreview
* GetAnalyzedResource
* GetAnalyzer
* GetArchiveRule
* GetFinding
* GetFindingV2
* GetGeneratedPolicy
* ListAccessPreviewFindings
* ListAccessPreviews
* ListAnalyzedResources
* ListAnalyzers
* ListArchiveRules
* ListFindings
* ListFindingsV2
* ListPolicyGenerations
* ListTagsForResource
* StartPolicyGeneration
* StartResourceScan
* TagResource
* UntagResource
* UpdateArchiveRule
* UpdateFindings
* ValidatePolicy



