# ListParts

**Data & Analytics > Data Lake Storage > API 가이드 > Multipart > ListParts**

멀티 파트 업로드의 파트 목록을 조회합니다.

### 요청

```http
GET /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1
```

### 요청 파라미터

Data Lake Storage API에서 공통으로 사용하는 헤더에 대한 정보는 Data Lake Storage API 요청 헤더(API 가이드 > Data Lake Storage API 공통 정보 > 요청 > 요청 헤더 링크 필요)를 참조해 주세요.

| 이름 | 구분 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | 버킷 이름 |
| objectKey | Path | String | Y | 객체 이름 |
| x-amz-storage-class | Header | String | N | 스토리지 클래스 |
| uploadId | Parameter | String | Y | 멀티 파트 업로드 ID |

### 응답

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<ListPartsResult>
  <Bucket>String</Bucket>
  <Key>String</Key>
  <UploadId>String</UploadId>
  <StorageClass>String</StorageClass>
  <PartNumberMarker>Integer</PartNumberMarker>
  <NextPartNumberMarker>Integer</NextPartNumberMarker>
  <MaxParts>Integer</MaxParts>
  <IsTruncated>Boolean</IsTruncated>
  <Part>
    <PartNumber>Integer</PartNumber>
    <LastModified>Timestamp</LastModified>
    <ETag>String</ETag>
    <Size>Long</Size>
  </Part>
</ListPartsResult>
```

| 이름 | 타입 | 설명 |
| --- | --- | --- |
| ListPartsResult | Object | 파트 목록 조회 결과 |
| ListPartsResult.Bucket | String | 버킷 이름 |
| ListPartsResult.Key | String | 객체 키 |
| ListPartsResult.UploadId | String | 멀티 파트 업로드 ID |
| ListPartsResult.StorageClass | String | 스토리지 클래스 |
| ListPartsResult.PartNumberMarker | Integer | 조회 시작 기준 |
| ListPartsResult.NextPartNumberMarker | Integer | 다음 조회 시작 기준 |
| ListPartsResult.MaxParts | Integer | 최대 파트 수 |
| ListPartsResult.IsTruncated | Boolean | 추가 페이지 존재 여부 |
| ListPartsResult.Part | Object | 파트 정보 |
| ListPartsResult.Part.PartNumber | Integer | 파트 번호 |
| ListPartsResult.Part.LastModified | Timestamp | 마지막 수정 일시 (ISO 8601 형식) |
| ListPartsResult.Part.ETag | String | 파트 객체 고유 식별자 |
| ListPartsResult.Part.Size | Long | 파트 크기 (bytes) |
