## CreateBucket

**Data & Analytics > Data Lake Storage > API ガイド > Bucket > CreateBucket**

バケットを作成します。Data Lake Storageサービスのバケット名はリージョン内で一意です。

### リクエスト

```http
PUT /{bucket} HTTP/1.1

<?xml version="1.0" encoding="UTF-8"?>
<CreateBucketConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
   <LocationConstraint>KR3</LocationConstraint>
</CreateBucketConfiguration>
```

### リクエストパラメータ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |

### リクエストボディ

| 名前 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| CreateBucketConfiguration | Object | Y | 作成するバケット情報 |
| CreateBucketConfiguration.LocationConstraint | String | Conditional | リージョンコード。未入力時はエンドポイントに合ったリージョン情報を適用 |

### レスポンス

```http
HTTP/1.1 200 OK
Location: "/{bucket}"
```
