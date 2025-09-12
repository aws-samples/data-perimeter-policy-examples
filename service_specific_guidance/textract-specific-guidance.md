
# Service-specific guidance: Amazon Textract


This document outlines service-specific guidance for implementing a data perimeter for Amazon Textract. 

Amazon Textract is a machine learning service that automatically extracts text, handwriting, and data from scanned documents. It goes beyond simple optical character recognition (OCR) to identify, understand, and extract data from forms and tables. Textract enables you to quickly automate document processing workflows, making it easier to process applications, claims, and other structured documents.


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

* AnalyzeDocument
* AnalyzeExpense
* AnalyzeID
* CreateAdapter
* CreateAdapterVersion
* DeleteAdapter
* DeleteAdapterVersion
* DetectDocumentText
* GetAdapter
* GetAdapterVersion
* GetDocumentAnalysis
* GetDocumentTextDetection
* GetExpenseAnalysis
* GetLendingAnalysis
* GetLendingAnalysisSummary
* ListAdapterVersions
* ListAdapters
* ListTagsForResource
* StartDocumentAnalysis
* StartDocumentTextDetection
* StartExpenseAnalysis
* StartLendingAnalysis
* TagResource
* UntagResource
* UpdateAdapter
