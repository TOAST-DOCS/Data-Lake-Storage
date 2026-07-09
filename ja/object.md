## Object
**Data & Analytics > Data Lake Storage > Amazon S3互換APIガイド > Object**


## DeleteObject

バケットに保存されたオブジェクトを削除します。

### リクエスト

```http
DELETE /{bucket}/{objectKey} HTTP/1.1
```

#### リクエストヘッダ

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

#### リクエストヘッダ

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

#### レスポンス本文

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

#### リクエストヘッダ

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

#### レスポンスヘッダ

| フィールド                 | 説明                                                                         |
|--------------------------|------------------------------------------------------------------------------|
| ETag                     | 特定のバージョンのリソースにサーバーが割り当てる一意の識別子                                                  |
| x-amz-checksum-crc32     | オブジェクトの32ビット`CRC32`チェックサム値をBase64でエンコードした値                                       |
| x-amz-checksum-crc32c    | オブジェクトの32ビット`CRC32C`チェックサム値をBase64でエンコードした値                                      |
| x-amz-checksum-crc64nvme | オブジェクトの64ビット`CRC64NVME`チェックサム値をBase64でエンコードした値                                 |
| x-amz-checksum-sha1      | オブジェクトの160ビット`SHA1`ダイジェスト値をBase64でエンコードした値                                     |
| x-amz-checksum-sha256    | オブジェクトの256ビット`SHA256`ダイジェスト値をBase64でエンコードした値                                     |
| x-amz-checksum-sha512    | オブジェクトの512ビット`SHA512`ダイジェスト値をBase64でエンコードした値                                     |
| x-amz-checksum-md5       | オブジェクトの128ビット`MD5`ダイジェスト値をBase64でエンコードした値                                      |
| x-amz-checksum-xxhash64  | オブジェクトの64ビット`XXHASH64`チェックサム値をBase64でエンコードした値                                    |
| x-amz-checksum-xxhash3   | オブジェクトの64ビット`XXHASH3`チェックサム値をBase64でエンコードした値                                     |
| x-amz-checksum-xxhash128 | オブジェクトの128ビット`XXHASH128`チェックサム値をBase64でエンコードした値                                  |
| x-amz-checksum-type      | マルチパートオブジェクトのパート別チェックサムを結合してオブジェクトレベルのチェックサムを生成した方式。有効な値: `COMPOSITE \| FULL_OBJECT` |


## HeadObject

バケットに保存されたオブジェクトのメタデータを照会します。

### リクエスト

```http
HEAD /{bucket}/{objectKey} HTTP/1.1
```

#### リクエストヘッダ

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

#### レスポンスヘッダ

| フィールド                 | 説明                                                                         |
|--------------------------|------------------------------------------------------------------------------|
| Content-Length           | レスポンス本文のサイズ(バイト)                                                       |
| Content-Type             | オブジェクトデータの形式を表す標準MIMEタイプ                                                                      |
| ETag                     | 特定のバージョンのリソースにサーバーが割り当てる一意の識別子                                                  |
| Last-Modified            | オブジェクトが最後に変更された日時                                                                               |
| x-amz-checksum-crc32     | オブジェクトの32ビット`CRC32`チェックサム値をBase64でエンコードした値                                       |
| x-amz-checksum-crc32c    | オブジェクトの32ビット`CRC32C`チェックサム値をBase64でエンコードした値                                      |
| x-amz-checksum-crc64nvme | オブジェクトの64ビット`CRC64NVME`チェックサム値をBase64でエンコードした値                                 |
| x-amz-checksum-sha1      | オブジェクトの160ビット`SHA1`ダイジェスト値をBase64でエンコードした値                                     |
| x-amz-checksum-sha256    | オブジェクトの256ビット`SHA256`ダイジェスト値をBase64でエンコードした値                                     |
| x-amz-checksum-sha512    | オブジェクトの512ビット`SHA512`ダイジェスト値をBase64でエンコードした値                                     |
| x-amz-checksum-md5       | オブジェクトの128ビット`MD5`ダイジェスト値をBase64でエンコードした値                                      |
| x-amz-checksum-xxhash64  | オブジェクトの64ビット`XXHASH64`チェックサム値をBase64でエンコードした値                                    |
| x-amz-checksum-xxhash3   | オブジェクトの64ビット`XXHASH3`チェックサム値をBase64でエンコードした値                                     |
| x-amz-checksum-xxhash128 | オブジェクトの128ビット`XXHASH128`チェックサム値をBase64でエンコードした値                                  |
| x-amz-checksum-type      | マルチパートオブジェクトのパート別チェックサムを結合してオブジェクトレベルのチェックサムを生成した方式。有効な値: `COMPOSITE \| FULL_OBJECT` |


## ListObjectsV2

バケットに保存されたオブジェクト一覧を照会します。

### リクエスト

```http
GET /{bucket}?list-type=2&continuation-token={continuationToken}&delimiter={delimiter}&encoding-type={encodingType}&fetch-owner={fetchOwner}&max-keys={maxKeys}&prefix={prefix}&start-after={startAfter} HTTP/1.1
```

#### リクエストヘッダ

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

#### レスポンス本文

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
| ListBucketResult.Contents.ChecksumAlgorithm | Array     | オブジェクトのチェックサム生成に使用されたアルゴリズム             |
| ListBucketResult.Contents.ChecksumType      | String    | オブジェクトのチェックサム値を計算する方式                 |
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

#### リクエストヘッダ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

| フィールド                     | 必須 | 説明                                                                                                                                                                             |
|------------------------------|-------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| Content-Length               | N     | リクエスト本文のサイズ(バイト)です。本文のサイズを自動的に決定できない場合に使用。                                                                                                                |
| Content-MD5                  | N     | RFC 1864に従ってメッセージ本文を128ビット`MD5`ダイジェストとして計算した後、Base64でエンコードした値。データ整合性検証のために使用でき、必須ではありませんが、エンドツーエンドの整合性検証手段として使用することを推奨                     |
| x-amz-checksum-crc32         | N     | オブジェクトの32ビット`CRC32`チェックサム値をBase64でエンコードした値                                                                                                                |
| x-amz-checksum-crc32c        | N     | オブジェクトの32ビット`CRC32C`チェックサム値をBase64でエンコードした値                                                                                                               |
| x-amz-checksum-crc64nvme     | N     | オブジェクトの64ビット`CRC64NVME`チェックサム値をBase64でエンコードした値                                                                                                            |
| x-amz-checksum-sha1          | N     | オブジェクトの160ビット`SHA1`ダイジェスト値をBase64でエンコードした値                                                                                                                |
| x-amz-checksum-sha256        | N     | オブジェクトの256ビット`SHA256`ダイジェスト値をBase64でエンコードした値                                                                                                              |
| x-amz-checksum-sha512        | N     | オブジェクトの512ビット`SHA512`ダイジェスト値をBase64でエンコードした値                                                                                                              |
| x-amz-checksum-md5           | N     | オブジェクトの128ビット`MD5`ダイジェスト値をBase64でエンコードした値                                                                                                                 |
| x-amz-checksum-xxhash64      | N     | オブジェクトの64ビット`XXHASH64`チェックサム値をBase64でエンコードした値                                                                                                             |
| x-amz-checksum-xxhash3       | N     | オブジェクトの64ビット`XXHASH3`チェックサム値をBase64でエンコードした値                                                                                                              |
| x-amz-checksum-xxhash128     | N     | オブジェクトの128ビット`XXHASH128`チェックサム値をBase64でエンコードした値                                                                                                           |
| x-amz-sdk-checksum-algorithm | N     | SDKを使用してオブジェクトのチェックサムを生成する際に使用したアルゴリズムを指定。このヘッダを送信する場合、必ず`x-amz-checksum-algorithm`または`x-amz-trailer`ヘッダを一緒に送信する必要があり、そうでない場合はHTTP 400エラーが返される |

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

#### レスポンスヘッダ

| フィールド                 | 説明                                                                                                                                                                                                             |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------|
| ETag                     | アップロードされたオブジェクトのエンティティタグ。ETagがオブジェクトのMD5ダイジェストである場合、オブジェクトのアップロード時に計算したMD5値と返されたETagを比較してデータ整合性を確認可能                                  |
| x-amz-checksum-crc32     | オブジェクトの32ビット`CRC32`チェックサム値をBase64でエンコードした値                                                                                                                |
| x-amz-checksum-crc32c    | オブジェクトの32ビット`CRC32C`チェックサム値をBase64でエンコードした値                                                                                                               |
| x-amz-checksum-crc64nvme | オブジェクトの64ビット`CRC64NVME`チェックサム値をBase64でエンコードした値。`CRC64NVME`アルゴリズムでアップロードされたか、チェックサムなしでアップロードされてデフォルトのチェックサム(`CRC64NVME`)が自動的に追加された場合にレスポンスに含まれます。 |
| x-amz-checksum-sha1      | オブジェクトの160ビット`SHA1`ダイジェスト値をBase64でエンコードした値                                                                                                                |
| x-amz-checksum-sha256    | オブジェクトの256ビット`SHA256`ダイジェスト値をBase64でエンコードした値                                                                                                              |
| x-amz-checksum-sha512    | オブジェクトの512ビット`SHA512`ダイジェスト値をBase64でエンコードした値                                                                                                              |
| x-amz-checksum-md5       | オブジェクトの128ビット`MD5`ダイジェスト値をBase64でエンコードした値                                                                                                                 |
| x-amz-checksum-xxhash64  | オブジェクトの64ビット`XXHASH64`チェックサム値をBase64でエンコードした値                                                                                                             |
| x-amz-checksum-xxhash3   | オブジェクトの64ビット`XXHASH3`チェックサム値をBase64でエンコードした値                                                                                                              |
| x-amz-checksum-xxhash128 | オブジェクトの128ビット`XXHASH128`チェックサム値をBase64でエンコードした値                                                                                                           |
| x-amz-checksum-type      | マルチパートオブジェクトのパート別チェックサムを結合してオブジェクトレベルのチェックサムを生成した方式。PutObjectアップロードの場合は常に`FULL_OBJECT`として返される                                       |
