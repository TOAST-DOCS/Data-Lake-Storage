## Bucket
**Data & Analytics > Data Lake Storage > Amazon S3 호환 API 가이드 > Bucket**


## CreateBucket

バケットを作成します。Data Lake Storageサービスのバケット名はリージョン内で一意です。

### リクエスト

```http
PUT /{bucket} HTTP/1.1

<?xml version="1.0" encoding="UTF-8"?>
<CreateBucketConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
   <LocationConstraint>KR3</LocationConstraint>
</CreateBucketConfiguration>
```

#### 요청 헤더

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |

#### リクエストボディ

| 名前 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| CreateBucketConfiguration | Object | Y | 作成するバケット情報 |
| CreateBucketConfiguration.LocationConstraint | String | Conditional | リージョンコード。未入力時はエンドポイントに合ったリージョン情報を適用 |

### レスポンス

```http
HTTP/1.1 200 OK
Location: "/{bucket}"
```


## DeleteBucket

バケットを削除します。

### リクエスト

```http
DELETE /{bucket} HTTP/1.1
```

#### 요청 헤더

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |

### レスポンス

```http
HTTP/1.1 204 No Content
```


## DeleteBucketPolicy

버킷에 등록된 정책(Bucket Policy)을 삭제합니다.

### 요청

```http
DELETE /{bucket}?policy HTTP/1.1
```

#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

#### 요청 파라미터

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |

### 응답

```http
HTTP/1.1 204 No Content
```


## GetBucketAcl

バケットのアクセス制御リスト(ACL)を照会します。

### リクエスト

```http
GET /{bucket}?acl HTTP/1.1
```

#### 요청 헤더

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |

### レスポンス

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

#### 응답 본문

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| AccessControlPolicy | Object | バケットACL照会結果のルート要素 |
| AccessControlPolicy.Owner | Object | バケット所有者情報 |
| AccessControlPolicy.Owner.DisplayName | String | バケット所有者名 |
| AccessControlPolicy.Owner.ID | String | バケット所有者ID |
| AccessControlPolicy.AccessControlList | Object | 権限付与一覧 |
| AccessControlPolicy.AccessControlList.Grant | Array | 個別の権限付与項目 |
| AccessControlPolicy.AccessControlList.Grant.Grantee | Object | 権限を付与された対象の情報 |
| AccessControlPolicy.AccessControlList.Grant.Grantee.xsi:type | String | 権限を付与された対象のタイプ。`CanonicalUser`のみサポート |
| AccessControlPolicy.AccessControlList.Grant.Grantee.DisplayName | String | 権限を付与された対象名 |
| AccessControlPolicy.AccessControlList.Grant.Grantee.ID | String | 権限を付与された対象ID |
| AccessControlPolicy.AccessControlList.Grant.Permission | String | 付与された権限。`FULL_CONTROL`のみサポート |


## GetBucketPolicy

버킷에 등록된 정책(Bucket Policy)을 조회합니다. 정책 문서가 JSON 형식의 응답 본문으로 그대로 반환됩니다.

### 요청

```http
GET /{bucket}?policy HTTP/1.1
```

#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

#### 요청 파라미터

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

バケット一覧を照会します。

### リクエスト

```http
GET /?max-buckets=20 HTTP/1.1
```

#### 요청 헤더

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| continuation-token | Parameter | String | N | 次のページを照会するための連続トークン |
| max-buckets | Parameter | Integer | N | 返却する最大バケット数 |
| prefix | Parameter | String | N | バケット名フィルタリングのプレフィックス |

### レスポンス

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

#### 응답 본문

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| ListAllMyBucketsResult | Object | バケット一覧照会結果 |
| ListAllMyBucketsResult.Buckets | Object | バケット情報 |
| ListAllMyBucketsResult.Buckets.BucketRegion | String | バケットが配置されているリージョン |
| ListAllMyBucketsResult.Buckets.CreationDate | Timestamp | バケット作成日時 (ISO 8601) |
| ListAllMyBucketsResult.Buckets.Name | String | バケット名 |
| ListAllMyBucketsResult.Owner | Object | バケットの所有者情報 |
| ListAllMyBucketsResult.Owner.DisplayName | String | 所有者の表示名 |
| ListAllMyBucketsResult.Owner.ID | String | 所有者ID |
| ListAllMyBucketsResult.ContinuationToken | String | 次のページ照会用の連続トークン (最後のページの場合は未記載) |
| ListAllMyBucketsResult.Prefix | String | リクエストに使用されたプレフィックスフィルタ |


## PutBucketPolicy

버킷에 정책(Bucket Policy)을 등록하거나 기존 정책을 교체합니다. 버킷 정책은 JSON 형식의 정책 문서로, 어떤 주체(Principal)가 어떤 리소스(Resource)의 어떤 작업(Action)을 허용 또는 거부할지 정의합니다.

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

#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

#### 요청 파라미터

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |

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

!!! danger "주의"
    버킷 생성/삭제, 버킷 정책 및 ACL 관리 등 버킷 관리(Bucket Administration) 작업은 버킷 정책으로 부여할 수 없습니다. 해당 권한은 프로젝트/버킷 소유자 역할로만 수행할 수 있습니다.

#### Resource

리소스는 ARN이 아닌 버킷 내 경로 형식으로 지정합니다.

| 형식 | 설명 |
| --- | --- |
| `*` | 버킷 내 모든 객체 |
| `curated/*` | `curated/` 접두사를 가진 모든 객체 |
| `report.csv` | 특정 객체 키 |

!!! danger "주의"
    리소스는 `arn:`으로 시작하거나 `/`로 시작할 수 없습니다.

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

### 응답

```http
HTTP/1.1 204 No Content
```
