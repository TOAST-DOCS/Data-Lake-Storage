## Multipart
**Data & Analytics > Data Lake Storage > Amazon S3 호환 API 가이드 > Multipart**


## AbortMultipartUpload

進行中のマルチパートアップロードを中断します。

### リクエスト

```http
DELETE /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1
```

#### 요청 헤더

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |
| objectKey | Path | String | Y | オブジェクト名 |
| x-amz-storage-class | Header | String | N | ストレージクラス |
| uploadId | Parameter | String | Y | マルチパートアップロードID |

### レスポンス

```http
HTTP/1.1 204 No Content
```


## CompleteMultipartUpload

アップロードされたパートを組み合わせてオブジェクトを保存し、マルチパートアップロードを完了します。

### リクエスト

```http
POST /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1
x-amz-checksum-crc32: ChecksumCRC32
x-amz-checksum-crc32c: ChecksumCRC32C
x-amz-checksum-crc64nvme: ChecksumCRC64NVME
x-amz-checksum-sha1: ChecksumSHA1
x-amz-checksum-sha256: ChecksumSHA256
x-amz-checksum-sha512: ChecksumSHA512
x-amz-checksum-md5: ChecksumMD5
x-amz-checksum-xxhash64: ChecksumXXHASH64
x-amz-checksum-xxhash3: ChecksumXXHASH3
x-amz-checksum-xxhash128: ChecksumXXHASH128
x-amz-checksum-type: ChecksumType

<?xml version="1.0" encoding="UTF-8"?>
<CompleteMultipartUpload xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Part>
  <ChecksumCRC32>string</ChecksumCRC32>
    <ChecksumCRC32C>string</ChecksumCRC32C>
    <ChecksumCRC64NVME>string</ChecksumCRC64NVME>
    <ChecksumMD5>string</ChecksumMD5>
    <ChecksumSHA1>string</ChecksumSHA1>
    <ChecksumSHA256>string</ChecksumSHA256>
    <ChecksumSHA512>string</ChecksumSHA512>
    <ChecksumXXHASH128>string</ChecksumXXHASH128>
    <ChecksumXXHASH3>string</ChecksumXXHASH3>
    <ChecksumXXHASH64>string</ChecksumXXHASH64>
    <PartNumber>Integer</PartNumber>
    <ETag>String</ETag>
  </Part>
</CompleteMultipartUpload>
```

#### 요청 헤더

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

| 이름                       | 필수 여부 | 설명                                                                                                                                                        |
|--------------------------|-------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-amz-checksum-crc32     | N     | 데이터 무결성 검증을 위한 헤더. 오브젝트의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값                                                                                               |
| x-amz-checksum-crc32c    | N     | 데이터 무결성 검증을 위한 헤더. 오브젝트의 32비트 `CRC32C` 체크섬 값을 Base64로 인코한 값                                                                                               |
| x-amz-checksum-crc64nvme | N     | 데이터 무결성 검증을 위한 헤더. 오브젝트의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값                                                                                           |
| x-amz-checksum-sha1      | N     | 데이터 무결성 검증을 위한 헤더. 오브젝트의 160비트 `SHA1` 다이제스트 값을 Base64로 인코딩한 값                                                                                             |
| x-amz-checksum-sha256    | N     | 데이터 무결성 검증을 위한 헤더. 오브젝트의 256비트 `SHA256` 다이제스트 값을 Base64로 인코딩한 값                                                                                           |
| x-amz-checksum-sha512    | N     | 데이터 무결성 검증을 위한 헤더. 오브젝트의 512비트 `SHA512` 다이제스트 값을 Base64로 인코딩한 값                                                                                           |
| x-amz-checksum-md5       | N     | 데이터 무결성 검증을 위한 헤더. 오브젝트의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값                                                                                              |
| x-amz-checksum-xxhash64  | N     | 데이터 무결성 검증을 위한 헤더. 오브젝트의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값                                                                                            |
| x-amz-checksum-xxhash3   | N     | 데이터 무결성 검증을 위한 헤더. 오브젝트의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값                                                                                             |
| x-amz-checksum-xxhash128 | N     | 데이터 무결성 검증을 위한 헤더. 오브젝트의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값                                                                                          |
| x-amz-checksum-type      | N     | 멀티파트 업로드에서 파트별 체크섬을 결합하여 오브젝트 수준의 체크섬을 생성하는 방식. `CreateMultipartUpload` 요청에서 지정한 체크섬 타입과 일치하지 않을 경우 `BadDigest` 오류가 반환. 유효한 값: `COMPOSITE \| FULL_OBJECT` |

#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |
| objectKey | Path | String | Y | オブジェクト名 |
| x-amz-storage-class | Header | String | N | ストレージクラス |
| uploadId | Parameter | String | Y | マルチパートアップロードID |

#### リクエストボディ

| 名前 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| CompleteMultipartUpload | Object | Y | マルチパートアップロード完了リクエスト |
| CompleteMultipartUpload.Part | Object | N | パート一覧 |
| CompleteMultipartUpload.Part.PartNumber | Integer | N | パート番号 |
| CompleteMultipartUpload.Part.ETag | String | N | オブジェクト固有識別子 |
| CompleteMultipartUpload.Part.ChecksumCRC32     | String  | N  | 파트의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값      |
| CompleteMultipartUpload.Part.ChecksumCRC32C    | String  | N  | 파트의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값     |
| CompleteMultipartUpload.Part.ChecksumCRC64NVME | String  | N  | 파트의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값  |
| CompleteMultipartUpload.Part.ChecksumSHA1      | String  | N  | 파트의 160비트 `SHA1` 체크섬 값을 Base64로 인코딩한 값      |
| CompleteMultipartUpload.Part.ChecksumSHA256    | String  | N  | 파트의 256비트 `SHA256` 체크섬 값을 Base64로 인코딩한 값    |
| CompleteMultipartUpload.Part.ChecksumSHA512    | String  | N  | 파트의 512비트 `SHA512` 체크섬 값을 Base64로 인코딩한 값    |
| CompleteMultipartUpload.Part.ChecksumMD5       | String  | N  | 파트의 128비트 `MD5` 체크섬 값을 Base64로 인코딩한 값       |
| CompleteMultipartUpload.Part.ChecksumXXHASH64  | String  | N  | 파트의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값   |
| CompleteMultipartUpload.Part.ChecksumXXHASH3   | String  | N  | 파트의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값    |
| CompleteMultipartUpload.Part.ChecksumXXHASH128 | String  | N  | 파트의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값 |

### レスポンス

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<CompleteMultipartUploadResult>
  <Location>String</Location>
  <Bucket>String</Bucket>
  <Key>String</Key>
  <ETag>String</ETag>
  <ChecksumCRC32>string</ChecksumCRC32>
  <ChecksumCRC32C>string</ChecksumCRC32C>
  <ChecksumCRC64NVME>string</ChecksumCRC64NVME>
  <ChecksumSHA1>string</ChecksumSHA1>
  <ChecksumSHA256>string</ChecksumSHA256>
  <ChecksumSHA512>string</ChecksumSHA512>
  <ChecksumMD5>string</ChecksumMD5>
  <ChecksumXXHASH64>string</ChecksumXXHASH64>
  <ChecksumXXHASH3>string</ChecksumXXHASH3>
  <ChecksumXXHASH128>string</ChecksumXXHASH128>
  <ChecksumType>string</ChecksumType>
</CompleteMultipartUploadResult>
```

#### 응답 본문

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| CompleteMultipartUploadResult | Object | マルチパートアップロード完了結果 |
| CompleteMultipartUploadResult.Location | String | 作成されたオブジェクトパス |
| CompleteMultipartUploadResult.Bucket | String | 対象バケット名 |
| CompleteMultipartUploadResult.Key | String | 作成されたオブジェクトキー |
| CompleteMultipartUploadResult.ETag | String | 最終的に結合されたオブジェクト固有識別子 |
| CompleteMultipartUploadResult.ChecksumCRC32     | String | 파트의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값       |
| CompleteMultipartUploadResult.ChecksumCRC32C    | String | 파트의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값      |
| CompleteMultipartUploadResult.ChecksumCRC64NVME | String | 파트의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값   |
| CompleteMultipartUploadResult.ChecksumSHA1      | String | 파트의 160비트 `SHA1` 체크섬 값을 Base64로 인코딩하여 인코딩한 값 |
| CompleteMultipartUploadResult.ChecksumSHA256    | String | 파트의 256비트 `SHA256` 체크섬 값을 Base64로 인코딩한 값     |
| CompleteMultipartUploadResult.ChecksumSHA512    | String | 파트의 512비트 `SHA512` 체크섬 값을 Base64로 인코딩한 값     |
| CompleteMultipartUploadResult.ChecksumMD5       | String | 파트의 128비트 `MD5` 체크섬 값을 Base64로 인코딩한 값        |
| CompleteMultipartUploadResult.ChecksumXXHASH64  | String | 파트의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값    |
| CompleteMultipartUploadResult.ChecksumXXHASH3   | String | 파트의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값     |
| CompleteMultipartUploadResult.ChecksumXXHASH128 | String | 파트의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값  |


## CreateMultipartUpload

大容量のオブジェクトをアップロードできるように、マルチパートアップロードを開始してアップロードIDを生成します。アップロードIDは最大1時間有効です。

### リクエスト

```http
POST /{bucket}/{objectKey}?uploads HTTP/1.1
x-amz-checksum-algorithm: ChecksumAlgorithm
x-amz-checksum-type: ChecksumType
```

#### 요청 헤더

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

| 필드                       | 필수 여부 | 설명                                                                                                                                       |
|--------------------------|-------|------------------------------------------------------------------------------------------------------------------------------------------|
| x-amz-checksum-algorithm | N     | 오브젝트의 체크섬 생성에 사용할 알고리즘을 지정. 유효한 값: `CRC32 \| CRC32C \| SHA1 \| SHA256 \| CRC64NVME \| SHA512 \| MD5 \| XXHASH64 \| XXHASH3 \| XXHASH128` |
| x-amz-checksum-type      | N     | 오브젝트의 체크섬 값을 계산하는 방식을 지정. 유효한 값: `COMPOSITE \| FULL_OBJECT`                                                                              |

#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |
| objectKey | Path | String | Y | オブジェクト名 |
| Content-Type | Header | String | N | オブジェクトのコンテンツタイプ |
| x-amz-storage-class | Header | String | N | ストレージクラス |
| x-amz-meta-\* | Header | String | N | ユーザー定義メタデータ |

### レスポンス

```http
HTTP/1.1 200 OK
x-amz-checksum-algorithm: ChecksumAlgorithm
x-amz-checksum-type: ChecksumType

<?xml version="1.0" encoding="UTF-8"?>
<InitiateMultipartUploadResult>
  <Bucket>String</Bucket>
  <Key>String</Key>
  <UploadId>String</UploadId>
</InitiateMultipartUploadResult>
```

#### 응답 헤더

| 필드                       | 설명                       |
|--------------------------|--------------------------|
| x-amz-checksum-algorithm | 오브젝트의 체크섬 생성에 사용한 알고리즘 값 |
| x-amz-checksum-type      | 오브젝트의 체크섬 값을 계산한 방식      |

#### 응답 본문

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| InitiateMultipartUploadResult | Object | マルチパートアップロード開始結果 |
| InitiateMultipartUploadResult.Bucket | String | 対象バケット名 |
| InitiateMultipartUploadResult.Key | String | オブジェクトキー |
| InitiateMultipartUploadResult.UploadId | String | マルチパートアップロードID |


## ListParts

マルチパートアップロードのパート一覧を照会します。

### リクエスト

```http
GET /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1
```

#### 요청 헤더

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |
| objectKey | Path | String | Y | オブジェクト名 |
| x-amz-storage-class | Header | String | N | ストレージクラス |
| uploadId | Parameter | String | Y | マルチパートアップロードID |

### レスポンス

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

#### 응답 본문

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| ListPartsResult | Object | パート一覧照会結果 |
| ListPartsResult.Bucket | String | バケット名 |
| ListPartsResult.Key | String | オブジェクトキー |
| ListPartsResult.UploadId | String | マルチパートアップロードID |
| ListPartsResult.StorageClass | String | ストレージクラス |
| ListPartsResult.PartNumberMarker | Integer | 照会開始の基準 |
| ListPartsResult.NextPartNumberMarker | Integer | 次の照会開始の基準 |
| ListPartsResult.MaxParts | Integer | 最大パート数 |
| ListPartsResult.IsTruncated | Boolean | 次のページがあるかどうか |
| ListPartsResult.Part | Object | パート情報 |
| ListPartsResult.Part.PartNumber | Integer | パート番号 |
| ListPartsResult.Part.LastModified | Timestamp | 最終更新日時(ISO 8601 形式) |
| ListPartsResult.Part.ETag | String | パートのオブジェクト固有識別子 |
| ListPartsResult.Part.Size | Long | パートのサイズ(bytes) |


## UploadPart

マルチパートアップロードのパートをアップロードします。アップロードする前にCreateMultipartUpload APIを呼び出して、マルチパートアップロードIDを生成する必要があります。

### リクエスト

```http
PUT /{bucket}/{objectKey}?partNumber={partNumber}&uploadId={uploadId} HTTP/1.1
Content-Length: ContentLength
Content-MD5: ContentMD5
x-amz-sdk-checksum-algorithm: ChecksumAlgorithm
x-amz-checksum-crc32: ChecksumCRC32
x-amz-checksum-crc32c: ChecksumCRC32C
x-amz-checksum-crc64nvme: ChecksumCRC64NVME
x-amz-checksum-sha1: ChecksumSHA1
x-amz-checksum-sha256: ChecksumSHA256
x-amz-checksum-sha512: ChecksumSHA512
x-amz-checksum-md5: ChecksumMD5
x-amz-checksum-xxhash64: ChecksumXXHASH64
x-amz-checksum-xxhash3: ChecksumXXHASH3
x-amz-checksum-xxhash128: ChecksumXXHASH128

body
```

#### 요청 헤더

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

| 필드                       | 필수 여부 | 설명                                          |
|--------------------------|-------|---------------------------------------------|
| Content-Length           | N     | 요청 본문의 크기(바이트). 본문의 크기를 자동으로 결정할 수 없는 경우 사용 |
| Content-MD5              | N     | 파트 데이터의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값 |
| x-amz-checksum-crc32     | N     | 파트의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값      |
| x-amz-checksum-crc32c    | N     | 파트의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값     |
| x-amz-checksum-crc64nvme | N     | 파트의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값  |
| x-amz-checksum-sha1      | N     | 파트의 160비트 `SHA1` 다이제스트 값을 Base64로 인코딩한 값    |
| x-amz-checksum-sha256    | N     | 파트의 256비트 `SHA256` 다이제스트 값을 Base64로 인코딩한 값  |
| x-amz-checksum-sha512    | N     | 파트의 512비트 `SHA512` 다이제스트 값을 Base64로 인코딩한 값. |
| x-amz-checksum-md5       | N     | 파트의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값     |
| x-amz-checksum-xxhash64  | N     | 파트의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값   |
| x-amz-checksum-xxhash3   | N     | 파트의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값    |
| x-amz-checksum-xxhash128 | N     | 파트의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값 |

#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |
| objectKey | Path | String | Y | オブジェクト名 |
| x-amz-storage-class | Header | String | N | ストレージクラス |
| partNumber | Parameter | Integer | Y | パート番号(1 ～ 10,000) |
| uploadId | Parameter | String | Y | マルチパートアップロードID |

#### リクエストボディ

| 名前 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| Body | Binary | Y | パートのバイナリデータ。最大5GiBまでアップロード可能 |

### レスポンス

```http
HTTP/1.1 200 OK
ETag: ETag
x-amz-checksum-crc32: ChecksumCRC32
x-amz-checksum-crc32c: ChecksumCRC32C
x-amz-checksum-crc64nvme: ChecksumCRC64NVME
x-amz-checksum-sha1: ChecksumSHA1
x-amz-checksum-sha256: ChecksumSHA256
x-amz-checksum-sha512: ChecksumSHA512
x-amz-checksum-md5: ChecksumMD5
x-amz-checksum-xxhash64: ChecksumXXHASH64
x-amz-checksum-xxhash3: ChecksumXXHASH3
x-amz-checksum-xxhash128: ChecksumXXHASH128
```

#### 응답 헤더

| 필드                       | 설명                                          |
|--------------------------|---------------------------------------------|
| ETag                     | 업로드된 파트의 엔티티 태그                             |
| x-amz-checksum-crc32     | 파트의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값      |
| x-amz-checksum-crc32c    | 파트의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값     |
| x-amz-checksum-crc64nvme | 파트의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값  |
| x-amz-checksum-sha1      | 파트의 160비트 `SHA1` 체크섬 값을 Base64로 인코딩한 값      |
| x-amz-checksum-sha256    | 파트의 256비트 `SHA256` 체크섬 값을 Base64로 인코딩한 값    |
| x-amz-checksum-sha512    | 파트의 512비트 `SHA512` 체크섬 값을 Base64로 인코딩한 값    |
| x-amz-checksum-md5       | 파트의 128비트 `MD5` 체크섬 값을 Base64로 인코딩한 값       |
| x-amz-checksum-xxhash64  | 파트의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값   |
| x-amz-checksum-xxhash3   | 파트의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값    |
| x-amz-checksum-xxhash128 | 파트의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값 |
