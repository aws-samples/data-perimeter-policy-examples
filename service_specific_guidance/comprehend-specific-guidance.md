# Service-specific guidance: Amazon Comprehend


This document outlines service-specific guidance for implementing a data perimeter for Amazon Comprehend.

Amazon Comprehend uses natural language processing (NLP) to extract insights about the content of documents, recognizing the entities, key phrases, language, sentiments, and other common elements in a document. You can run real-time analysis for small workloads or asynchronous analysis jobs for large document sets, using either Comprehend's pre-trained models or custom models that you train for classification and entity recognition.

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

* BatchDetectDominantLanguage
* BatchDetectEntities
* BatchDetectKeyPhrases
* BatchDetectSentiment
* BatchDetectSyntax
* BatchDetectTargetedSentiment
* ClassifyDocument
* ContainsPiiEntities
* CreateDataset
* CreateDocumentClassifier
* CreateEndpoint
* CreateEntityRecognizer
* CreateFlywheel
* DeleteDocumentClassifier
* DeleteEndpoint
* DeleteEntityRecognizer
* DeleteFlywheel
* DeleteResourcePolicy
* DescribeDataset
* DescribeDocumentClassificationJob
* DescribeDocumentClassifier
* DescribeDominantLanguageDetectionJob
* DescribeEndpoint
* DescribeEntitiesDetectionJob
* DescribeEntityRecognizer
* DescribeEventsDetectionJob
* DescribeFlywheel
* DescribeFlywheelIteration
* DescribeKeyPhrasesDetectionJob
* DescribePiiEntitiesDetectionJob
* DescribeResourcePolicy
* DescribeSentimentDetectionJob
* DescribeTargetedSentimentDetectionJob
* DescribeTopicsDetectionJob
* DetectDominantLanguage
* DetectEntities
* DetectKeyPhrases
* DetectPiiEntities
* DetectSentiment
* DetectSyntax
* DetectTargetedSentiment
* DetectToxicContent
* ImportModel
* ListDatasets
* ListDocumentClassificationJobs
* ListDocumentClassifierSummaries
* ListDocumentClassifiers
* ListDominantLanguageDetectionJobs
* ListEndpoints
* ListEntitiesDetectionJobs
* ListEntityRecognizerSummaries
* ListEntityRecognizers
* ListEventsDetectionJobs
* ListFlywheelIterationHistory
* ListFlywheels
* ListKeyPhrasesDetectionJobs
* ListPiiEntitiesDetectionJobs
* ListSentimentDetectionJobs
* ListTagsForResource
* ListTargetedSentimentDetectionJobs
* ListTopicsDetectionJobs
* PutResourcePolicy
* StartDocumentClassificationJob
* StartDominantLanguageDetectionJob
* StartEntitiesDetectionJob
* StartEventsDetectionJob
* StartFlywheelIteration
* StartKeyPhrasesDetectionJob
* StartPiiEntitiesDetectionJob
* StartSentimentDetectionJob
* StartTargetedSentimentDetectionJob
* StartTopicsDetectionJob
* StopDominantLanguageDetectionJob
* StopEntitiesDetectionJob
* StopEventsDetectionJob
* StopKeyPhrasesDetectionJob
* StopPiiEntitiesDetectionJob
* StopSentimentDetectionJob
* StopTargetedSentimentDetectionJob
* StopTrainingDocumentClassifier
* StopTrainingEntityRecognizer
* TagResource
* UntagResource
* UpdateEndpoint
* UpdateFlywheel
