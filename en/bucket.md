## Bucket
**Data & Analytics > Data Lake Storage > Amazon S3 호환 API 가이드 > Bucket**


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

버킷에 등록된 정책(Bucket Policy)을 삭제합니다.

### 요청

```http
DELETE /{bucket}?policy HTTP/1.1
```

### 요청 파라미터

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |

### 응답

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

버킷에 등록된 정책(Bucket Policy)을 조회합니다. 정책 문서가 JSON 형식의 응답 본문으로 그대로 반환됩니다.

### 요청

```http
GET /{bucket}?policy HTTP/1.1
```

### 요청 파라미터

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |

### 응답

등록된 정책 문서가 JSON 본문으로 반환됩니다. 정책 문서의 각 필드에 대한 설명은 [PutBucketPolicy](#putbucketpolicy)를 참고하세요.

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

!!! tip "알아두기"
    버킷에 정책이 등록되어 있지 않으면 `404 NoSuchBucketPolicy` 오류가 반환됩니다.


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


## PutBucketPolicy

버킷에 정책(Bucket Policy)을 등록하거나 기존 정책을 교체합니다. 버킷 정책은 JSON 형식의 정책 문서로, 어떤 주체(Principal)가 어떤 작업(Action)을 어떤 리소스(Resource)에 대해 허용 또는 거부할지 정의합니다.

### 요청

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

### 요청 파라미터

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |

### 요청 본문

요청 본문은 정책 문서 전체를 담은 JSON입니다.

| 이름 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- |
| Version | String | Y | 정책 언어 버전. `2012-10-17`을 사용합니다. |
| Id | String | N | 정책 식별자 |
| Statement | Array | Y | 정책 구문 목록. 최소 1개 이상이어야 합니다. |
| Statement.Sid | String | N | 정책 구문 식별자 |
| Statement.Effect | String | Y | 권한 적용 방식. `Allow` 또는 `Deny` |
| Statement.Principal | Object | Y | 권한을 적용할 주체. `NotPrincipal`과 함께 사용할 수 없습니다. |
| Statement.NotPrincipal | Object | N | 권한 적용에서 제외할 주체. `Principal`과 함께 사용할 수 없습니다. |
| Statement.Action | Array | Y | 적용할 작업 목록. `NotAction`과 함께 사용할 수 없습니다. |
| Statement.NotAction | Array | N | 적용에서 제외할 작업 목록. `Action`과 함께 사용할 수 없습니다. |
| Statement.Resource | Array | Y | 작업 대상 리소스 목록. `NotResource`와 함께 사용할 수 없습니다. |
| Statement.NotResource | Array | N | 적용에서 제외할 리소스 목록. `Resource`와 함께 사용할 수 없습니다. |
| Statement.Condition | Object | N | 정책 구문이 적용되는 조건 |

#### Principal

권한을 적용할 주체를 지정합니다.

| 형식 | 설명 |
| --- | --- |
| `"*"` | 모든 사용자(익명 포함) |
| `{ "NHN": "arn:nhn:cloud:iam:{appKey}:user/{memberUuid}" }` | 특정 NHN Cloud IAM 사용자. `appKey`는 16자리 영숫자, `memberUuid`는 UUID 형식입니다. |
| `{ "NHN": "arn:nhn:cloud:iam:{appKey}:user/*" }` | 프로젝트(appKey)에 속한 모든 사용자 |

!!! tip "알아두기"
    `Principal`에는 단일 문자열 또는 문자열 배열을 모두 사용할 수 있습니다.

#### Action

정책에서 사용할 수 있는 작업은 다음과 같습니다.

| 작업 | 설명 |
| --- | --- |
| s3:GetObject | 객체 조회 |
| s3:PutObject | 객체 업로드 |
| s3:DeleteObject | 객체 삭제 |
| s3:GetObjectAcl | 객체 ACL 조회 |
| s3:PutObjectAcl | 객체 ACL 설정 |
| s3:ListBucket | 버킷 내 객체 목록 조회 |
| s3:ListBucketMultipartUploads | 진행 중인 멀티파트 업로드 목록 조회 |
| s3:ListMultipartUploadParts | 멀티파트 업로드의 파트 목록 조회 |
| s3:AbortMultipartUpload | 멀티파트 업로드 중단 |
| s3:* | 위 작업을 포함한 모든 작업 |

!!! warning "주의"
    버킷 생성/삭제, 버킷 정책 및 ACL 관리 등 버킷 관리(Bucket Administration) 작업은 버킷 정책으로 부여할 수 없습니다. 해당 권한은 프로젝트/버킷 소유자 역할로만 수행할 수 있습니다.

#### Resource

리소스는 ARN이 아닌 버킷 내 경로 형식으로 지정합니다.

| 형식 | 설명 |
| --- | --- |
| `*` | 버킷 내 모든 객체 |
| `curated/*` | `curated/` 접두사를 가진 모든 객체 |
| `report.csv` | 특정 객체 키 |

!!! warning "주의"
    리소스는 `arn:`으로 시작하거나 `/`로 시작할 수 없습니다.

### 요청 예시

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

### 응답

```http
HTTP/1.1 204 No Content
```
