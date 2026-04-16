## PutObject

**Data & Analytics > Data Lake Storage > API ガイド > Object > PutObject**

バケットにオブジェクトを保存します。

### リクエスト

```http
PUT /{bucket}/{objectKey} HTTP/1.1

Body
```

### リクエストパラメータ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](https://docs.beta-nhncloud.com/ja/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/)をご参照ください。

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |
| objectKey | Path | String | Y | オブジェクト名 |
| Content-Type | Header | String | N | オブジェクトのコンテンツタイプ |
| Content-Length | Header | Long | Y | オブジェクトのサイズ(bytes) |
| x-amz-storage-class | Header | String | N | ストレージクラス |
| x-amz-meta-\* | Header | String | N | ユーザー定義メタデータ |

### リクエストボディ

| 名前 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| Body | Binary | Y | オブジェクトデータ |


### レスポンス

```http
HTTP/1.1 200 OK
ETag: String
```
