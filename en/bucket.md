## Bucket
**Data & Analytics > Data Lake Storage > Amazon S3-Compatible API Guide > Bucket**


## CreateBucket

Creates a bucket. The bucket name in the Data Lake Storage service is unique within a region.

### Request

```http
PUT /{bucket} HTTP/1.1

<?xml version="1.0" encoding="UTF-8"?>
<CreateBucketConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
   <LocationConstraint>KR3</LocationConstraint>
</CreateBucketConfiguration>
```

#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

#### Request Parameter

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |

#### Request Body

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| CreateBucketConfiguration | Object | Y | Information of the bucket to create |
| CreateBucketConfiguration.LocationConstraint | String | Conditional | Region code; if not specified, the region information matching the endpoint is applied |

### Response

```http
HTTP/1.1 200 OK
Location: "/{bucket}"
```


## DeleteBucket

Deletes bucket.

### Request

```http
DELETE /{bucket} HTTP/1.1
```

#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

#### Request Parameter

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |

### Response

```http
HTTP/1.1 204 No Content
```


## DeleteBucketPolicy

Deletes the policy (bucket policy) registered for a bucket.

### Request

```http
DELETE /{bucket}?policy HTTP/1.1
```

#### Request Header

For header information commonly used in Data Lake Storage APIs, see the Data Lake Storage [API Request Header Guide](api-guide-common).

#### Request Parameter

| Name | In | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |

### Response

```http
HTTP/1.1 204 No Content
```


## GetBucketAcl

Retrieves the access control list (ACL) of a bucket.

### Request

```http
GET /{bucket}?acl HTTP/1.1
```

#### Request Header

For header information commonly used in Data Lake Storage APIs, see the Data Lake Storage [API Request Header Guide](api-guide-common).

#### Request Parameter

| Name | In | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |

### Response

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<AccessControlPolicy>
  <Owner>
    <DisplayName>String</DisplayName>
    <ID>String</ID>
  </Owner>
  <AccessControlList>
    <Grant>
      <Grantee>
        <xsi:type>string</xsi:type>
        <DisplayName>String</DisplayName>
        <ID>String</ID>
      </Grantee>
      <Permission>String</Permission>
    </Grant>
  </AccessControlList>
</AccessControlPolicy>
```

#### Response Body

| Name | Type | Description |
| --- | --- | --- |
| AccessControlPolicy | Object | Root element of the bucket ACL retrieval result |
| AccessControlPolicy.Owner | Object | Bucket owner information |
| AccessControlPolicy.Owner.DisplayName | String | Bucket owner name |
| AccessControlPolicy.Owner.ID | String | Bucket owner ID |
| AccessControlPolicy.AccessControlList | Object | List of grants |
| AccessControlPolicy.AccessControlList.Grant | Array | Individual grant entries |
| AccessControlPolicy.AccessControlList.Grant.Grantee | Object | Information about the grantee |
| AccessControlPolicy.AccessControlList.Grant.Grantee.xsi:type | String | Type of the grantee. Only `CanonicalUser` is supported. |
| AccessControlPolicy.AccessControlList.Grant.Grantee.DisplayName | String | Name of the grantee |
| AccessControlPolicy.AccessControlList.Grant.Grantee.ID | String | ID of the grantee |
| AccessControlPolicy.AccessControlList.Grant.Permission | String | Permission granted. Only `FULL_CONTROL` is supported. |


## GetBucketPolicy

Retrieves the policy (bucket policy) registered for a bucket. The policy document is returned as-is in the response body in JSON format.

### Request

```http
GET /{bucket}?policy HTTP/1.1
```

#### Request Header

For header information commonly used in Data Lake Storage APIs, see the Data Lake Storage [API Request Header Guide](api-guide-common).

#### Request Parameter

| Name | In | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |

### Response

The registered policy document is returned as a JSON body. For descriptions of each field in the policy document, see [PutBucketPolicy](#putbucketpolicy).

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ProjectMembersFullObjectAccess",
      "Effect": "Allow",
      "Principal": { "NHN": "arn:nhn:cloud:iam:{appKey}:user/*" },
      "Action": ["s3:GetObject", "s3:PutObject", "s3:DeleteObject", "s3:ListBucket"],
      "Resource": ["*"]
    }
  ]
}
```

!!! tip "Note"
    If no policy is registered for the bucket, a `404 NoSuchBucketPolicy` error is returned.


## ListBuckets

Lists buckets.

### Request

```http
GET /?max-buckets=20 HTTP/1.1
```

#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

#### Request Parameter

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

#### Response Body

| Name | Type | Description |
| --- | --- | --- |
| ListAllMyBucketsResult | Object | Result of bucket list retrieval |
| ListAllMyBucketsResult.Buckets | Object | Bucket information |
| ListAllMyBucketsResult.Buckets.BucketRegion | String | Region where the bucket is located |
| ListAllMyBucketsResult.Buckets.CreationDate | Timestamp | Bucket creation time (ISO 8601) |
| ListAllMyBucketsResult.Buckets.Name | String | Bucket name |
| ListAllMyBucketsResult.Owner | Object | Bucket owner information |
| ListAllMyBucketsResult.Owner.DisplayName | String | Owner display name |
| ListAllMyBucketsResult.Owner.ID | String | Owner ID |
| ListAllMyBucketsResult.ContinuationToken | String | Continuation token for retrieving the next page (not included if this is the last page) |
| ListAllMyBucketsResult.Prefix | String | Prefix filter used in the request |


## PutBucketPolicy

Registers or replaces a policy (bucket policy) for a bucket. A bucket policy is a policy document in JSON format that defines which principal is allowed or denied access to which actions on which resources.

### Request

```http
PUT /{bucket}?policy HTTP/1.1

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ExampleStatement",
      "Effect": "Allow",
      "Principal": { "NHN": "arn:nhn:cloud:iam:{appKey}:user/{memberUuid}" },
      "Action": ["s3:GetObject", "s3:PutObject"],
      "Resource": ["*"]
    }
  ]
}
```

#### Request Header

For header information commonly used in Data Lake Storage APIs, see the Data Lake Storage [API Request Header Guide](api-guide-common).

#### Request Parameters

| Name | In | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |

#### Request Body

The request body is a JSON containing the entire policy document.

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| Version | String | Y | Policy language version. Use `2012-10-17`. |
| Id | String | N | Policy identifier |
| Statement | Array | Y | List of policy statements. Must contain at least one statement. |
| Statement.Sid | String | N | Policy statement identifier |
| Statement.Effect | String | Y | Permission application method. `Allow` or `Deny` |
| Statement.Principal | Object | Y | Principal to apply permissions to. Cannot be used together with `NotPrincipal`. |
| Statement.NotPrincipal | Object | N | Principal to exclude from permission application. Cannot be used together with `Principal`. |
| Statement.Action | Array | Y | List of actions to apply. Cannot be used together with `NotAction`. |
| Statement.NotAction | Array | N | List of actions to exclude from application. Cannot be used together with `Action`. |
| Statement.Resource | Array | Y | List of target resources for the action. Cannot be used together with `NotResource`. |
| Statement.NotResource | Array | N | List of resources to exclude from application. Cannot be used together with `Resource`. |
| Statement.Condition | Object | N | Condition under which the policy statement applies |

#### Principal

Specifies the principal to apply permissions to.

| Format | Description |
| --- | --- |
| `"*"` | All users (including anonymous users) |
| `{ "NHN": "arn:nhn:cloud:iam:{appKey}:user/{memberUuid}" }` | A specific NHN Cloud IAM user. `appKey` is a 16-character alphanumeric string, and `memberUuid` is in UUID format. |
| `{ "NHN": "arn:nhn:cloud:iam:{appKey}:user/*" }` | All users belonging to the project (appKey) |

!!! tip "Note"
    `Principal` accepts both a single string and an array of strings.

#### Action

The following actions are available for use in policies:

| Action | Description |
| --- | --- |
| s3:GetObject | Retrieve an object |
| s3:PutObject | Upload an object |
| s3:DeleteObject | Delete an object |
| s3:GetObjectAcl | Retrieve the ACL of an object |
| s3:PutObjectAcl | Set the ACL of an object |
| s3:ListBucket | Retrieve the list of objects in a bucket |
| s3:ListBucketMultipartUploads | Retrieve the list of in-progress multipart uploads |
| s3:ListMultipartUploadParts | Retrieve the list of parts in a multipart upload |
| s3:AbortMultipartUpload | Abort a multipart upload |
| s3:* | All actions, including the above |

!!! danger "Caution"
    Bucket administration actions such as creating/deleting buckets and managing bucket policies and ACLs cannot be granted through bucket policies. These permissions can only be performed by the project/bucket owner role.

#### Resource

Resources are specified in path format within the bucket, not as ARNs.

| Format | Description |
| --- | --- |
| `*` | All objects in the bucket |
| `curated/*` | All objects with the `curated/` prefix |
| `report.csv` | A specific object key |

!!! danger "Caution"
    Resources cannot start with `arn:` or `/`.

#### Request Example

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ProjectMembersFullObjectAccess",
      "Effect": "Allow",
      "Principal": { "NHN": "arn:nhn:cloud:iam:{appKey}:user/*" },
      "Action": ["s3:GetObject", "s3:PutObject", "s3:DeleteObject", "s3:ListBucket"],
      "Resource": ["*"]
    }
  ]
}
```

### Response

```http
HTTP/1.1 204 No Content
```
