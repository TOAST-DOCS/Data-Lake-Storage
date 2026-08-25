<!-- pre-align:aligned sig=bfea2e9c91d3 -->

<a id="data-lake-storage-overview"></a>
## Data Lake Storage Overview { #data-lake-storage-overview }

**Data & Analytics > Data Lake Storage > Overview**

Data Lake Storage is an object storage service for analytics provided by NHN Cloud.

It provides the scalability and flexibility to store data in any structure without pre-configuring capacity — from unstructured raw data with unpredictable formats and sizes, to structured data that has been processed and refined.

Built on high compatibility with the AWS S3 API, you can use the SDKs, CLIs, and third-party tools from your existing analytics ecosystem as-is, enabling a predictable data access environment without additional migration costs.

!!! danger "Caution"
    If you disable the service, all data stored in the storage will be deleted and cannot be recovered.


<a id="main-features"></a>
## Main features { #main-features }
* Flexible scaling
    * Supports horizontal scaling so you can store data without worrying about storage capacity.
* Storage class
    * Allows you to choose from various storage options based on data access frequency and cost efficiency (Upcoming).
* AWS S3 compatible API
    * Provides high-level compatibility with AWS S3 API, allowing you to use existing S3 SDKs, CLIs, and third-party tools as-is.

<a id="how-it-works"></a>
## How it works { #how-it-works }
![Data Lake Storage 동작 방식](../static/images/15_data&analytics_data-lake-storage_img_en.png)

<a id="glossary"></a>
## Glossary { #glossary }
| Terms | Description |
| --- | --- |
| Object | A file-based storage element consisting of data and metadata |
| Bucket | The top-level storage space for storing and managing objects |
| API credentials | Authentication information (such as Access Key) used to verify authentication and authorization when accessing the service |
| Storage class | A storage tier categorized by data access frequency and cost |

