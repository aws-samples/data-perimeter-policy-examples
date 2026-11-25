# Service-specific guidance: AWS CodeCommit


This document outlines service-specific guidance for implementing a data perimeter for AWS CodeCommit.

AWS CodeCommit is a secure, highly scalable, managed source control service that hosts private Git repositories, letting you privately store and manage assets such as documents, source code, and binary files in the cloud. It supports the standard functionality of Git, so it works with your existing Git-based tools and removes the need to manage your own source control system or scale its infrastructure.

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

## PutRepositoryTriggers
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [PutRepositoryTriggers](https://docs.aws.amazon.com/codecommit/latest/APIReference/API_PutRepositoryTriggers.html) allows you to specify an Amazon SNS topic or AWS Lambda function that does not belong to your organization as the value for the `triggers[].destinationArn` request parameter. Because the subsequent call against the Amazon SNS topic or AWS Lambda function is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Triggers.DestinationArn](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-codecommit-repository-repositorytrigger.html#cfn-codecommit-repository-repositorytrigger-destinationarn) property that does not belong to your organization for the [AWS::CodeCommit::Repository](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codecommit-repository.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [PutRepositoryTriggers](https://docs.aws.amazon.com/codecommit/latest/APIReference/API_PutRepositoryTriggers.html) API calls in your environment (specifically, the [triggers[].destinationArn](https://docs.aws.amazon.com/codecommit/latest/APIReference/API_RepositoryTrigger.html#CodeCommit-Type-RepositoryTrigger-destinationArn) request parameter). If necessary, remediate with the responsive controls of your choice.

## TestRepositoryTriggers
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [TestRepositoryTriggers](https://docs.aws.amazon.com/codecommit/latest/APIReference/API_TestRepositoryTriggers.html) allows you to specify an Amazon SNS topic or AWS Lambda function that does not belong to your organization as the value for the `triggers[].destinationArn` request parameter. Because the subsequent call against the Amazon SNS topic or AWS Lambda function is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Detective control example: Consider using CloudTrail management events to monitor the [TestRepositoryTriggers](https://docs.aws.amazon.com/codecommit/latest/APIReference/API_TestRepositoryTriggers.html) API calls in your environment (specifically, the [triggers[].destinationArn](https://docs.aws.amazon.com/codecommit/latest/APIReference/API_RepositoryTrigger.html#CodeCommit-Type-RepositoryTrigger-destinationArn) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* AssociateApprovalRuleTemplateWithRepository
* BatchAssociateApprovalRuleTemplateWithRepositories
* BatchDescribeMergeConflicts
* BatchDisassociateApprovalRuleTemplateFromRepositories
* BatchGetCommits
* BatchGetRepositories
* CreateApprovalRuleTemplate
* CreateBranch
* CreateCommit
* CreatePullRequest
* CreatePullRequestApprovalRule
* CreateRepository
* CreateUnreferencedMergeCommit
* DeleteApprovalRuleTemplate
* DeleteBranch
* DeleteCommentContent
* DeleteFile
* DeletePullRequestApprovalRule
* DeleteRepository
* DescribeMergeConflicts
* DescribePullRequestEvents
* DisassociateApprovalRuleTemplateFromRepository
* EvaluatePullRequestApprovalRules
* GetApprovalRuleTemplate
* GetBlob
* GetBranch
* GetComment
* GetCommentReactions
* GetCommentsForComparedCommit
* GetCommentsForPullRequest
* GetCommit
* GetDifferences
* GetFile
* GetFolder
* GetMergeConflicts
* GetMergeOptions
* GetPullRequest
* GetPullRequestApprovalStates
* GetPullRequestOverrideState
* GetRepository
* GetRepositoryTriggers
* ListApprovalRuleTemplates
* ListAssociatedApprovalRuleTemplatesForRepository
* ListBranches
* ListFileCommitHistory
* ListPullRequests
* ListRepositories
* ListRepositoriesForApprovalRuleTemplate
* ListTagsForResource
* MergeBranchesByFastForward
* MergeBranchesBySquash
* MergeBranchesByThreeWay
* MergePullRequestByFastForward
* MergePullRequestBySquash
* MergePullRequestByThreeWay
* OverridePullRequestApprovalRules
* PostCommentForComparedCommit
* PostCommentForPullRequest
* PostCommentReply
* PutCommentReaction
* PutFile
* PutRepositoryTriggers
* TagResource
* TestRepositoryTriggers
* UntagResource
* UpdateApprovalRuleTemplateContent
* UpdateApprovalRuleTemplateDescription
* UpdateApprovalRuleTemplateName
* UpdateComment
* UpdateDefaultBranch
* UpdatePullRequestApprovalRuleContent
* UpdatePullRequestDescription
* UpdatePullRequestStatus
* UpdatePullRequestTitle
* UpdateRepositoryDescription
* UpdateRepositoryEncryptionKey
* UpdateRepositoryName
