<!-- pre-align:aligned sig=b6666f81ba17 -->

<a id="bucket"></a>
## Bucket { #bucket }
**Data & Analytics > Data Lake Storage > Amazon S3互換APIガイド > Bucket**


<a id="createbucket"></a>
## CreateBucket { #createbucket }

バケットを作成します。Data Lake Storageサービスのバケット名はリージョン内で一意です。

<a id="request"></a>
### リクエスト { #request }

```http
PUT /{bucket} HTTP/1.1

<?xml version="1.0" encoding="UTF-8"?>
<CreateBucketConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
   <LocationConstraint>KR3</LocationConstraint>
</CreateBucketConfiguration>
```

<a id="request-header"></a>
#### リクエストヘッダ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

<a id="request-parameter"></a>
#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |

<a id="request-body"></a>
#### リクエストボディ

| 名前 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| CreateBucketConfiguration | Object | Y | 作成するバケット情報 |
| CreateBucketConfiguration.LocationConstraint | String | Conditional | リージョンコード。未入力時はエンドポイントに合ったリージョン情報を適用 |

<a id="response"></a>
### レスポンス { #response }

```http
HTTP/1.1 200 OK
Location: "/{bucket}"
```


<a id="deletebucket"></a>
## DeleteBucket { #deletebucket }

バケットを削除します。

<a id="deletebucket-request"></a>
### リクエスト { #deletebucket-request }

```http
DELETE /{bucket} HTTP/1.1
```

<a id="deletebucket-request-request-header"></a>
#### リクエストヘッダ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

<a id="deletebucket-request-request-parameter"></a>
#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |

<a id="deletebucket-response"></a>
### レスポンス { #deletebucket-response }

```http
HTTP/1.1 204 No Content
```


<a id="deletebucketpolicy"></a>
## DeleteBucketPolicy { #deletebucketpolicy }

バケットに登録されたポリシー(Bucket Policy)を削除します。

<a id="deletebucketpolicy-request"></a>
### リクエスト { #deletebucketpolicy-request }

```http
DELETE /{bucket}?policy HTTP/1.1
```

<a id="deletebucketpolicy-request-request-header"></a>
#### リクエストヘッダ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storageの[APIリクエストヘッダガイド](api-guide-common)をご参照ください。

<a id="deletebucketpolicy-request-request-parameter"></a>
#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |

<a id="deletebucketpolicy-response"></a>
### レスポンス { #deletebucketpolicy-response }

```http
HTTP/1.1 204 No Content
```


<a id="getbucketacl"></a>
## GetBucketAcl { #getbucketacl }

バケットのアクセス制御リスト(ACL)を照会します。

<a id="getbucketacl-request"></a>
### リクエスト { #getbucketacl-request }

```http
GET /{bucket}?acl HTTP/1.1
```

<a id="getbucketacl-request-request-header"></a>
#### リクエストヘッダ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

<a id="getbucketacl-request-request-parameter"></a>
#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |

<a id="getbucketacl-response"></a>
### レスポンス { #getbucketacl-response }

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

<a id="getbucketacl-response-response-body"></a>
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


<a id="getbucketpolicy"></a>
## GetBucketPolicy { #getbucketpolicy }

バケットに登録されたポリシー(Bucket Policy)を照会します。ポリシードキュメントがJSON形式のレスポンス本文としてそのまま返されます。

<a id="getbucketpolicy-request"></a>
### リクエスト { #getbucketpolicy-request }

```http
GET /{bucket}?policy HTTP/1.1
```

<a id="getbucketpolicy-request-request-header"></a>
#### リクエストヘッダ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storageの[APIリクエストヘッダガイド](api-guide-common)をご参照ください。

<a id="getbucketpolicy-request-request-parameter"></a>
#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |

<a id="getbucketpolicy-response"></a>
### レスポンス { #getbucketpolicy-response }

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


<a id="listbuckets"></a>
## ListBuckets { #listbuckets }

バケット一覧を照会します。

<a id="listbuckets-request"></a>
### リクエスト { #listbuckets-request }

```http
GET /?max-buckets=20 HTTP/1.1
```

<a id="listbuckets-request-request-header"></a>
#### リクエストヘッダ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](api-guide-common)をご参照ください。

<a id="listbuckets-request-request-parameter"></a>
#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| continuation-token | Parameter | String | N | 次のページを照会するための連続トークン |
| max-buckets | Parameter | Integer | N | 返却する最大バケット数 |
| prefix | Parameter | String | N | バケット名フィルタリングのプレフィックス |

<a id="listbuckets-response"></a>
### レスポンス { #listbuckets-response }

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

<a id="listbuckets-response-response-body"></a>
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


<a id="putbucketpolicy"></a>
## PutBucketPolicy { #putbucketpolicy }

バケットにポリシー(Bucket Policy)を登録するか、既存のポリシーを置き換えます。バケットポリシーはJSON形式のポリシードキュメントであり、どの主体(Principal)がどのリソース(Resource)のどのアクション(Action)を許可または拒否するかを定義します。

<a id="putbucketpolicy-request"></a>
### リクエスト { #putbucketpolicy-request }

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

<a id="putbucketpolicy-request-request-header"></a>
#### リクエストヘッダ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storageの[APIリクエストヘッダガイド](api-guide-common)をご参照ください。

<a id="putbucketpolicy-request-request-parameters"></a>
#### リクエストパラメータ

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |

<a id="putbucketpolicy-request-request-body"></a>
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

<a id="putbucketpolicy-request-principal"></a>
#### Principal

権限を適用する主体を指定します。

| 形式 | 説明 |
| --- | --- |
| `"*"` | 全てのユーザー(匿名を含む) |
| `{ "NHN": "arn:nhn:cloud:iam:{appKey}:user/{memberUuid}" }` | 特定のNHN Cloud IAMユーザー。`appKey`は16桁の英数字、`memberUuid`はUUID形式です。 |
| `{ "NHN": "arn:nhn:cloud:iam:{appKey}:user/*" }` | プロジェクト(appKey)に属する全てのユーザー |

!!! tip "ポイント"
    `Principal`には単一の文字列または文字列の配列をいずれも使用できます。

<a id="putbucketpolicy-request-action"></a>
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

<a id="putbucketpolicy-request-resource"></a>
#### Resource

リソースはARNではなくバケット内のパス形式で指定します。

| 形式 | 説明 |
| --- | --- |
| `*` | バケット内の全てのオブジェクト |
| `curated/*` | `curated/`プレフィックスを持つ全てのオブジェクト |
| `report.csv` | 特定のオブジェクトキー |

!!! danger "注意"
    リソースは`arn:`で始まるか、`/`で始まることはできません。

<a id="putbucketpolicy-request-request-example"></a>
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

<a id="putbucketpolicy-response"></a>
### レスポンス { #putbucketpolicy-response }

```http
HTTP/1.1 204 No Content
```
