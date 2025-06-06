
# Service-specific guidance: AWS Identity and Access Management Access Analyzer


This document outlines service-specific guidance for implementing a data perimeter for AWS Identity and Access Management Access Analyzer. IAM Access Analyzer helps you identify resources in your organization and accounts, such as Amazon S3 buckets or IAM roles, that are shared with an external entity. This service uses logic-based reasoning to analyze resource-based policies and provide actionable findings to help you ensure that your resources are only accessible to intended principals within your specified zone of trust.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | N |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC Endpoint Policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | N |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC Endpoint Policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accesses only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.
 


**List of service APIs reviewed against data perimeter control objectives**


            * CreateAnalyzer
            
            * ListAnalyzers
            
            * CreateAccessPreview
            
            * CreateArchiveRule
            
            * StartPolicyGeneration
            
            * ListFindings
            
            * ListFindingsV2
            
            * StartResourceScan
            
            * GenerateFindingRecommendation
            
            * ApplyArchiveRule
            
            * TagResource
            
            * UpdateArchiveRule
            
            * UpdateFindings
            
            * ListAccessPreviewFindings
            
            * ListAccessPreviews
            
            * ListAnalyzedResources
            
            * ListArchiveRules
            
            * ListPolicyGenerations
            
            * ListTagsForResource
            
            * GetAccessPreview
            
            * GetAnalyzedResource
            
            * GetAnalyzer
            
            * GetArchiveRule
            
            * GetFinding
            
            * GetFindingV2
            
            * GetGeneratedPolicy
            
            * UntagResource
            
            * CancelPolicyGeneration
            
            * CheckNoNewAccess
            
            * ValidatePolicy
            
            * CheckAccessNotGranted
            
            * CheckNoPublicAccess
            
            * DeleteArchiveRule
            
            * DeleteAnalyzer
            

