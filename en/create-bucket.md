## CreateBucket

**Data & Analytics > Data Lake Storage > API Guide > Bucket > CreateBucket**

Creates a bucket. The bucket name in the Data Lake Storage service is unique within a region.

### Request

```http
PUT /{bucket} HTTP/1.1

<?xml version="1.0" encoding="UTF-8"?>
<CreateBucketConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
   <LocationConstraint>KR3</LocationConstraint>
</CreateBucketConfiguration>
```

### Request Parameter

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](https://docs.beta-nhncloud.com/en/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/).

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |

### Request Body

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| CreateBucketConfiguration | Object | Y | Information of the bucket to create |
| CreateBucketConfiguration.LocationConstraint | String | Conditional | Region code; if not specified, the region information matching the endpoint is applied |

### Response

```http
HTTP/1.1 200 OK
Location: "/{bucket}"
```
