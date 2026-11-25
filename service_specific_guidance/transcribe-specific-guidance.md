# Service-specific guidance: Amazon Transcribe


This document outlines service-specific guidance for implementing a data perimeter for Amazon Transcribe.

Amazon Transcribe is an automatic speech recognition service that uses machine learning models to convert audio to text. You can use it as a standalone transcription service or to add speech-to-text capabilities to any application, transcribing media in real time (streaming) or media files stored in an Amazon S3 bucket (batch).

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

* CreateCallAnalyticsCategory
* CreateLanguageModel
* CreateMedicalVocabulary
* CreateVocabulary
* CreateVocabularyFilter
* DeleteCallAnalyticsCategory
* DeleteCallAnalyticsJob
* DeleteLanguageModel
* DeleteMedicalScribeJob
* DeleteMedicalTranscriptionJob
* DeleteMedicalVocabulary
* DeleteTranscriptionJob
* DeleteVocabulary
* DeleteVocabularyFilter
* DescribeLanguageModel
* GetCallAnalyticsCategory
* GetCallAnalyticsJob
* GetMedicalScribeJob
* GetMedicalTranscriptionJob
* GetMedicalVocabulary
* GetTranscriptionJob
* GetVocabulary
* GetVocabularyFilter
* ListCallAnalyticsCategories
* ListCallAnalyticsJobs
* ListLanguageModels
* ListMedicalScribeJobs
* ListMedicalTranscriptionJobs
* ListMedicalVocabularies
* ListTagsForResource
* ListTranscriptionJobs
* ListVocabularies
* ListVocabularyFilters
* StartCallAnalyticsJob
* StartMedicalScribeJob
* StartMedicalTranscriptionJob
* StartTranscriptionJob
* TagResource
* UntagResource
* UpdateCallAnalyticsCategory
* UpdateMedicalVocabulary
* UpdateVocabulary
* UpdateVocabularyFilter
