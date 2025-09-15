# Service-specific guidance: AWS Service Catalog


This document outlines service-specific guidance for implementing a data perimeter for AWS Service Catalog. 


AWS Service Catalog allows organizations to create and manage catalogs of IT services that are approved for use on AWS. It enables administrators to create, organize, and govern a curated set of products, while allowing end-users to quickly deploy approved IT services. This service helps maintain consistency, standardization, and compliance across an organization's AWS environment.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | Y |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | Y |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.
 


**Additional consideration 1**

Perimeter type applicability: identity perimeter applied on resource; resource perimeter applied on identity.
        
CreatePortfolioShare allows you to share a portfolio with another account.

See ["Sid":"PreventExternalResourceShare"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidpreventexternalresourceshare) for a list of resources that can be granted cross-account access.

If you want to restrict access so that only trusted identities can take actions against your resources, consider implementing these additional controls:

* **Preventative control example:** Consider restricting [CreatePortfolioShare](https://docs.aws.amazon.com/servicecatalog/latest/dg/API_CreatePortfolioShare.html) permissions to administrators only using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [AccountId](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-servicecatalog-portfolioshare.html#cfn-servicecatalog-portfolioshare-accountid) property that grants permissions to untrusted identities for the [AWS::ServiceCatalog::PortfolioShare](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-servicecatalog-portfolioshare.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [CreatePortfolioShare](https://docs.aws.amazon.com/servicecatalog/latest/dg/API_CreatePortfolioShare.html) API calls in your environment (specifically, the [AccountId](https://docs.aws.amazon.com/servicecatalog/latest/dg/API_CreatePortfolioShare.html#servicecatalog-CreatePortfolioShare-request-AccountId) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:

* **Detective control example:** Consider using [ListAcceptedPortfolioShares](https://docs.aws.amazon.com/servicecatalog/latest/dg/API_ListAcceptedPortfolioShares.html) to monitor the portfolios shared with your accounts (specifically, the [ARN](https://docs.aws.amazon.com/servicecatalog/latest/dg/API_PortfolioDetail.html#servicecatalog-Type-PortfolioDetail-ARN) response parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 2**

Perimeter type applicability: identity perimeter applied on resource; resource perimeter applied on identity.
        
UpdatePortfolioShare allows you to share a portfolio with another account.

See ["Sid":"PreventExternalResourceShare"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidpreventexternalresourceshare) for a list of resources that can be granted cross-account access.

If you want to restrict access so that only trusted identities can take actions against your resources, consider implementing these additional controls:

* **Preventative control example:** Consider restricting [UpdatePortfolioShare](https://docs.aws.amazon.com/servicecatalog/latest/dg/API_UpdatePortfolioShare.html) permissions to administrators only using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [AccountId](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-servicecatalog-portfolioshare.html#cfn-servicecatalog-portfolioshare-accountid) property that grants permissions to untrusted identities for the [AWS::ServiceCatalog::PortfolioShare](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-servicecatalog-portfolioshare.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [UpdatePortfolioShare](https://docs.aws.amazon.com/servicecatalog/latest/dg/API_UpdatePortfolioShare.html) API calls in your environment (specifically, the [AccountId](https://docs.aws.amazon.com/servicecatalog/latest/dg/API_UpdatePortfolioShare.html#servicecatalog-UpdatePortfolioShare-request-AccountId) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:

* **Detective control example:** Consider using [ListAcceptedPortfolioShares](https://docs.aws.amazon.com/servicecatalog/latest/dg/API_ListAcceptedPortfolioShares.html) to monitor the portfolios shared with your accounts (specifically, the [ARN](https://docs.aws.amazon.com/servicecatalog/latest/dg/API_PortfolioDetail.html#servicecatalog-Type-PortfolioDetail-ARN) response parameter). If necessary, remediate with the responsive controls of your choice.



**List of service APIs reviewed against data perimeter control objectives**
* AssociateBudgetWithResource
* AssociatePrincipalWithPortfolio
* AssociateProductWithPortfolio
* AssociateServiceActionWithProvisioningArtifact
* AssociateTagOptionWithResource
* BatchAssociateServiceActionWithProvisioningArtifact
* BatchDisassociateServiceActionFromProvisioningArtifact
* CopyProduct
* CreateConstraint
* CreatePortfolio
* CreatePortfolioShare
* CreateProduct
* CreateProvisionedProductPlan
* CreateProvisioningArtifact
* CreateServiceAction
* CreateTagOption
* DeleteConstraint
* DeletePortfolio
* DeletePortfolioShare
* DeleteProduct
* DeleteProvisionedProductPlan
* DeleteProvisioningArtifact
* DeleteServiceAction
* DeleteTagOption
* DescribeConstraint
* DescribeCopyProductStatus
* DescribePortfolio
* DescribePortfolioShares
* DescribePortfolioShareStatus
* DescribeProductAsAdmin
* DescribeProvisionedProduct
* DescribeProvisionedProductPlan
* DescribeProvisioningArtifact
* DescribeProvisioningParameters
* DescribeRecord
* DescribeServiceAction
* DescribeServiceActionExecutionParameters
* DescribeTagOption
* DisassociateBudgetFromResource
* DisassociatePrincipalFromPortfolio
* DisassociateProductFromPortfolio
* DisassociateServiceActionFromProvisioningArtifact
* DisassociateTagOptionFromResource
* ExecuteProvisionedProductPlan
* ExecuteProvisionedProductServiceAction
* GetProvisionedProductOutputs
* ImportAsProvisionedProduct
* ListAcceptedPortfolioShares
* ListBudgetsForResource
* ListConstraintsForPortfolio
* ListLaunchPaths
* ListPortfolioAccess
* ListPortfolios
* ListPortfoliosForProduct
* ListPrincipalsForPortfolio
* ListProvisionedProductPlans
* ListProvisioningArtifacts
* ListProvisioningArtifactsForServiceAction
* ListRecordHistory
* ListResourcesForTagOption
* ListServiceActions
* ListServiceActionsForProvisioningArtifact
* ListStackInstancesForProvisionedProduct
* ListTagOptions
* ProvisionProduct
* ScanProvisionedProducts
* SearchProducts
* SearchProductsAsAdmin
* SearchProvisionedProducts
* TerminateProvisionedProduct
* UpdateConstraint
* UpdatePortfolio
* UpdatePortfolioShare
* UpdateProduct
* UpdateProvisionedProduct
* UpdateProvisionedProductProperties
* UpdateProvisioningArtifact
* UpdateServiceAction
* UpdateTagOption