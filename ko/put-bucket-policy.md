## PutBucketPolicy

**Data & Analytics > Data Lake Storage > API 가이드 > Bucket > PutBucketPolicy**

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
