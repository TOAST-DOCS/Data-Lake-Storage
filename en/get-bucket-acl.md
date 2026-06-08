## GetBucketAcl

**Data & Analytics > Data Lake Storage > API Guide > Bucket > GetBucketAcl**

Retrieves the access control list (ACL) of a bucket.

### Request

```http
GET /{bucket}?acl HTTP/1.1
```

### Request Parameter

For header information commonly used in Data Lake Storage APIs, see the Data Lake Storage [API Request Header Guide](api-guide-common).

| Name | In | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |

### Response

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

| Name | Type | Description |
| --- | --- | --- |
| AccessControlPolicy | Object | Root element of the bucket ACL retrieval result |
| AccessControlPolicy.Owner | Object | Bucket owner information |
| AccessControlPolicy.Owner.DisplayName | String | Bucket owner name |
| AccessControlPolicy.Owner.ID | String | Bucket owner ID |
| AccessControlPolicy.AccessControlList | Object | List of grants |
| AccessControlPolicy.AccessControlList.Grant | Array | Individual grant entries |
| AccessControlPolicy.AccessControlList.Grant.Grantee | Object | Information about the grantee |
| AccessControlPolicy.AccessControlList.Grant.Grantee.xsi:type | String | Type of the grantee. Only `CanonicalUser` is supported. |
| AccessControlPolicy.AccessControlList.Grant.Grantee.DisplayName | String | Name of the grantee |
| AccessControlPolicy.AccessControlList.Grant.Grantee.ID | String | ID of the grantee |
| AccessControlPolicy.AccessControlList.Grant.Permission | String | Permission granted. Only `FULL_CONTROL` is supported. |