## Object
**Data & Analytics > Data Lake Storage > Amazon S3 호환 API 가이드 > Object**


## DeleteObject

バケットに保存されたオブジェクトを削除します。

### リクエスト

```http
DELETE /{bucket}/{objectKey} HTTP/1.1
```

#### 요청 헤더

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |
| objectKey | Path | String | Y | オブジェクト名 |

### レスポンス

```http
HTTP/1.1 204 No Content
```


## DeleteObjects

1つのリクエストで複数のオブジェクトを削除します。1回のリクエストで最大1,000個のオブジェクトキーを指定できます。

### リクエスト

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

#### 요청 헤더

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |
| Content-MD5 | Header | String | Y | リクエスト本文のMD5ハッシュ値(送信中の改ざん検証用) |

#### リクエスト本文

| 名前 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| Delete | Object | Y | リクエストのルート要素 |
| Delete.Object | Array | Y | 削除するオブジェクト一覧。最大1,000個 |
| Delete.Object.ETag | String | N | オブジェクトのETag値。指定した場合、ETagが一致するオブジェクトのみ削除します。 |
| Delete.Object.Key | String | Y | 削除するオブジェクトキー |
| Delete.Object.LastModifiedTime | String | N | オブジェクトの最終変更時間。指定した場合、該当時間と一致するオブジェクトのみ削除します。 |
| Delete.Object.Size | Long | N | オブジェクトのサイズ(バイト)。指定した場合、該当サイズと一致するオブジェクトのみ削除します。 |
| Delete.Quiet | Boolean | N | `true`に設定するとQuietモードで動作し、失敗した項目のみレスポンスに含まれます。デフォルト値はVerboseモード(全ての結果を返す)です。 |

### レスポンス

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

#### 응답 본문

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| DeleteResult | Object | 削除結果のルート要素 |
| DeleteResult.Deleted | Array | 削除に成功したオブジェクト一覧 |
| DeleteResult.Deleted.Key | String | 削除されたオブジェクトキー |
| DeleteResult.Error | Array | 削除に失敗したオブジェクト一覧 |
| DeleteResult.Error.Key | String | 削除に失敗したオブジェクトキー |
| DeleteResult.Error.Code | String | エラーコード |
| DeleteResult.Error.Message | String | エラーメッセージ |


## GetObject

バケットに保存されたオブジェクトを照会します。

### リクエスト

```http
GET /{bucket}/{objectKey} HTTP/1.1
```

#### 요청 헤더

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |
| objectKey | Path | String | Y | オブジェクト名 |
| Range | Header | String | N | 部分ダウンロードの範囲 |
| x-amz-storage-class | Header | String | N | ストレージクラス |

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
x-amz-checksum-type: ChecksumType

data
```

#### 응답 헤더

| 필드                       | 설명                                                                           |
|--------------------------|------------------------------------------------------------------------------|
| ETag                     | 특정 버전의 리소스에 서버가 할당하는 고유 식별자                                                  |
| x-amz-checksum-crc32     | 객체의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값                                       |
| x-amz-checksum-crc32c    | 객체의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값                                      |
| x-amz-checksum-crc64nvme | 객체의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값                                   |
| x-amz-checksum-sha1      | 객체의 160비트 `SHA1` 다이제스트 값을 Base64로 인코딩한 값                                     |
| x-amz-checksum-sha256    | 객체의 256비트 `SHA256` 다이제스트 값을 Base64로 인코딩한 값                                   |
| x-amz-checksum-sha512    | 객체의 512비트 `SHA512` 다이제스트 값을 Base64로 인코딩한 값                                   |
| x-amz-checksum-md5       | 객체의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값                                      |
| x-amz-checksum-xxhash64  | 객체의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값                                    |
| x-amz-checksum-xxhash3   | 객체의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값                                     |
| x-amz-checksum-xxhash128 | 객체의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값                                  |
| x-amz-checksum-type      | 멀티파트 객체의 파트별 체크섬을 결합하여 객체 수준의 체크섬을 생성한 방식. 유효한 값: `COMPOSITE \| FULL_OBJECT` |


## HeadObject

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

| 필드                       | 설명                                                                           |
|--------------------------|------------------------------------------------------------------------------|
| Content-Length           | 응답 본문의 크기(바이트)                                                               |
| Content-Type             | 객체 데이터의 형식을 나타내는 표준 MIME 타입                                                  |
| ETag                     | 특정 버전의 리소스에 서버가 할당하는 고유 식별자                                                  |
| Last-Modified            | 객체가 마지막으로 수정된 날짜 및 시간                                                        |
| x-amz-checksum-crc32     | 객체의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값                                       |
| x-amz-checksum-crc32c    | 객체의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값                                      |
| x-amz-checksum-crc64nvme | 객체의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값                                   |
| x-amz-checksum-sha1      | 객체의 160비트 `SHA1` 다이제스트 값을 Base64로 인코딩한 값                                     |
| x-amz-checksum-sha256    | 객체의 256비트 `SHA256` 다이제스트 값을 Base64로 인코딩한 값                                   |
| x-amz-checksum-sha512    | 객체의 512비트 `SHA512` 다이제스트 값을 Base64로 인코딩한 값                                   |
| x-amz-checksum-md5       | 객체의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값                                      |
| x-amz-checksum-xxhash64  | 객체의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값                                    |
| x-amz-checksum-xxhash3   | 객체의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값                                     |
| x-amz-checksum-xxhash128 | 객체의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값                                  |
| x-amz-checksum-type      | 멀티파트 객체의 파트별 체크섬을 결합하여 객체 수준의 체크섬을 생성한 방식. 유효한 값: `COMPOSITE \| FULL_OBJECT` |


## ListObjectsV2

バケットに保存されたオブジェクト一覧を照会します。

### リクエスト

```http
GET /{bucket}?list-type=2&continuation-token={continuationToken}&delimiter={delimiter}&encoding-type={encodingType}&fetch-owner={fetchOwner}&max-keys={maxKeys}&prefix={prefix}&start-after={startAfter} HTTP/1.1
```

#### 요청 헤더

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

#### リクエストパラメータ

| 名前                  | 区分        | タイプ     | 必須 | 説明                     |
|---------------------|-----------|---------|----|------------------------|
| bucket              | Path      | String  | Y  | バケット名                  |
| x-amz-storage-class | Header    | String  | N  | ストレージクラス               |
| list-type           | Parameter | String  | Y  | 2に固定（V2 API識別用）        |
| continuation-token  | Parameter | String  | N  | 次のページ照会識別子             |
| delimiter           | Parameter | String  | N  | キーグループの区切り文字(デフォルト: /) |
| encoding-type       | Parameter | String  | N  | キーのエンコーディング方式          |
| fetch-owner         | Parameter | Boolean | N  | 所有者情報を含めるかどうか          |
| max-keys            | Parameter | Integer | N  | 返却する最大オブジェクト数          |
| prefix              | Parameter | String  | N  | オブジェクト名のプレフィックス        |
| start-after         | Parameter | String  | N  | 照会開始の基準                |

### レスポンス

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<ListBucketResult>
  <Name>String</Name>
  <Prefix>String/</Prefix>
  <StartAfter>String</StartAfter>
  <ContinuationToken>String</ContinuationToken>
  <NextContinuationToken>String</NextContinuationToken>
  <KeyCount>Integer</KeyCount>
  <MaxKeys>Integer</MaxKeys>
  <Delimiter>String</Delimiter>
  <IsTruncated>Boolean</IsTruncated>
  <EncodingType>String</EncodingType>
  <Contents>
    <ChecksumAlgorithm>string</ChecksumAlgorithm>
    ...
    <ChecksumType>string</ChecksumType>
    <Key>String</Key>
    <LastModified>Timestamp</LastModified>
    <ETag>String</ETag>
    <Size>Long</Size>
    <StorageClass>String</StorageClass>
    <Owner>
      <ID>String</ID>
    </Owner>
  </Contents>
  <CommonPrefixes>
    <Prefix>String</Prefix>
  </CommonPrefixes>
</ListBucketResult>
```

#### 응답 본문

| 名前                                          | タイプ       | 説明                               |
|---------------------------------------------|-----------|----------------------------------|
| ListBucketResult                            | Object    | オブジェクト一覧照会結果                     |
| ListBucketResult.Name                       | String    | バケット名                            |
| ListBucketResult.Prefix                     | String    | リクエスト時に指定したオブジェクト名のプレフィックス       |
| ListBucketResult.StartAfter                 | String    | リクエスト時に指定した照会開始の基準               |
| ListBucketResult.ContinuationToken          | String    | 今回のリクエストに使用されたページ照会識別子           |
| ListBucketResult.NextContinuationToken      | String    | 次のページをリクエストする際に使用するページ照会識別子      |
| ListBucketResult.KeyCount                   | Integer   | レスポンスのオブジェクト数                    |
| ListBucketResult.MaxKeys                    | Integer   | リクエスト時に指定した最大オブジェクト数             |
| ListBucketResult.Delimiter                  | String    | リクエスト時に指定したキーグループの区切り文字          |
| ListBucketResult.IsTruncated                | Boolean   | 次のページがあるかどうか                     |
| ListBucketResult.Contents                   | Array     | オブジェクト一覧                         |
| ListBucketResult.Contents.ChecksumAlgorithm | Array     | 객체의 체크섬 생성에 사용된 알고리즘             |
| ListBucketResult.Contents.ChecksumType      | String    | 객체의 체크섬 값을 계산하는 방식               |
| ListBucketResult.Contents.Key               | String    | オブジェクトキー                         |
| ListBucketResult.Contents.LastModified      | Timestamp | 最終更新日時 (ISO 8601 形式)             |
| ListBucketResult.Contents.ETag              | String    | オブジェクト固有識別子                      |
| ListBucketResult.Contents.Size              | Long      | オブジェクトサイズ(bytes)                 |
| ListBucketResult.Contents.StorageClass      | String    | ストレージクラス                         |
| ListBucketResult.Contents.Owner.ID          | String    | 所有者ID (fetch-owner=true の場合に包含)  |
| ListBucketResult.CommonPrefixes             | Array     | delimiter基準でグループ化された共通のプレフィックス一覧 |
| ListBucketResult.CommonPrefixes.Prefix      | String    | delimiter基準でグループ化されたパス           |


## PutObject

バケットにオブジェクトを保存します。

### リクエスト

```http
PUT /{bucket}/{objectKey} HTTP/1.1
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

Body
```

#### 요청 헤더

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

| 필드                           | 필수 여부 | 설명                                                                                                                                               |
|------------------------------|-------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| Content-Length               | N     | 요청 본문의 크기(바이트)입니다. 본문의 크기를 자동으로 결정할 수 없는 경우 사용.                                                                                                  |
| Content-MD5                  | N     | RFC 1864에 따라 메시지 본문을 128비트 `MD5` 다이제스트로 계산한 후 Base64로 인코딩한 값. 데이터 무결성 검증을 위해 사용할 수 있으며, 필수는 아니지만 종단 간 무결성 검증 수단으로 사용하는 것을 권장                     |
| x-amz-checksum-crc32         | N     | 객체의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값                                                                                                         |
| x-amz-checksum-crc32c        | N     | 객체의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값                                                                                                        |
| x-amz-checksum-crc64nvme     | N     | 객체의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값                                                                                                     |
| x-amz-checksum-sha1          | N     | 객체의 160비트 `SHA1` 다이제스트 값을 Base64로 인코딩한 값                                                                                                       |
| x-amz-checksum-sha256        | N     | 객체의 256비트 `SHA256` 다이제스트 값을 Base64로 인코딩한 값                                                                                                     |
| x-amz-checksum-sha512        | N     | 객체의 512비트 `SHA512` 다이제스트 값을 Base64로 인코딩한 값                                                                                                     |
| x-amz-checksum-md5           | N     | 객체의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값                                                                                                        |
| x-amz-checksum-xxhash64      | N     | 객체의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값                                                                                                      |
| x-amz-checksum-xxhash3       | N     | 객체의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값                                                                                                       |
| x-amz-checksum-xxhash128     | N     | 객체의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값                                                                                                    |
| x-amz-sdk-checksum-algorithm | N     | SDK를 사용하여 객체의 체크섬을 생성할 때 사용한 알고리즘을 지정. 이 헤더를 전송할 경우 반드시 `x-amz-checksum-algorithm` 또는 `x-amz-trailer` 헤더를 함께 전송해야 하며, 그렇지 않을 경우 HTTP 400 오류가 반환됨 |

#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明                |
| --- | --- | --- | --- |-------------------|
| bucket | Path | String | Y | バケット名             |
| objectKey | Path | String | Y | オブジェクト名           |
| Content-Type | Header | String | N | オブジェクトのコンテンツタイプ   |
| Content-Length | Header | Long | Y | オブジェクトのサイズ(bytes) |
| x-amz-storage-class | Header | String | N | ストレージクラス          |
| x-amz-meta-\* | Header | String | N | ユーザー定義メタデータ       |

#### リクエストボディ

| 名前 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| Body | Binary | Y | オブジェクトデータ |

### レスポンス

```http
HTTP/1.1 200 OK
ETag: String
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

| 필드                       | 설명                                                                                                                                 |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------|
| ETag                     | 업로드된 객체의 엔티티 태그. ETag가 객체의 MD5 다이제스트인 경우, 객체 업로드 시 계산한 MD5 값과 반환된 ETag를 비교하여 데이터 무결성을 확인 가능                                  |
| x-amz-checksum-crc32     | 객체의 32비트 `CRC32` 체크섬 값을 Base64로 인코딩한 값                                                                                           |
| x-amz-checksum-crc32c    | 객체의 32비트 `CRC32C` 체크섬 값을 Base64로 인코딩한 값                                                                                          |
| x-amz-checksum-crc64nvme | 객체의 64비트 `CRC64NVME` 체크섬 값을 Base64로 인코딩한 값. `CRC64NVME` 알고리즘으로 업로드되었거나, 체크섬 없이 업로드되어 기본 체크섬(`CRC64NVME`)이 자동으로 추가된 경우에 응답에 포함됩니다. |
| x-amz-checksum-sha1      | 객체의 160비트 `SHA1` 다이제스트 값을 Base64로 인코딩한 값                                                                                         |
| x-amz-checksum-sha256    | 객체의 256비트 `SHA256` 다이제스트 값을 Base64로 인코딩한 값                                                                                       |
| x-amz-checksum-sha512    | 객체의 512비트 `SHA512` 다이제스트 값을 Base64로 인코딩한 값                                                                                       |
| x-amz-checksum-md5       | 객체의 128비트 `MD5` 다이제스트 값을 Base64로 인코딩한 값                                                                                          |
| x-amz-checksum-xxhash64  | 객체의 64비트 `XXHASH64` 체크섬 값을 Base64로 인코딩한 값                                                                                        |
| x-amz-checksum-xxhash3   | 객체의 64비트 `XXHASH3` 체크섬 값을 Base64로 인코딩한 값                                                                                         |
| x-amz-checksum-xxhash128 | 객체의 128비트 `XXHASH128` 체크섬 값을 Base64로 인코딩한 값                                                                                      |
| x-amz-checksum-type      | 멀티파트 객체의 파트별 체크섬을 결합하여 객체 수준의 체크섬을 생성한 방식. PutObject 업로드의 경우 항상 `FULL_OBJECT`로 반환됨                                             |
