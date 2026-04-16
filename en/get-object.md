## GetObject

**Data & Analytics > Data Lake Storage > API Guide > Object > GetObject**

Retrieves an object stored in a bucket.

### Request

```http
GET /{bucket}/{objectKey} HTTP/1.1
```

### Request Parameter

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](https://docs.beta-nhncloud.com/en/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/).

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |
| objectKey | Path | String | Y | Object name |
| Range | Header | String | N | Partial download range |
| x-amz-storage-class | Header | String | N | Storage class |

### Response

```http
HTTP/1.1 200 OK

data
```
