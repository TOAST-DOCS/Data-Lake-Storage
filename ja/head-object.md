## HeadObject

**Data & Analytics > Data Lake Storage > API ガイド > Object > HeadObject**

バケットに保存されたオブジェクトのメタデータを照会します。

### リクエスト

```http
HEAD /{bucket}/{objectKey} HTTP/1.1
```

#### 요청 헤더

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |
| objectKey | Path | String | Y | オブジェクト名 |
| x-amz-storage-class | Header | String | N | ストレージクラス |

### レスポンス

```http
HTTP/1.1 200 OK
Content-Type: String
Content-Length: Long
ETag: String
Last-Modified: Timestamp
x-amz-storage-class: String
x-amz-meta-{key}: String
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
```

#### 응답 헤더

| 필드                       | 설명                                                                               |
|--------------------------|----------------------------------------------------------------------------------|
| Content-Length           | 응답 본문의 크기(바이트)                                                                   |
| Content-Type             | 오브젝트 데이터의 형식을 나타내는 표준 MIME 타입                                                    |
| ETag                     | 특정 버전의 리소스에 대해 서버가 할당하는 고유 식별자.                                                  |
| Last-Modified            | 오브젝트가 마지막으로 수정된 날짜 및 시간                                                          |
| x-amz-checksum-crc32     | 오브젝트의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값                                         |
| x-amz-checksum-crc32c    | 오브젝트의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값                                        |
| x-amz-checksum-crc64nvme | 오브젝트의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값                                     |
| x-amz-checksum-sha1      | 오브젝트의 160비트 `SHA1` 다이제스트 값을 Base64로 인코딩한 값                                       |
| x-amz-checksum-sha256    | 오브젝트의 256비트 `SHA256` 다이제스트 값을 Base64로 인코딩한 값                                     |
| x-amz-checksum-sha512    | 오브젝트의 512비트 `SHA512` 다이제스트 값을 Base64로 인코딩한 값                                     |
| x-amz-checksum-md5       | 오브젝트의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값                                        |
| x-amz-checksum-xxhash64  | 오브젝트의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값                                      |
| x-amz-checksum-xxhash3   | 오브젝트의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값                                       |
| x-amz-checksum-xxhash128 | 오브젝트의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값                                    |
| x-amz-checksum-type      | 멀티파트 오브젝트의 파트별 체크섬을 결합하여 오브젝트 수준의 체크섬을 생성한 방식. 유효한 값: `COMPOSITE \| FULL_OBJECT` |
