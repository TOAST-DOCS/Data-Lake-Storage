## DeleteObject

**Data & Analytics > Data Lake Storage > API Guide > Object > DeleteObject**

Deletes objects stored in the bucket.

### Request

```http
DELETE /{bucket}/{objectKey} HTTP/1.1
```

### Request Parameter

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |
| objectKey | Path | String | Y | Object name |

### Response

```http
HTTP/1.1 204 No Content
```
