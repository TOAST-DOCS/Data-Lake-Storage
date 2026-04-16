## ListBuckets

**Data & Analytics > Data Lake Storage > API Guide > Bucket > ListBuckets**

Lists buckets.

### Request

```http
GET /?max-buckets=20 HTTP/1.1
```

### Request Parameter

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](https://docs.beta-nhncloud.com/en/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/).

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| continuation-token | Parameter | String | N | Continuation token for retrieving the next page |
| max-buckets | Parameter | Integer | N | Maximum number of buckets to return |
| prefix | Parameter | String | N | Prefix for filtering bucket names |

### Response

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<ListAllMyBucketsResult>
  <Buckets>
    <Bucket>
      <BucketRegion>string</BucketRegion>
      <CreationDate>timestamp</CreationDate>
      <Name>string</Name>
    </Bucket>
  </Buckets>
  <Owner>
    <DisplayName>string</DisplayName>
    <ID>string</ID>
  </Owner>
  <ContinuationToken>string</ContinuationToken>
  <Prefix></Prefix>
</ListAllMyBucketsResult>
```

| Name | Type | Description |
| --- | --- | --- |
| ListAllMyBuckets | Object | Result of bucket list retrieval |
| ListAllMyBuckets.Buckets | Object | Bucket information |
| ListAllMyBuckets.Buckets.BucketRegion | String | Region where the bucket is located |
| ListAllMyBuckets.Buckets.CreationDate | Timestamp | Bucket creation time (ISO 8601) |
| ListAllMyBuckets.Buckets.Name | String | Bucket name |
| ListAllMyBuckets.Owner | Object | Bucket owner information |
| ListAllMyBuckets.Owner.DisplayName | String | Owner display name |
| ListAllMyBuckets.Owner.ID | String | Owner ID |
| ListAllMyBuckets.ContinuationToken | String | Continuation token for retrieving the next page (not included if this is the last page) |
| ListAllMyBuckets.Prefix | String | Prefix filter used in the request |
