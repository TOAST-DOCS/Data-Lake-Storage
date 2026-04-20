## ListParts

**Data & Analytics > Data Lake Storage > API ガイド > Multipart > ListParts**

マルチパートアップロードのパート一覧を照会します。

### リクエスト

```http
GET /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1
```

### リクエストパラメータ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

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
