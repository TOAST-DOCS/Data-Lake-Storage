## GetBucketPolicy

**Data & Analytics > Data Lake Storage > API 가이드 > Bucket > GetBucketPolicy**

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

등록된 정책 문서가 JSON 본문으로 반환됩니다. 정책 문서의 각 필드에 대한 설명은 [PutBucketPolicy](put-bucket-policy)를 참고하세요.

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
