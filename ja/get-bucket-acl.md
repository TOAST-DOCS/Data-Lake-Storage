## GetBucketAcl

**Data & Analytics > Data Lake Storage > API ガイド > Bucket > GetBucketAcl**

バケットのアクセス制御リスト(ACL)を照会します。

### リクエスト

```http
GET /{bucket}?acl HTTP/1.1
```

### リクエストパラメータ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |

### レスポンス

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<AccessControlPolicy>
  <Owner>
    <DisplayName>String</DisplayName>
    <ID>String</ID>
  </Owner>
  <AccessControlList>
    <Grant>
      <Grantee>
        <xsi:type>string</xsi:type>
        <DisplayName>String</DisplayName>
        <ID>String</ID>
      </Grantee>
      <Permission>String</Permission>
    </Grant>
  </AccessControlList>
</AccessControlPolicy>
```

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| AccessControlPolicy | Object | バケットACL照会結果のルート要素 |
| AccessControlPolicy.Owner | Object | バケット所有者情報 |
| AccessControlPolicy.Owner.DisplayName | String | バケット所有者名 |
| AccessControlPolicy.Owner.ID | String | バケット所有者ID |
| AccessControlPolicy.AccessControlList | Object | 権限付与一覧 |
| AccessControlPolicy.AccessControlList.Grant | Array | 個別の権限付与項目 |
| AccessControlPolicy.AccessControlList.Grant.Grantee | Object | 権限を付与された対象の情報 |
| AccessControlPolicy.AccessControlList.Grant.Grantee.xsi:type | String | 権限を付与された対象のタイプ。`CanonicalUser`のみサポート |
| AccessControlPolicy.AccessControlList.Grant.Grantee.DisplayName | String | 権限を付与された対象名 |
| AccessControlPolicy.AccessControlList.Grant.Grantee.ID | String | 権限を付与された対象ID |
| AccessControlPolicy.AccessControlList.Grant.Permission | String | 付与された権限。`FULL_CONTROL`のみサポート |
