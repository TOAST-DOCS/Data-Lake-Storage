## DeleteBucket

**Data & Analytics > Data Lake Storage > API Guide > Bucket > DeleteBucket**

Deletes bucket.

### Request

```http
DELETE /{bucket} HTTP/1.1
```

### Request Parameter

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](https://docs.beta-nhncloud.com/en/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/).

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |

### Response

```http
HTTP/1.1 204 No Content
```
