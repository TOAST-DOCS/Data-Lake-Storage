## DeleteObjects

**Data & Analytics > Data Lake Storage > API Guide > Object > DeleteObjects**

Deletes multiple objects in a single request. Up to 1,000 object keys can be specified per request.

### Request

```http
POST /{bucket}?delete HTTP/1.1

<?xml version="1.0" encoding="UTF-8"?>
<Delete>
  <Object>
    <ETag>string</ETag>
    <Key>string</Key>
    <LastModifiedTime>timestamp</LastModifiedTime>
    <Size>long</Size>
  </Object>
  ...
  <Quiet>Boolean</Quiet>
</Delete>
```

### Request Parameter

For header information commonly used in Data Lake Storage APIs, see the Data Lake Storage [API Request Header Guide](api-guide-common).

| Name | In | Type | Required | Description |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | Bucket name |
| Content-MD5 | Header | String | Y | MD5 hash of the request body (used to verify integrity during transmission) |

### Request Body

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| Delete | Object | Y | Root element of the request |
| Delete.Object | Array | Y | List of objects to delete. Up to 1,000 |
| Delete.Object.ETag | String | N | ETag value of the object. If specified, only objects with a matching ETag are deleted. |
| Delete.Object.Key | String | Y | Key of the object to delete |
| Delete.Object.LastModifiedTime | String | N | Last modified time of the object. If specified, only objects with a matching time are deleted. |
| Delete.Object.Size | Long | N | Size of the object (in bytes). If specified, only objects with a matching size are deleted. |
| Delete.Quiet | Boolean | N | When set to `true`, operates in Quiet mode, and only failed items are included in the response. The default is Verbose mode (returns all results). |

### Response

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<DeleteResult>
  <Deleted>
    <Key>String</Key>
  </Deleted>
  ...
  <Error>
    <Key>String</Key>
    <Code>String</Code>
    <Message>String</Message>
  </Error>
  ...
</DeleteResult>
```

| Name | Type | Description |
| --- | --- | --- |
| DeleteResult | Object | Root element of the deletion result |
| DeleteResult.Deleted | Array | List of successfully deleted objects |
| DeleteResult.Deleted.Key | String | Key of the deleted object |
| DeleteResult.Error | Array | List of objects that failed to delete |
| DeleteResult.Error.Key | String | Key of the object that failed to delete |
| DeleteResult.Error.Code | String | Error code |
| DeleteResult.Error.Message | String | Error message |