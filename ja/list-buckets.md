## ListBuckets

**Data & Analytics > Data Lake Storage > API ガイド > Bucket > ListBuckets**

バケット一覧を照会します。

### リクエスト

```http
GET /?max-buckets=20 HTTP/1.1
```

### リクエストパラメータ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](https://docs.beta-nhncloud.com/ja/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/)をご参照ください。

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| continuation-token | Parameter | String | N | 次のページを照会するための連続トークン |
| max-buckets | Parameter | Integer | N | 返却する最大バケット数 |
| prefix | Parameter | String | N | バケット名フィルタリングのプレフィックス |

### レスポンス

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<ListAllMyBucketsResult>
  <Buckets>
    <Bucket>
      <BucketRegion>string</BucketRegion>
      <CreationDate>timestamp</CreationDate>
      <Name>string</Name>
    </Bucket>
  </Buckets>
  <Owner>
    <DisplayName>string</DisplayName>
    <ID>string</ID>
  </Owner>
  <ContinuationToken>string</ContinuationToken>
  <Prefix></Prefix>
</ListAllMyBucketsResult>
```

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| ListAllMyBuckets | Object | バケット一覧照会結果 |
| ListAllMyBuckets.Buckets | Object | バケット情報 |
| ListAllMyBuckets.Buckets.BucketRegion | String | バケットが配置されているリージョン |
| ListAllMyBuckets.Buckets.CreationDate | Timestamp | バケット作成日時 (ISO 8601) |
| ListAllMyBuckets.Buckets.Name | String | バケット名 |
| ListAllMyBuckets.Owner | Object | バケットの所有者情報 |
| ListAllMyBuckets.Owner.DisplayName | String | 所有者の表示名 |
| ListAllMyBuckets.Owner.ID | String | 所有者ID |
| ListAllMyBuckets.ContinuationToken | String | 次のページ照会用の連続トークン (最後のページの場合は未記載) |
| ListAllMyBuckets.Prefix | String | リクエストに使用されたプレフィックスフィルタ |
