<!-- pre-align:aligned sig=b6666f81ba17 -->

<a id="bucket"></a>
## Bucket { #bucket }
**Data & Analytics > Data Lake Storage > Amazon S3-Compatible API Guide > Bucket**


<a id="createbucket"></a>
## CreateBucket { #createbucket }

Creates a bucket. The bucket name in the Data Lake Storage service is unique within a region.

<a id="request"></a>
### Request { #request }

```http
PUT /{bucket} HTTP/1.1

<?xml version="1.0" encoding="UTF-8"?>
<CreateBucketConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
   <LocationConstraint>KR3</LocationConstraint>
</CreateBucketConfiguration>
```

<a id="request-header"></a>
#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

<a id="request-parameter"></a>
#### Request Parameter

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |

<a id="request-body"></a>
#### Request Body

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| CreateBucketConfiguration | Object | Y | Information of the bucket to create |
| CreateBucketConfiguration.LocationConstraint | String | Conditional | Region code; if not specified, the region information matching the endpoint is applied |

<a id="response"></a>
### Response { #response }

```http
HTTP/1.1 200 OK
Location: "/{bucket}"
```


<a id="deletebucket"></a>
## DeleteBucket { #deletebucket }

Deletes bucket.

<a id="deletebucket-request"></a>
### Request { #deletebucket-request }

```http
DELETE /{bucket} HTTP/1.1
```

<a id="deletebucket-request-request-header"></a>
#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

<a id="deletebucket-request-request-parameter"></a>
#### Request Parameter

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |

<a id="deletebucket-response"></a>
### Response { #deletebucket-response }

```http
HTTP/1.1 204 No Content
```


<a id="deletebucketpolicy"></a>
## DeleteBucketPolicy { #deletebucketpolicy }

Deletes the policy (bucket policy) registered for a bucket.

<a id="deletebucketpolicy-request"></a>
### Request { #deletebucketpolicy-request }

```http
DELETE /{bucket}?policy HTTP/1.1
```

<a id="deletebucketpolicy-request-request-header"></a>
#### Request Header

For header information commonly used in Data Lake Storage APIs, see the Data Lake Storage [API Request Header Guide](api-guide-common).

<a id="deletebucketpolicy-request-request-parameter"></a>
#### Request Parameter

| Name | In | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |

<a id="deletebucketpolicy-response"></a>
### Response { #deletebucketpolicy-response }

```http
HTTP/1.1 204 No Content
```


<a id="getbucketacl"></a>
## GetBucketAcl { #getbucketacl }

Retrieves the access control list (ACL) of a bucket.

<a id="getbucketacl-request"></a>
### Request { #getbucketacl-request }

```http
GET /{bucket}?acl HTTP/1.1
```

<a id="getbucketacl-request-request-header"></a>
#### Request Header

For header information commonly used in Data Lake Storage APIs, see the Data Lake Storage [API Request Header Guide](api-guide-common).

<a id="getbucketacl-request-request-parameter"></a>
#### Request Parameter

| Name | In | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |

<a id="getbucketacl-response"></a>
### Response { #getbucketacl-response }

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

<a id="getbucketacl-response-response-body"></a>
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


<a id="getbucketpolicy"></a>
## GetBucketPolicy { #getbucketpolicy }

Retrieves the policy (bucket policy) registered for a bucket. The policy document is returned as-is in the response body in JSON format.

<a id="getbucketpolicy-request"></a>
### Request { #getbucketpolicy-request }

```http
GET /{bucket}?policy HTTP/1.1
```

<a id="getbucketpolicy-request-request-header"></a>
#### Request Header

For header information commonly used in Data Lake Storage APIs, see the Data Lake Storage [API Request Header Guide](api-guide-common).

<a id="getbucketpolicy-request-request-parameter"></a>
#### Request Parameter

| Name | In | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |

<a id="getbucketpolicy-response"></a>
### Response { #getbucketpolicy-response }

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


<a id="listbuckets"></a>
## ListBuckets { #listbuckets }

Lists buckets.

<a id="listbuckets-request"></a>
### Request { #listbuckets-request }

```http
GET /?max-buckets=20 HTTP/1.1
```

<a id="listbuckets-request-request-header"></a>
#### Request Header

For the common header information for Data Lake Storage API, see the Data Lake Storage [API Request Header Guide](api-guide-common).

<a id="listbuckets-request-request-parameter"></a>
#### Request Parameter

| Name | Category | Type | Required | Description |
| --- | --- | --- | --- | --- |
| continuation-token | Parameter | String | N | Continuation token for retrieving the next page |
| max-buckets | Parameter | Integer | N | Maximum number of buckets to return |
| prefix | Parameter | String | N | Prefix for filtering bucket names |

<a id="listbuckets-response"></a>
### Response { #listbuckets-response }

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

<a id="listbuckets-response-response-body"></a>
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


<a id="putbucketpolicy"></a>
## PutBucketPolicy { #putbucketpolicy }

Registers or replaces a policy (bucket policy) for a bucket. A bucket policy is a policy document in JSON format that defines which principal is allowed or denied access to which actions on which resources.

<a id="putbucketpolicy-request"></a>
### Request { #putbucketpolicy-request }

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

<a id="putbucketpolicy-request-request-header"></a>
#### Request Header

For header information commonly used in Data Lake Storage APIs, see the Data Lake Storage [API Request Header Guide](api-guide-common).

<a id="putbucketpolicy-request-request-parameters"></a>
#### Request Parameters

| Name | In | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |

<a id="putbucketpolicy-request-request-body"></a>
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

<a id="putbucketpolicy-request-principal"></a>
#### Principal

Specifies the principal to apply permissions to.

| Format | Description |
| --- | --- |
| `"*"` | All users (including anonymous users) |
| `{ "NHN": "arn:nhn:cloud:iam:{appKey}:user/{memberUuid}" }` | A specific NHN Cloud IAM user. `appKey` is a 16-character alphanumeric string, and `memberUuid` is in UUID format. |
| `{ "NHN": "arn:nhn:cloud:iam:{appKey}:user/*" }` | All users belonging to the project (appKey) |

!!! tip "Note"
    `Principal` accepts both a single string and an array of strings.

<a id="putbucketpolicy-request-action"></a>
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

<a id="putbucketpolicy-request-resource"></a>
#### Resource

Resources are specified in path format within the bucket, not as ARNs.

| Format | Description |
| --- | --- |
| `*` | All objects in the bucket |
| `curated/*` | All objects with the `curated/` prefix |
| `report.csv` | A specific object key |

!!! danger "Caution"
    Resources cannot start with `arn:` or `/`.

<a id="putbucketpolicy-request-request-example"></a>
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

<a id="putbucketpolicy-response"></a>
### Response { #putbucketpolicy-response }

```http
HTTP/1.1 204 No Content
```
