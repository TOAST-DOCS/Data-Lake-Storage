## GetBucketAcl

**Data & Analytics > Data Lake Storage > API 가이드 > Bucket > GetBucketAcl**

버킷의 액세스 제어 목록(ACL)을 조회합니다.

### 요청

```http
GET /{bucket}?acl HTTP/1.1
```

#### 요청 헤더

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

#### 요청 파라미터

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |

### 응답

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
