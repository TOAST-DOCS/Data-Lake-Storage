## DeleteObjects

**Data & Analytics > Data Lake Storage > API 가이드 > Object > DeleteObjects**

하나의 요청으로 여러 객체를 삭제합니다. 한 번의 요청에 최대 1,000개의 객체 키를 지정할 수 있습니다.

### 요청

```http
POST /{bucket}?delete HTTP/1.1

<?xml version="1.0" encoding="UTF-8"?>
<Delete>
  <Object>
    <ETag>string</ETag>
    <Key>string</Key>
    <LastModifiedTime>timestamp</LastModifiedTime>
    <Size>long</Size>
  </Object>
  ...
  <Quiet>Boolean</Quiet>
</Delete>
```

### 요청 파라미터

Data Lake Storage API에서 공통으로 사용하는 헤더 정보는 Data Lake Storage [API 요청 헤더 가이드](api-guide-common)를 참고하세요.

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |
| Content-MD5 | Header | String | Y | 요청 본문의 MD5 해시값 (전송 중 변조 검증용) |

### 요청 본문

| 이름 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- |
| Delete | Object | Y | 요청의 루트 요소 |
| Delete.Object | Array | Y | 삭제할 객체 목록. 최대 1,000개 |
| Delete.Object.ETag | String | N | 객체의 ETag 값. 지정한 경우 ETag가 일치하는 객체만 삭제합니다. |
| Delete.Object.Key | String | Y | 삭제할 객체 키 |
| Delete.Object.LastModifiedTime | String | N | 객체의 최종 수정 시간. 지정한 경우 해당 시간과 일치하는 객체만 삭제합니다. |
| Delete.Object.Size | Long | N | 객체의 크기(바이트). 지정한 경우 해당 크기와 일치하는 객체만 삭제합니다. |
| Delete.Quiet | Boolean | N | `true`로 설정하면 Quiet 모드로 동작하며, 실패한 항목만 응답에 포함됩니다. 기본값은 Verbose 모드(전체 결과 반환)입니다. |

### 응답
```http

HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<DeleteResult>
  <Deleted>
    <Key>String</Key>
  </Deleted>
  ...
  <Error>
    <Key>String</Key>
    <Code>String</Code>
    <Message>String</Message>
  </Error>
  ...
</DeleteResult>
```

| 이름 | 타입 | 설명 |
| --- | --- | --- |
| DeleteResult | Object | 삭제 결과의 루트 요소 |
| DeleteResult.Deleted | Array | 삭제에 성공한 객체 목록 |
| DeleteResult.Deleted.Key | String | 삭제된 객체 키 |
| DeleteResult.Error | Array | 삭제에 실패한 객체 목록 |
| DeleteResult.Error.Key | String | 삭제에 실패한 객체 키 |
| DeleteResult.Error.Code | String | 오류 코드 |
| DeleteResult.Error.Message | String | 오류 메시지 |