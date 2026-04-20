## DeleteBucket

**Data & Analytics > Data Lake Storage > API ガイド > Bucket > DeleteBucket**

バケットを削除します。

### リクエスト

```http
DELETE /{bucket} HTTP/1.1
```

### リクエストパラメータ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](https://docs.beta-nhncloud.com/ja/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/)をご参照ください。

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |

### レスポンス

```http
HTTP/1.1 204 No Content
```
