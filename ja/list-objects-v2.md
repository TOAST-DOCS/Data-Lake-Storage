## ListObjectsV2

**Data & Analytics > Data Lake Storage > API ガイド > Object > ListObjectsV2**

バケットに保存されたオブジェクト一覧を照会します。

### リクエスト

```http
GET /{bucket}?list-type=2&continuation-token={continuationToken}&delimiter={delimiter}&encoding-type={encodingType}&fetch-owner={fetchOwner}&max-keys={maxKeys}&prefix={prefix}&start-after={startAfter} HTTP/1.1
```

### リクエストパラメータ

Data Lake Storage APIで共通して使用するヘッダ情報は、Data Lake Storage [API リクエストヘッダガイド](https://docs.beta-nhncloud.com/ja/Data%20&%20Analytics/Data%20Lake%20Storage/ko/api-guide-common/)をご参照ください。

| 名前 | 区分 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| bucket | Path | String | Y | バケット名 |
| x-amz-storage-class | Header | String | N | ストレージクラス |
| list-type | Parameter | String | Y | 2に固定（V2 API識別用） |
| continuation-token | Parameter | String | N | 次のページ照会識別子 |
| delimiter | Parameter | String | N | キーグループの区切り文字(デフォルト: /) |
| encoding-type | Parameter | String | N | キーのエンコーディング方式 |
| fetch-owner | Parameter | Boolean | N | 所有者情報を含めるかどうか |
| max-keys | Parameter | Integer | N | 返却する最大オブジェクト数 |
| prefix | Parameter | String | N | オブジェクト名のプレフィックス |
| start-after | Parameter | String | N | 照会開始の基準 |

### レスポンス

```http
HTTP/1.1 200 OK

<?xml version="1.0" encoding="UTF-8"?>
<ListBucketResult>
  <Name>String</Name>
  <Prefix>String/</Prefix>
  <StartAfter>String</StartAfter>
  <ContinuationToken>String</ContinuationToken>
  <NextContinuationToken>String</NextContinuationToken>
  <KeyCount>Integer</KeyCount>
  <MaxKeys>Integer</MaxKeys>
  <Delimiter>String</Delimiter>
  <IsTruncated>Boolean</IsTruncated>
  <EncodingType>String</EncodingType>
  <Contents>
    <Key>String</Key>
    <LastModified>Timestamp</LastModified>
    <ETag>String</ETag>
    <Size>Long</Size>
    <StorageClass>String</StorageClass>
    <Owner>
      <ID>String</ID>
    </Owner>
  </Contents>
  <CommonPrefixes>
    <Prefix>String</Prefix>
  </CommonPrefixes>
</ListBucketResult>
```

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| ListBucketResult | Object | オブジェクト一覧照会結果 |
| ListBucketResult.Name | String | バケット名 |
| ListBucketResult.Prefix | String | リクエスト時に指定したオブジェクト名のプレフィックス |
| ListBucketResult.StartAfter | String | リクエスト時に指定した照会開始の基準 |
| ListBucketResult.ContinuationToken | String | 今回のリクエストに使用されたページ照会識別子 |
| ListBucketResult.NextContinuationToken | String | 次のページをリクエストする際に使用するページ照会識別子 |
| ListBucketResult.KeyCount | Integer | レスポンスのオブジェクト数 |
| ListBucketResult.MaxKeys | Integer | リクエスト時に指定した最大オブジェクト数 |
| ListBucketResult.Delimiter | String | リクエスト時に指定したキーグループの区切り文字 |
| ListBucketResult.IsTruncated | Boolean | 次のページがあるかどうか |
| ListBucketResult.Contents | Array | オブジェクト一覧 |
| ListBucketResult.Contents.Key | String | オブジェクトキー |
| ListBucketResult.Contents.LastModified | Timestamp | 最終更新日時 (ISO 8601 形式) |
| ListBucketResult.Contents.ETag | String | オブジェクト固有識別子 |
| ListBucketResult.Contents.Size | Long | オブジェクトサイズ(bytes) |
| ListBucketResult.Contents.StorageClass | String | ストレージクラス |
| ListBucketResult.Contents.Owner.ID | String | 所有者ID (fetch-owner=true の場合に包含) |
| ListBucketResult.CommonPrefixes | Array | delimiter基準でグループ化された共通のプレフィックス一覧 |
| ListBucketResult.CommonPrefixes.Prefix | String | delimiter基準でグループ化されたパス |
