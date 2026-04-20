## HeadObject

**Data & Analytics > Data Lake Storage > API ガイド > Object > HeadObject**

バケットに保存されたオブジェクトのメタデータを照会します。

### リクエスト

```http
HEAD /{bucket}/{objectKey} HTTP/1.1
```

### リクエストパラメータ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](https://docs.beta-nhncloud.com/ja/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/)をご参照ください。

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
```
