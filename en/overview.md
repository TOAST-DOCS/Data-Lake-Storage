## Data Lake Storage Overview

**Data & Analytics > Data Lake Storage > Overview**

Data Lake Storage is an object storage service for analytics provided by NHN Cloud.

It provides the scalability and flexibility to store data in any structure without pre-configuring capacity — from unstructured raw data with unpredictable formats and sizes, to structured data that has been processed and refined.

Built on high compatibility with the AWS S3 API, you can use the SDKs, CLIs, and third-party tools from your existing analytics ecosystem as-is, enabling a predictable data access environment without additional migration costs.

!!! danger "Caution"
    If you disable the service, all data stored in the storage will be deleted and cannot be recovered.


## Main features
* Flexible scaling
    * Supports horizontal scaling so you can store data without worrying about storage capacity.
* Storage class
    * Allows you to choose from a variety of storage options based on data access frequency and cost efficiency. (Upcoming)
* AWS S3 compatible API
    * Provides high-level compatibility with AWS S3 API, allowing you to use existing S3 SDKs, CLIs, and third-party tools as-is.

## How it works
<img src="https://static.toastoven.net/prod_data_lake_storage/15_data&analytics_data-lake-storage_img_en.png" />


## Glossary
| Terms | Description |
| --- | --- |
| Object | A file-based storage element consisting of data and metadata |
| Bucket | The top-level storage space for storing and managing objects |
| API credentials | Authentication information (such as Access Key) used to verify authentication and authorization when accessing the service |
| Storage class | A storage tier categorized by data access frequency and cost |

