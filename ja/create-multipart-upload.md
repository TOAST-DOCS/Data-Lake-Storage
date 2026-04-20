## CreateMultipartUpload

**Data & Analytics > Data Lake Storage > API ガイド > Multipart > CreateMultipartUpload**

大容量のオブジェクトをアップロードできるように、マルチパートアップロードを開始してアップロードIDを生成します。アップロードIDは最大1時間有効です。

### リクエスト

```http
POST /{bucket}/{objectKey}?uploads HTTP/1.1
```

### リクエストパラメータ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](https://docs.beta-nhncloud.com/ja/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/)をご参照ください。

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

<?xml version="1.0" encoding="UTF-8"?>
<InitiateMultipartUploadResult>
  <Bucket>String</Bucket>
  <Key>String</Key>
  <UploadId>String</UploadId>
</InitiateMultipartUploadResult>
```

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| InitiateMultipartUploadResult | Object | マルチパートアップロード開始結果 |
| InitiateMultipartUploadResult.Bucket | String | 対象バケット名 |
| InitiateMultipartUploadResult.Key | String | オブジェクトキー |
| InitiateMultipartUploadResult.UploadId | String | マルチパートアップロードID |
