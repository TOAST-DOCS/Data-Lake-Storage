<!-- pre-align:aligned sig=f760859bc93e -->

<a id="multipart"></a>
## Multipart { #multipart }

**Data & Analytics > Data Lake Storage > Amazon S3互換APIガイド > Multipart**


<a id="abortmultipartupload"></a>
## AbortMultipartUpload { #abortmultipartupload }

進行中のマルチパートアップロードを中断します。

<a id="request"></a>
### リクエスト { #request }

```http
DELETE /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1
```

<a id="request-header"></a>
#### リクエストヘッダ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

<a id="request-parameter"></a>
#### リクエストパラメータ

| 名前                  | 区分        | タイプ    | 必須 | 説明             |
|---------------------|-----------|--------|----|----------------|
| bucket              | Path      | String | Y  | バケット名          |
| objectKey           | Path      | String | Y  | オブジェクト名        |
| x-amz-storage-class | Header    | String | N  | ストレージクラス       |
| uploadId            | Parameter | String | Y  | マルチパートアップロードID |

<a id="response"></a>
### レスポンス { #response }

```http
HTTP/1.1 204 No Content
```

<a id="completemultipartupload"></a>
## CompleteMultipartUpload { #completemultipartupload }

アップロードされたパートを組み合わせてオブジェクトを保存し、マルチパートアップロードを完了します。

<a id="completemultipartupload-request"></a>
### リクエスト { #completemultipartupload-request }

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

<a id="completemultipartupload-request-request-header"></a>
#### リクエストヘッダ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

| 名前                       | 必須 | 説明                                                                                                                                                       |
|--------------------------|-------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-amz-checksum-crc32     | N     | データ整合性検証のためのヘッダ。オブジェクトの32ビット`CRC32`チェックサム値をBase64でエンコードした値                                                                                              |
| x-amz-checksum-crc32c    | N     | データ整合性検証のためのヘッダ。オブジェクトの32ビット`CRC32C`チェックサム値をBase64でエンコードした値                                                                                             |
| x-amz-checksum-crc64nvme | N     | データ整合性検証のためのヘッダ。オブジェクトの64ビット`CRC64NVME`チェックサム値をBase64でエンコードした値                                                                                            |
| x-amz-checksum-sha1      | N     | データ整合性検証のためのヘッダ。オブジェクトの160ビット`SHA1`ダイジェスト値をBase64でエンコードした値                                                                                             |
| x-amz-checksum-sha256    | N     | データ整合性検証のためのヘッダ。オブジェクトの256ビット`SHA256`ダイジェスト値をBase64でエンコードした値                                                                                            |
| x-amz-checksum-sha512    | N     | データ整合性検証のためのヘッダ。オブジェクトの512ビット`SHA512`ダイジェスト値をBase64でエンコードした値                                                                                            |
| x-amz-checksum-md5       | N     | データ整合性検証のためのヘッダ。オブジェクトの128ビット`MD5`ダイジェスト値をBase64でエンコードした値                                                                                              |
| x-amz-checksum-xxhash64  | N     | データ整合性検証のためのヘッダ。オブジェクトの64ビット`XXHASH64`チェックサム値をBase64でエンコードした値                                                                                             |
| x-amz-checksum-xxhash3   | N     | データ整合性検証のためのヘッダ。オブジェクトの64ビット`XXHASH3`チェックサム値をBase64でエンコードした値                                                                                              |
| x-amz-checksum-xxhash128 | N     | データ整合性検証のためのヘッダ。オブジェクトの128ビット`XXHASH128`チェックサム値をBase64でエンコードした値                                                                                             |
| x-amz-checksum-type      | N     | マルチパートアップロードでパート別のチェックサムを結合してオブジェクトレベルのチェックサムを生成する方式。`CreateMultipartUpload`リクエストで指定したチェックサムタイプと一致しない場合、`BadDigest`エラーが返されます。有効な値: `COMPOSITE \| FULL_OBJECT` |

<a id="completemultipartupload-request-request-parameter"></a>
#### リクエストパラメータ

| 名前                  | 区分        | タイプ    | 必須 | 説明             |
|---------------------|-----------|--------|----|----------------|
| bucket              | Path      | String | Y  | バケット名          |
| objectKey           | Path      | String | Y  | オブジェクト名        |
| x-amz-storage-class | Header    | String | N  | ストレージクラス       |
| uploadId            | Parameter | String | Y  | マルチパートアップロードID |

<a id="completemultipartupload-request-request-body"></a>
#### リクエストボディ

| 名前                                             | タイプ     | 必須 | 説明                                          |
|------------------------------------------------|---------|----|---------------------------------------------|
| CompleteMultipartUpload                        | Object  | Y  | マルチパートアップロード完了リクエスト                         |
| CompleteMultipartUpload.Part                   | Object  | N  | パート一覧                                       |
| CompleteMultipartUpload.Part.PartNumber        | Integer | N  | パート番号                                       |
| CompleteMultipartUpload.Part.ETag              | String  | N  | オブジェクト固有識別子                                 |
| CompleteMultipartUpload.Part.ChecksumCRC32     | String  | N  | パートの32ビット`CRC32`チェックサム値をBase64でエンコードした値      |
| CompleteMultipartUpload.Part.ChecksumCRC32C    | String  | N  | パートの32ビット`CRC32C`チェックサム値をBase64でエンコードした値     |
| CompleteMultipartUpload.Part.ChecksumCRC64NVME | String  | N  | パートの64ビット`CRC64NVME`チェックサム値をBase64でエンコードした値  |
| CompleteMultipartUpload.Part.ChecksumSHA1      | String  | N  | パートの160ビット`SHA1`チェックサム値をBase64でエンコードした値      |
| CompleteMultipartUpload.Part.ChecksumSHA256    | String  | N  | パートの256ビット`SHA256`チェックサム値をBase64でエンコードした値    |
| CompleteMultipartUpload.Part.ChecksumSHA512    | String  | N  | パートの512ビット`SHA512`チェックサム値をBase64でエンコードした値    |
| CompleteMultipartUpload.Part.ChecksumMD5       | String  | N  | パートの128ビット`MD5`チェックサム値をBase64でエンコードした値       |
| CompleteMultipartUpload.Part.ChecksumXXHASH64  | String  | N  | パートの64ビット`XXHASH64`チェックサム値をBase64でエンコードした値   |
| CompleteMultipartUpload.Part.ChecksumXXHASH3   | String  | N  | パートの64ビット`XXHASH3`チェックサム値をBase64でエンコードした値    |
| CompleteMultipartUpload.Part.ChecksumXXHASH128 | String  | N  | パートの128ビット`XXHASH128`チェックサム値をBase64でエンコードした値 |

<a id="completemultipartupload-response"></a>
### レスポンス { #completemultipartupload-response }

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

<a id="completemultipartupload-response-response-body"></a>
#### レスポンス本文

| 名前                                              | タイプ    | 説明                                          |
|-------------------------------------------------|--------|---------------------------------------------|
| CompleteMultipartUploadResult                   | Object | マルチパートアップロード完了結果                            |
| CompleteMultipartUploadResult.Location          | String | 作成されたオブジェクトパス                               |
| CompleteMultipartUploadResult.Bucket            | String | 対象バケット名                                     |
| CompleteMultipartUploadResult.Key               | String | 作成されたオブジェクトキー                               |
| CompleteMultipartUploadResult.ETag              | String | 最終的に結合されたオブジェクト固有識別子                        |
| CompleteMultipartUploadResult.ChecksumCRC32     | String | パートの32ビット`CRC32`チェックサム値をBase64でエンコードした値      |
| CompleteMultipartUploadResult.ChecksumCRC32C    | String | パートの32ビット`CRC32C`チェックサム値をBase64でエンコードした値     |
| CompleteMultipartUploadResult.ChecksumCRC64NVME | String | パートの64ビット`CRC64NVME`チェックサム値をBase64でエンコードした値  |
| CompleteMultipartUploadResult.ChecksumSHA1      | String | パートの160ビット`SHA1`チェックサム値をBase64でエンコードした値      |
| CompleteMultipartUploadResult.ChecksumSHA256    | String | パートの256ビット`SHA256`チェックサム値をBase64でエンコードした値    |
| CompleteMultipartUploadResult.ChecksumSHA512    | String | パートの512ビット`SHA512`チェックサム値をBase64でエンコードした値    |
| CompleteMultipartUploadResult.ChecksumMD5       | String | パートの128ビット`MD5`チェックサム値をBase64でエンコードした値       |
| CompleteMultipartUploadResult.ChecksumXXHASH64  | String | パートの64ビット`XXHASH64`チェックサム値をBase64でエンコードした値   |
| CompleteMultipartUploadResult.ChecksumXXHASH3   | String | パートの64ビット`XXHASH3`チェックサム値をBase64でエンコードした値    |
| CompleteMultipartUploadResult.ChecksumXXHASH128 | String | パートの128ビット`XXHASH128`チェックサム値をBase64でエンコードした値 |


<a id="createmultipartupload"></a>
## CreateMultipartUpload { #createmultipartupload }

大容量のオブジェクトをアップロードできるように、マルチパートアップロードを開始してアップロードIDを生成します。アップロードIDは最大1時間有効です。

<a id="createmultipartupload-request"></a>
### リクエスト { #createmultipartupload-request }

```http
POST /{bucket}/{objectKey}?uploads HTTP/1.1
x-amz-checksum-algorithm: ChecksumAlgorithm
x-amz-checksum-type: ChecksumType
```

<a id="createmultipartupload-request-request-header"></a>
#### リクエストヘッダ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

| フィールド                 | 必須 | 説明                                                                                                                                                                                             |
|--------------------------|-------|----------------------------------------------------------------------------------------------------------------------------------------|
| x-amz-checksum-algorithm | N     | オブジェクトのチェックサム生成に使用するアルゴリズムを指定。有効な値: `CRC32 \| CRC32C \| SHA1 \| SHA256 \| CRC64NVME \| SHA512 \| MD5 \| XXHASH64 \| XXHASH3 \| XXHASH128` |
| x-amz-checksum-type      | N     | オブジェクトのチェックサム値を計算する方式を指定。有効な値: `COMPOSITE \| FULL_OBJECT`                                                                                                |

<a id="createmultipartupload-request-request-parameter"></a>
#### リクエストパラメータ

| 名前                  | 区分     | タイプ    | 必須 | 説明              |
|---------------------|--------|--------|----|-----------------|
| bucket              | Path   | String | Y  | バケット名           |
| objectKey           | Path   | String | Y  | オブジェクト名         |
| Content-Type        | Header | String | N  | オブジェクトのコンテンツタイプ |
| x-amz-storage-class | Header | String | N  | ストレージクラス        |
| x-amz-meta-\*       | Header | String | N  | ユーザー定義メタデータ     |

<a id="createmultipartupload-response"></a>
### レスポンス { #createmultipartupload-response }

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

<a id="createmultipartupload-response-response-header"></a>
#### レスポンスヘッダ

| フィールド                 | 説明                     |
|--------------------------|------------------------|
| x-amz-checksum-algorithm | オブジェクトのチェックサム生成に使用したアルゴリズム値 |
| x-amz-checksum-type      | オブジェクトのチェックサム値を計算した方式      |

<a id="createmultipartupload-response-response-body"></a>
#### レスポンス本文

| 名前                                     | タイプ    | 説明               |
|----------------------------------------|--------|------------------|
| InitiateMultipartUploadResult          | Object | マルチパートアップロード開始結果 |
| InitiateMultipartUploadResult.Bucket   | String | 対象バケット名          |
| InitiateMultipartUploadResult.Key      | String | オブジェクトキー         |
| InitiateMultipartUploadResult.UploadId | String | マルチパートアップロードID   |


<a id="listparts"></a>
## ListParts { #listparts }

マルチパートアップロードのパート一覧を照会します。

<a id="listparts-request"></a>
### リクエスト { #listparts-request }

```http
GET /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1
```

<a id="listparts-request-request-header"></a>
#### リクエストヘッダ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

<a id="listparts-request-request-parameter"></a>
#### リクエストパラメータ

| 名前                  | 区分        | タイプ    | 必須 | 説明             |
|---------------------|-----------|--------|----|----------------|
| bucket              | Path      | String | Y  | バケット名          |
| objectKey           | Path      | String | Y  | オブジェクト名        |
| x-amz-storage-class | Header    | String | N  | ストレージクラス       |
| uploadId            | Parameter | String | Y  | マルチパートアップロードID |

<a id="listparts-response"></a>
### レスポンス { #listparts-response }

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

<a id="listparts-response-response-body"></a>
#### レスポンス本文

| 名前                                   | タイプ       | 説明                  |
|--------------------------------------|-----------|---------------------|
| ListPartsResult                      | Object    | パート一覧照会結果           |
| ListPartsResult.Bucket               | String    | バケット名               |
| ListPartsResult.Key                  | String    | オブジェクトキー            |
| ListPartsResult.UploadId             | String    | マルチパートアップロードID      |
| ListPartsResult.StorageClass         | String    | ストレージクラス            |
| ListPartsResult.PartNumberMarker     | Integer   | 照会開始の基準             |
| ListPartsResult.NextPartNumberMarker | Integer   | 次の照会開始の基準           |
| ListPartsResult.MaxParts             | Integer   | 最大パート数              |
| ListPartsResult.IsTruncated          | Boolean   | 次のページがあるかどうか        |
| ListPartsResult.Part                 | Object    | パート情報               |
| ListPartsResult.Part.PartNumber      | Integer   | パート番号               |
| ListPartsResult.Part.LastModified    | Timestamp | 最終更新日時(ISO 8601 形式) |
| ListPartsResult.Part.ETag            | String    | パートのオブジェクト固有識別子     |
| ListPartsResult.Part.Size            | Long      | パートのサイズ(bytes)      |


<a id="uploadpart"></a>
## UploadPart { #uploadpart }

マルチパートアップロードのパートをアップロードします。アップロードする前にCreateMultipartUpload APIを呼び出して、マルチパートアップロードIDを生成する必要があります。

<a id="uploadpart-request"></a>
### リクエスト { #uploadpart-request }

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

<a id="uploadpart-request-request-header"></a>
#### リクエストヘッダ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

| フィールド                 | 必須 | 説明                                         |
|--------------------------|-------|--------------------------------------------|
| Content-Length           | N     | リクエスト本文のサイズ(バイト)。本文のサイズを自動的に決定できない場合に使用 |
| Content-MD5              | N     | パートデータの128ビット`MD5`ダイジェスト値をBase64でエンコードした値 |
| x-amz-checksum-crc32     | N     | パートの32ビット`CRC32`チェックサム値をBase64でエンコードした値     |
| x-amz-checksum-crc32c    | N     | パートの32ビット`CRC32C`チェックサム値をBase64でエンコードした値    |
| x-amz-checksum-crc64nvme | N     | パートの64ビット`CRC64NVME`チェックサム値をBase64でエンコードした値 |
| x-amz-checksum-sha1      | N     | パートの160ビット`SHA1`ダイジェスト値をBase64でエンコードした値   |
| x-amz-checksum-sha256    | N     | パートの256ビット`SHA256`ダイジェスト値をBase64でエンコードした値 |
| x-amz-checksum-sha512    | N     | パートの512ビット`SHA512`ダイジェスト値をBase64でエンコードした値 |
| x-amz-checksum-md5       | N     | パートの128ビット`MD5`ダイジェスト値をBase64でエンコードした値    |
| x-amz-checksum-xxhash64  | N     | パートの64ビット`XXHASH64`チェックサム値をBase64でエンコードした値  |
| x-amz-checksum-xxhash3   | N     | パートの64ビット`XXHASH3`チェックサム値をBase64でエンコードした値   |
| x-amz-checksum-xxhash128 | N     | パートの128ビット`XXHASH128`チェックサム値をBase64でエンコードした値 |

<a id="uploadpart-request-request-parameter"></a>
#### リクエストパラメータ

| 名前                  | 区分        | タイプ     | 必須 | 説明              |
|---------------------|-----------|---------|----|-----------------|
| bucket              | Path      | String  | Y  | バケット名           |
| objectKey           | Path      | String  | Y  | オブジェクト名         |
| x-amz-storage-class | Header    | String  | N  | ストレージクラス        |
| partNumber          | Parameter | Integer | Y  | パート番号(1～10,000) |
| uploadId            | Parameter | String  | Y  | マルチパートアップロードID  |

<a id="uploadpart-request-request-body"></a>
#### リクエストボディ

| 名前   | タイプ    | 必須 | 説明                           |
|------|--------|----|------------------------------|
| Body | Binary | Y  | パートのバイナリデータ。最大5GiBまでアップロード可能 |

<a id="uploadpart-response"></a>
### レスポンス { #uploadpart-response }

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

<a id="uploadpart-response-response-header"></a>
#### レスポンスヘッダ

| フィールド                 | 説明                                          |
|--------------------------|---------------------------------------------|
| ETag                     | アップロードされたパートのエンティティタグ                             |
| x-amz-checksum-crc32     | パートの32ビット`CRC32`チェックサム値をBase64でエンコードした値      |
| x-amz-checksum-crc32c    | パートの32ビット`CRC32C`チェックサム値をBase64でエンコードした値     |
| x-amz-checksum-crc64nvme | パートの64ビット`CRC64NVME`チェックサム値をBase64でエンコードした値  |
| x-amz-checksum-sha1      | パートの160ビット`SHA1`チェックサム値をBase64でエンコードした値      |
| x-amz-checksum-sha256    | パートの256ビット`SHA256`チェックサム値をBase64でエンコードした値    |
| x-amz-checksum-sha512    | パートの512ビット`SHA512`チェックサム値をBase64でエンコードした値    |
| x-amz-checksum-md5       | パートの128ビット`MD5`チェックサム値をBase64でエンコードした値       |
| x-amz-checksum-xxhash64  | パートの64ビット`XXHASH64`チェックサム値をBase64でエンコードした値   |
| x-amz-checksum-xxhash3   | パートの64ビット`XXHASH3`チェックサム値をBase64でエンコードした値    |
| x-amz-checksum-xxhash128 | パートの128ビット`XXHASH128`チェックサム値をBase64でエンコードした値 |
