## Bucket
**Data & Analytics > Data Lake Storage > Amazon S3互換APIガイド > Bucket**


## CreateBucket

バケットを作成します。Data Lake Storageサービスのバケット名はリージョン内で一意です。

### リクエスト

```http
PUT /{bucket} HTTP/1.1

<?xml version="1.0" encoding="UTF-8"?>
<CreateBucketConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
   <LocationConstraint>KR3</LocationConstraint>
</CreateBucketConfiguration>
```

#### リクエストヘッダ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |

#### リクエストボディ

| 名前 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| CreateBucketConfiguration | Object | Y | 作成するバケット情報 |
| CreateBucketConfiguration.LocationConstraint | String | Conditional | リージョンコード。未入力時はエンドポイントに合ったリージョン情報を適用 |

### レスポンス

```http
HTTP/1.1 200 OK
Location: "/{bucket}"
```


## DeleteBucket

バケットを削除します。

### リクエスト

```http
DELETE /{bucket} HTTP/1.1
```

#### リクエストヘッダ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |

### レスポンス

```http
HTTP/1.1 204 No Content
```


## DeleteBucketPolicy

バケットに登録されたポリシー(Bucket Policy)を削除します。

### リクエスト

```http
DELETE /{bucket}?policy HTTP/1.1
```

#### リクエストヘッダ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storageの[APIリクエストヘッダガイド](api-guide-common)をご参照ください。

#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |

### レスポンス

```http
HTTP/1.1 204 No Content
```


## GetBucketAcl

バケットのアクセス制御リスト(ACL)を照会します。

### リクエスト

```http
GET /{bucket}?acl HTTP/1.1
```

#### リクエストヘッダ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

#### リクエストパラメータ

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

#### レスポンス本文

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


## GetBucketPolicy

バケットに登録されたポリシー(Bucket Policy)を照会します。ポリシードキュメントがJSON形式のレスポンス本文としてそのまま返されます。

### リクエスト

```http
GET /{bucket}?policy HTTP/1.1
```

#### リクエストヘッダ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storageの[APIリクエストヘッダガイド](api-guide-common)をご参照ください。

#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |

### レスポンス

登録されたポリシードキュメントがJSON本文として返されます。ポリシードキュメントの各フィールドに関する説明は、[PutBucketPolicy](#putbucketpolicy)をご参照ください。

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ProjectMembersFullObjectAccess",
      "Effect": "Allow",
      "Principal": { "NHN": "arn:nhn:cloud:iam:{appKey}:user/*" },
      "Action": ["s3:GetObject", "s3:PutObject", "s3:DeleteObject", "s3:ListBucket"],
      "Resource": ["*"]
    }
  ]
}
```

!!! tip "ポイント"
    バケットにポリシーが登録されていない場合、`404 NoSuchBucketPolicy`エラーが返されます。


## ListBuckets

バケット一覧を照会します。

### リクエスト

```http
GET /?max-buckets=20 HTTP/1.1
```

#### リクエストヘッダ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

#### リクエストパラメータ

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

#### レスポンス本文

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| ListAllMyBucketsResult | Object | バケット一覧照会結果 |
| ListAllMyBucketsResult.Buckets | Object | バケット情報 |
| ListAllMyBucketsResult.Buckets.BucketRegion | String | バケットが配置されているリージョン |
| ListAllMyBucketsResult.Buckets.CreationDate | Timestamp | バケット作成日時 (ISO 8601) |
| ListAllMyBucketsResult.Buckets.Name | String | バケット名 |
| ListAllMyBucketsResult.Owner | Object | バケットの所有者情報 |
| ListAllMyBucketsResult.Owner.DisplayName | String | 所有者の表示名 |
| ListAllMyBucketsResult.Owner.ID | String | 所有者ID |
| ListAllMyBucketsResult.ContinuationToken | String | 次のページ照会用の連続トークン (最後のページの場合は未記載) |
| ListAllMyBucketsResult.Prefix | String | リクエストに使用されたプレフィックスフィルタ |


## PutBucketPolicy

バケットにポリシー(Bucket Policy)を登録するか、既存のポリシーを置き換えます。バケットポリシーはJSON形式のポリシードキュメントであり、どの主体(Principal)がどのリソース(Resource)のどのアクション(Action)を許可または拒否するかを定義します。

### リクエスト

```http
PUT /{bucket}?policy HTTP/1.1

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ExampleStatement",
      "Effect": "Allow",
      "Principal": { "NHN": "arn:nhn:cloud:iam:{appKey}:user/{memberUuid}" },
      "Action": ["s3:GetObject", "s3:PutObject"],
      "Resource": ["*"]
    }
  ]
}
```

#### リクエストヘッダ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storageの[APIリクエストヘッダガイド](api-guide-common)をご参照ください。

#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |

#### リクエスト本文

リクエスト本文は、ポリシードキュメント全体を含んだJSONです。

| 名前 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| Version | String | Y | ポリシー言語のバージョン。`2012-10-17`を使用します。 |
| Id | String | N | ポリシー識別子 |
| Statement | Array | Y | ポリシーステートメント一覧。少なくとも1つ以上である必要があります。 |
| Statement.Sid | String | N | ポリシーステートメント識別子 |
| Statement.Effect | String | Y | 権限の適用方式。`Allow`または`Deny` |
| Statement.Principal | Object | Y | 権限を適用する主体。`NotPrincipal`と一緒には使用できません。 |
| Statement.NotPrincipal | Object | N | 権限の適用から除外する主体。`Principal`と一緒には使用できません。 |
| Statement.Action | Array | Y | 適用するアクション一覧。`NotAction`と一緒には使用できません。 |
| Statement.NotAction | Array | N | 適用から除外するアクション一覧。`Action`と一緒には使用できません。 |
| Statement.Resource | Array | Y | アクション対象のリソース一覧。`NotResource`と一緒には使用できません。 |
| Statement.NotResource | Array | N | 適用から除外するリソース一覧。`Resource`と一緒には使用できません。 |
| Statement.Condition | Object | N | ポリシーステートメントが適用される条件 |

#### Principal

権限を適用する主体を指定します。

| 形式 | 説明 |
| --- | --- |
| `"*"` | 全てのユーザー(匿名を含む) |
| `{ "NHN": "arn:nhn:cloud:iam:{appKey}:user/{memberUuid}" }` | 特定のNHN Cloud IAMユーザー。`appKey`は16桁の英数字、`memberUuid`はUUID形式です。 |
| `{ "NHN": "arn:nhn:cloud:iam:{appKey}:user/*" }` | プロジェクト(appKey)に属する全てのユーザー |

!!! tip "ポイント"
    `Principal`には単一の文字列または文字列の配列をいずれも使用できます。

#### Action

ポリシーで使用できるアクションは次のとおりです。

| アクション | 説明 |
| --- | --- |
| s3:GetObject | オブジェクトの照会 |
| s3:PutObject | オブジェクトのアップロード |
| s3:DeleteObject | オブジェクトの削除 |
| s3:GetObjectAcl | オブジェクトACLの照会 |
| s3:PutObjectAcl | オブジェクトACLの設定 |
| s3:ListBucket | バケット内のオブジェクト一覧照会 |
| s3:ListBucketMultipartUploads | 進行中のマルチパートアップロード一覧照会 |
| s3:ListMultipartUploadParts | マルチパートアップロードのパート一覧照会 |
| s3:AbortMultipartUpload | マルチパートアップロードの中断 |
| s3:* | 上記のアクションを含む全てのアクション |

!!! danger "注意"
    バケットの作成/削除、バケットポリシー及びACL管理など、バケット管理(Bucket Administration)アクションはバケットポリシーで付与できません。該当する権限はプロジェクト/バケット所有者ロールでのみ実行できます。

#### Resource

リソースはARNではなくバケット内のパス形式で指定します。

| 形式 | 説明 |
| --- | --- |
| `*` | バケット内の全てのオブジェクト |
| `curated/*` | `curated/`プレフィックスを持つ全てのオブジェクト |
| `report.csv` | 特定のオブジェクトキー |

!!! danger "注意"
    リソースは`arn:`で始まるか、`/`で始まることはできません。

#### リクエスト例

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ProjectMembersFullObjectAccess",
      "Effect": "Allow",
      "Principal": { "NHN": "arn:nhn:cloud:iam:{appKey}:user/*" },
      "Action": ["s3:GetObject", "s3:PutObject", "s3:DeleteObject", "s3:ListBucket"],
      "Resource": ["*"]
    }
  ]
}
```

### レスポンス

```http
HTTP/1.1 204 No Content
```
