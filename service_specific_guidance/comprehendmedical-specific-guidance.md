# Service-specific guidance: Amazon Comprehend Medical


This document outlines service-specific guidance for implementing a data perimeter for Amazon Comprehend Medical.

Amazon Comprehend Medical is a HIPAA-eligible natural language processing (NLP) service that detects and returns useful medical information, such as medical conditions, medications, and Protected Health Information (PHI), from unstructured clinical text like physician's notes, discharge summaries, test results, and case notes. It can also link detected entities to standardized medical ontologies such as RxNorm, ICD-10-CM, and SNOMED CT.

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

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* DescribeEntitiesDetectionV2Job
* DescribeICD10CMInferenceJob
* DescribePHIDetectionJob
* DescribeRxNormInferenceJob
* DescribeSNOMEDCTInferenceJob
* DetectEntities
* DetectEntitiesV2
* DetectPHI
* InferICD10CM
* InferRxNorm
* InferSNOMEDCT
* ListEntitiesDetectionV2Jobs
* ListICD10CMInferenceJobs
* ListPHIDetectionJobs
* ListRxNormInferenceJobs
* ListSNOMEDCTInferenceJobs
* StartEntitiesDetectionV2Job
* StartICD10CMInferenceJob
* StartPHIDetectionJob
* StartRxNormInferenceJob
* StartSNOMEDCTInferenceJob
* StopEntitiesDetectionV2Job
* StopICD10CMInferenceJob
* StopPHIDetectionJob
* StopRxNormInferenceJob
* StopSNOMEDCTInferenceJob
