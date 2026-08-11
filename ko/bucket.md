<!-- pre-align:aligned sig=b6666f81ba17 -->

<a id="bucket"></a>
## Bucket { #bucket }
**Data & Analytics > Data Lake Storage > Amazon S3 호환 API 가이드 > Bucket**


<a id="createbucket"></a>
## CreateBucket { #createbucket }

버킷을 생성합니다. Data Lake Storage 서비스의 버킷 이름은 리전에서 고유합니다.

<a id="request"></a>
### 요청 { #request }

```http
PUT /{bucket} HTTP/1.1

<?xml version="1.0" encoding="UTF-8"?>
<CreateBucketConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
   <LocationConstraint>KR3</LocationConstraint>
</CreateBucketConfiguration>
```

<a id="request-header"></a>
#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

<a id="request-parameter"></a>
#### 요청 파라미터

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |

<a id="request-body"></a>
#### 요청 본문

| 이름 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- |
| CreateBucketConfiguration | Object | Y | 생성할 버킷 정보 |
| CreateBucketConfiguration.LocationConstraint | String | Conditional | 리전 코드, 미입력 시 엔드포인트에 맞는 리전 정보 적용 |

<a id="response"></a>
### 응답 { #response }

```http
HTTP/1.1 200 OK
Location: "/{bucket}"
```


<a id="deletebucket"></a>
## DeleteBucket { #deletebucket }

버킷을 삭제합니다.

<a id="deletebucket-request"></a>
### 요청 { #deletebucket-request }

```http
DELETE /{bucket} HTTP/1.1
```

<a id="deletebucket-request-request-header"></a>
#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

<a id="deletebucket-request-request-parameter"></a>
#### 요청 파라미터

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |

<a id="deletebucket-response"></a>
### 응답 { #deletebucket-response }

```http
HTTP/1.1 204 No Content
```


<a id="deletebucketpolicy"></a>
## DeleteBucketPolicy { #deletebucketpolicy }

버킷에 등록된 정책(Bucket Policy)을 삭제합니다.

<a id="deletebucketpolicy-request"></a>
### 요청 { #deletebucketpolicy-request }

```http
DELETE /{bucket}?policy HTTP/1.1
```

<a id="deletebucketpolicy-request-request-header"></a>
#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

<a id="deletebucketpolicy-request-request-parameter"></a>
#### 요청 파라미터

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |

<a id="deletebucketpolicy-response"></a>
### 응답 { #deletebucketpolicy-response }

```http
HTTP/1.1 204 No Content
```


<a id="getbucketacl"></a>
## GetBucketAcl { #getbucketacl }

버킷의 액세스 제어 목록(ACL)을 조회합니다.

<a id="getbucketacl-request"></a>
### 요청 { #getbucketacl-request }

```http
GET /{bucket}?acl HTTP/1.1
```

<a id="getbucketacl-request-request-header"></a>
#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

<a id="getbucketacl-request-request-parameter"></a>
#### 요청 파라미터

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |

<a id="getbucketacl-response"></a>
### 응답 { #getbucketacl-response }

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
#### 응답 본문

| 이름 | 타입 | 설명 |
| --- | --- | --- |
| AccessControlPolicy | Object | 버킷 ACL 조회 결과의 루트 요소 |
| AccessControlPolicy.Owner | Object | 버킷 소유자 정보 |
| AccessControlPolicy.Owner.DisplayName | String | 버킷 소유자 이름 |
| AccessControlPolicy.Owner.ID | String | 버킷 소유자 ID |
| AccessControlPolicy.AccessControlList | Object | 권한 부여 목록 |
| AccessControlPolicy.AccessControlList.Grant | Array | 개별 권한 부여 항목 |
| AccessControlPolicy.AccessControlList.Grant.Grantee | Object | 권한을 부여받은 대상 정보 |
| AccessControlPolicy.AccessControlList.Grant.Grantee.xsi:type | String | 권한을 부여받은 대상 유형. `CanonicalUser`만 지원 |
| AccessControlPolicy.AccessControlList.Grant.Grantee.DisplayName | String | 권한을 부여받은 대상 이름 |
| AccessControlPolicy.AccessControlList.Grant.Grantee.ID | String | 권한을 부여받은 대상 ID |
| AccessControlPolicy.AccessControlList.Grant.Permission | String | 부여된 권한. `FULL_CONTROL`만 지원 |


<a id="getbucketpolicy"></a>
## GetBucketPolicy { #getbucketpolicy }

버킷에 등록된 정책(Bucket Policy)을 조회합니다. 정책 문서가 JSON 형식의 응답 본문으로 그대로 반환됩니다.

<a id="getbucketpolicy-request"></a>
### 요청 { #getbucketpolicy-request }

```http
GET /{bucket}?policy HTTP/1.1
```

<a id="getbucketpolicy-request-request-header"></a>
#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

<a id="getbucketpolicy-request-request-parameter"></a>
#### 요청 파라미터

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |

<a id="getbucketpolicy-response"></a>
### 응답 { #getbucketpolicy-response }

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


<a id="listbuckets"></a>
## ListBuckets { #listbuckets }

버킷 목록을 조회합니다.

<a id="listbuckets-request"></a>
### 요청 { #listbuckets-request }

```http
GET /?max-buckets=20 HTTP/1.1
```

<a id="listbuckets-request-request-header"></a>
#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

<a id="listbuckets-request-request-parameter"></a>
#### 요청 파라미터

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| continuation-token | Parameter | String | N | 다음 페이지 조회를 위한 연속 토큰 |
| max-buckets | Parameter | Integer | N | 반환할 최대 버킷 수 |
| prefix | Parameter | String | N | 버킷 이름 필터링 접두어 |

<a id="listbuckets-response"></a>
### 응답 { #listbuckets-response }

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
#### 응답 본문

| 이름 | 타입 | 설명 |
| --- | --- | --- |
| ListAllMyBucketsResult | Object | 버킷 목록 조회 결과 |
| ListAllMyBucketsResult.Buckets | Object | 버킷 정보 |
| ListAllMyBucketsResult.Buckets.BucketRegion | String | 버킷이 위치한 리전 |
| ListAllMyBucketsResult.Buckets.CreationDate | Timestamp | 버킷 생성 일시 (ISO 8601) |
| ListAllMyBucketsResult.Buckets.Name | String | 버킷 이름 |
| ListAllMyBucketsResult.Owner | Object | 버킷 소유자 정보 |
| ListAllMyBucketsResult.Owner.DisplayName | String | 소유자 표시 이름 |
| ListAllMyBucketsResult.Owner.ID | String | 소유자 ID |
| ListAllMyBucketsResult.ContinuationToken | String | 다음 페이지 조회용 연속 토큰 (마지막 페이지면 미포함) |
| ListAllMyBucketsResult.Prefix | String | 요청에 사용된 접두어 필터 |


<a id="putbucketpolicy"></a>
## PutBucketPolicy { #putbucketpolicy }

버킷에 정책(Bucket Policy)을 등록하거나 기존 정책을 교체합니다. 버킷 정책은 JSON 형식의 정책 문서로, 어떤 주체(Principal)가 어떤 리소스(Resource)의 어떤 작업(Action)을 허용 또는 거부할지 정의합니다.

<a id="putbucketpolicy-request"></a>
### 요청 { #putbucketpolicy-request }

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
#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

<a id="putbucketpolicy-request-request-parameters"></a>
#### 요청 파라미터

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |

<a id="putbucketpolicy-request-request-body"></a>
#### 요청 본문

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

<a id="putbucketpolicy-request-principal"></a>
#### Principal

권한을 적용할 주체를 지정합니다.

| 형식 | 설명 |
| --- | --- |
| `"*"` | 모든 사용자(익명 포함) |
| `{ "NHN": "arn:nhn:cloud:iam:{appKey}:user/{memberUuid}" }` | 특정 NHN Cloud IAM 사용자. `appKey`는 16자리 영숫자, `memberUuid`는 UUID 형식입니다. |
| `{ "NHN": "arn:nhn:cloud:iam:{appKey}:user/*" }` | 프로젝트(appKey)에 속한 모든 사용자 |

!!! tip "알아두기"
    `Principal`에는 단일 문자열 또는 문자열 배열을 모두 사용할 수 있습니다.

<a id="putbucketpolicy-request-action"></a>
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

!!! danger "주의"
    버킷 생성/삭제, 버킷 정책 및 ACL 관리 등 버킷 관리(Bucket Administration) 작업은 버킷 정책으로 부여할 수 없습니다. 해당 권한은 프로젝트/버킷 소유자 역할로만 수행할 수 있습니다.

<a id="putbucketpolicy-request-resource"></a>
#### Resource

리소스는 ARN이 아닌 버킷 내 경로 형식으로 지정합니다.

| 형식 | 설명 |
| --- | --- |
| `*` | 버킷 내 모든 객체 |
| `curated/*` | `curated/` 접두사를 가진 모든 객체 |
| `report.csv` | 특정 객체 키 |

!!! danger "주의"
    리소스는 `arn:`으로 시작하거나 `/`로 시작할 수 없습니다.

<a id="putbucketpolicy-request-request-example"></a>
#### 요청 예시

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
### 응답 { #putbucketpolicy-response }

```http
HTTP/1.1 204 No Content
```
