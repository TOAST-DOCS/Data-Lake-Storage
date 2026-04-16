## AbortMultipartUpload

**Data & Analytics > Data Lake Storage > API ガイド > Multipart > AbortMultipartUpload**

進行中のマルチパートアップロードを中断します。

### リクエスト

```http
DELETE /{bucket}/{objectKey}?uploadId={uploadId} HTTP/1.1
```

### リクエストパラメータ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](https://docs.beta-nhncloud.com/ja/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/)をご参照ください。

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
