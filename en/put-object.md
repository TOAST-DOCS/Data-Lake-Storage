## PutObject

**Data & Analytics > Data Lake Storage > API Guide > Object > PutObject**

Stores an object in a bucket.

### Request

```http
PUT /{bucket}/{objectKey} HTTP/1.1

Body
```

### Request Parameter

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](https://docs.beta-nhncloud.com/en/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/).

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |
| objectKey | Path | String | Y | Object name |
| Content-Type | Header | String | N | Object content type |
| Content-Length | Header | Long | Y | Object size (bytes) |
| x-amz-storage-class | Header | String | N | Storage class |
| x-amz-meta-* | Header | String | N | Custom metadata |

### Request Body

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| Body | Binary | Y | Object data |


### Response

```http
HTTP/1.1 200 OK
ETag: String
```
