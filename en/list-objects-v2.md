## ListObjectsV2

**Data & Analytics > Data Lake Storage > API Guide > Object > ListObjectsV2**

Retrieves an object list stored in a bucket.

### Request

```http
GET /{bucket}?list-type=2&continuation-token={continuationToken}&delimiter={delimiter}&encoding-type={encodingType}&fetch-owner={fetchOwner}&max-keys={maxKeys}&prefix={prefix}&start-after={startAfter} HTTP/1.1
```

### Request Parameter

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |
| x-amz-storage-class | Header | String | N | Storage class |
| list-type | Parameter | String | Y | Fixed as 2 (V2 API identifier) |
| continuation-token | Parameter | String | N | Identifier for retrieving the next page |
| delimiter | Parameter | String | N | Key group delimiter (default: /) |
| encoding-type | Parameter | String | N | Key encoding method |
| fetch-owner | Parameter | Boolean | N | Whether to include owner information |
| max-keys | Parameter | Integer | N | Maximum number of objects to return |
| prefix | Parameter | String | N | Object name prefix |
| start-after | Parameter | String | N | Starting point for retrieval |

### Response

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<ListBucketResult>
  <Name>String</Name>
  <Prefix>String/</Prefix>
  <StartAfter>String</StartAfter>
  <ContinuationToken>String</ContinuationToken>
  <NextContinuationToken>String</NextContinuationToken>
  <KeyCount>Integer</KeyCount>
  <MaxKeys>Integer</MaxKeys>
  <Delimiter>String</Delimiter>
  <IsTruncated>Boolean</IsTruncated>
  <EncodingType>String</EncodingType>
  <Contents>
    <Key>String</Key>
    <LastModified>Timestamp</LastModified>
    <ETag>String</ETag>
    <Size>Long</Size>
    <StorageClass>String</StorageClass>
    <Owner>
      <ID>String</ID>
    </Owner>
  </Contents>
  <CommonPrefixes>
    <Prefix>String</Prefix>
  </CommonPrefixes>
</ListBucketResult>
```

| Name | Type | Description |
| --- | --- | --- |
| ListBucketResult | Object | Result of object list retrieval |
| ListBucketResult.Name | String | Bucket name |
| ListBucketResult.Prefix | String | Object name prefix specified in the request |
| ListBucketResult.StartAfter | String | Starting point for retrieval specified in the request |
| ListBucketResult.ContinuationToken | String | Page retrieval identifier used the current request |
| ListBucketResult.NextContinuationToken | String | Page retrieval identifier to use when requesting the next page |
| ListBucketResult.KeyCount | Integer | Number of objects in the response |
| ListBucketResult.MaxKeys | Integer | Maximum number of objects specified in the request |
| ListBucketResult.Delimiter | String | Key group delimiter specified in the request |
| ListBucketResult.IsTruncated | Boolean | Whether additional pages exist |
| ListBucketResult.Contents | Array | Object list |
| ListBucketResult.Contents.Key | String | Object key |
| ListBucketResult.Contents.LastModified | Timestamp | Last modified time (in ISO 8601 format) |
| ListBucketResult.Contents.ETag | String | Unique identifier of the object |
| ListBucketResult.Contents.Size | Long | Object size (bytes) |
| ListBucketResult.Contents.StorageClass | String | Storage class |
| ListBucketResult.Contents.Owner.ID | String | Owner ID (included when fetch-owner=true) |
| ListBucketResult.CommonPrefixes | Array | List of common prefixes grouped by delimiter |
| ListBucketResult.CommonPrefixes.Prefix | String | Path grouped by delimiter |
