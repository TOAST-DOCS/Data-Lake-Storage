## DeleteObjects

**Data & Analytics > Data Lake Storage > API ガイド > Object > DeleteObjects**

1つのリクエストで複数のオブジェクトを削除します。1回のリクエストで最大1,000個のオブジェクトキーを指定できます。

### リクエスト

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

#### 요청 헤더

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |
| Content-MD5 | Header | String | Y | リクエスト本文のMD5ハッシュ値(送信中の改ざん検証用) |

#### リクエスト本文

| 名前 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| Delete | Object | Y | リクエストのルート要素 |
| Delete.Object | Array | Y | 削除するオブジェクト一覧。最大1,000個 |
| Delete.Object.ETag | String | N | オブジェクトのETag値。指定した場合、ETagが一致するオブジェクトのみ削除します。 |
| Delete.Object.Key | String | Y | 削除するオブジェクトキー |
| Delete.Object.LastModifiedTime | String | N | オブジェクトの最終変更時間。指定した場合、該当時間と一致するオブジェクトのみ削除します。 |
| Delete.Object.Size | Long | N | オブジェクトのサイズ(バイト)。指定した場合、該当サイズと一致するオブジェクトのみ削除します。 |
| Delete.Quiet | Boolean | N | `true`に設定するとQuietモードで動作し、失敗した項目のみレスポンスに含まれます。デフォルト値はVerboseモード(全ての結果を返す)です。 |

### レスポンス

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

#### 응답 본문

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| DeleteResult | Object | 削除結果のルート要素 |
| DeleteResult.Deleted | Array | 削除に成功したオブジェクト一覧 |
| DeleteResult.Deleted.Key | String | 削除されたオブジェクトキー |
| DeleteResult.Error | Array | 削除に失敗したオブジェクト一覧 |
| DeleteResult.Error.Key | String | 削除に失敗したオブジェクトキー |
| DeleteResult.Error.Code | String | エラーコード |
| DeleteResult.Error.Message | String | エラーメッセージ |
