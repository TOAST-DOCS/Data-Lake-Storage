## CompleteMultipartUpload

**Data & Analytics > Data Lake Storage > API ガイド > Multipart > CompleteMultipartUpload**

アップロードされたパートを組み合わせてオブジェクトを保存し、マルチパートアップロードを完了します。

### リクエスト

```http
POST /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1

<?xml version="1.0" encoding="UTF-8"?>
<CompleteMultipartUpload xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
   <Part>
     <PartNumber>Integer</PartNumber>
     <ETag>String</ETag>
   </Part>
</CompleteMultipartUpload>
```

### リクエストパラメータ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |
| objectKey | Path | String | Y | オブジェクト名 |
| x-amz-storage-class | Header | String | N | ストレージクラス |
| uploadId | Parameter | String | Y | マルチパートアップロードID |

### リクエストボディ
| 名前 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| CompleteMultipartUpload | Object | Y | マルチパートアップロード完了リクエスト |
| CompleteMultipartUpload.Part | Object | N | パート一覧 |
| CompleteMultipartUpload.Part.PartNumber | Integer | N | パート番号 |
| CompleteMultipartUpload.Part.ETag | String | N | オブジェクト固有識別子 |

### レスポンス

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<CompleteMultipartUploadResult>
  <Location>String</Location>
  <Bucket>String</Bucket>
  <Key>String</Key>
  <ETag>String</ETag>
</CompleteMultipartUploadResult>
```

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| CompleteMultipartUploadResult | Object | マルチパートアップロード完了結果 |
| CompleteMultipartUploadResult.Location | String | 作成されたオブジェクトパス |
| CompleteMultipartUploadResult.Bucket | String | 対象バケット名 |
| CompleteMultipartUploadResult.Key | String | 作成されたオブジェクトキー |
| CompleteMultipartUploadResult.ETag | String | 最終的に結合されたオブジェクト固有識別子 |
