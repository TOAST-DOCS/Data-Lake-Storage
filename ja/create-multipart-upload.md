## CreateMultipartUpload

**Data & Analytics > Data Lake Storage > API ガイド > Multipart > CreateMultipartUpload**

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
