## UploadPart

**Data & Analytics > Data Lake Storage > API ガイド > Multipart > UploadPart**

マルチパートアップロードのパートをアップロードします。アップロードする前にCreateMultipartUpload APIを呼び出して、マルチパートアップロードIDを生成する必要があります。

### リクエスト

```http
PUT /{bucket}/{objectKey}?partNumber={partNumber}&uploadId={uploadId} HTTP/1.1

body
```

### リクエストパラメータ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](https://docs.beta-nhncloud.com/ja/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/)をご参照ください。

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |
| objectKey | Path | String | Y | オブジェクト名 |
| x-amz-storage-class | Header | String | N | ストレージクラス |
| partNumber | Parameter | Integer | Y | パート番号(1 ～ 10,000) |
| uploadId | Parameter | String | Y | マルチパートアップロードID |

### リクエストボディ
| 名前 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| Body | Binary | Y | パートのバイナリデータ。最大5GiBまでアップロード可能 |

### レスポンス

```http
HTTP/1.1 200 OK
```
