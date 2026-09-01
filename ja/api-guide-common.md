<!-- pre-align:aligned sig=bd9519149408 -->

<a id="amazon-s3-compatible-api-guide"></a>
## Amazon S3互換APIガイド { #amazon-s3-compatible-api-guide }

**Data & Analytics > Data Lake Storage > Amazon S3互換APIガイド > 共通**

<a id="common-information-for-data-lake-storage-api"></a>
## Data Lake Storage API 共通情報 { #common-information-for-data-lake-storage-api }

!!! tip 「ポイント」
    NHN Cloud Data Lake Storageサービスは、Amazon S3 API 2006-03-01バージョンと互換性を持つように設計されています。

<a id="api-endpoint"></a>
### API エンドポイント { #api-endpoint }

| リージョン | エンドポイント |
| --- | ----- |
| KR3 | https://kr3-data-lake-storage.nhncloudservice.com |

<a id="authentication-and-authorization"></a>
### 認証及び権限 { #authentication-and-authorization }

Data Lake Storageは、Amazon S3互換APIを呼び出す際の認証/認可にS3 API認証情報を使用します。S3 API認証情報は、NHN CloudでAmazon S3互換APIに対応したサービスAPIを使用するためのAWS EC2形式の認証キーです。
S3 API認証情報の詳細は、[S3 API認証情報](/nhncloud/ja/public-api/s3-api-credential/)を参照してください。コンソール経由の認証情報の照会、発行、修正、削除に関する詳細は、[認証情報の管理](console-user-guide/#manage-credentials)を参照してください。

<a id="request"></a>
### リクエスト { #request }

<a id="request-header"></a>
#### リクエストヘッダ

| フィールド | 必須 | 説明 |
| --- | ----- | --- |
| Authorization | Y | 認証のための署名です。コンソールで発行したAPI認証情報を基に、AWS Signature Version 4の署名を作成する必要があります。 |
| Host | Y | リージョン別のエンドポイントです。 |
| x-amz-date | Y | ISO 8601 形式(UTC 基準) のリクエスト日時です。 |

<a id="response"></a>
### レスポンス { #response }

<a id="response-failure-response-code"></a>
#### エラーレスポンスコード

| HTTP ステータスコード | コード | 説明 |
| ---------- | --- | --- |
| 400 | InvalidPart | 指定したパートのうち1つ以上が見つかりません。パートがアップロードされていないか、指定したETagがパートのETagと一致しない可能性があります。 |
| 400 | InvalidPartOrder | パート一覧が昇順に並べ替えられていません。パート一覧はパート番号の順に指定する必要があります。 |
| 400 | EntityTooSmall | アップロードしようとしているパートが最小許容サイズ(5MiB)より小さいです。最後のパートを除く全てのパートは最小サイズ以上である必要があります。 |
| 400 | EntityTooLarge | アップロードしようとしているオブジェクトが最大許容サイズ(5GiB)を超えています。 |
| 404 | NoSuchKey | 指定したキーが存在しません。 |
| 404 | NoSuchBucket | 指定したバケットが存在しません。 |
| 404 | NoSuchBucketPolicy | 指定したバケットにポリシーが存在しません。 |
| 405 | MethodNotAllowed | 該当リソースに対して指定したHTTPメソッドは許可されていません。 |
| 409 | BucketAlreadyOwnedByYou | 作成しようとしているバケットはすでに存在しており、ユーザー自身が所有しています。 |
| 500 | InternalError | サーバー内部でエラーが発生しました。 |
| 503 | ServiceUnavailable | サービスが現在リクエストを処理できません。しばらくしてからもう一度お試しください。 |
| 503 | SlowDown | リクエストの送信頻度を下げてください。 |

<a id="data-integrity-verification"></a>
## データ整合性検証 { #data-integrity-verification }

Data Lake Storageは、アップロード及びダウンロード時にチェックサムを使用したデータ整合性検証をサポートします。
アップロード時に指定したチェックサムアルゴリズムでチェックサム値を計算して送信すると、サーバーで独立してチェックサムを計算し、一致するかどうかを確認した後にオブジェクトを保存します。

!!! tip "ポイント"
    `x-amz-checksum-*`ヘッダと`Content-MD5`ヘッダが同時にリクエストに含まれる場合、`x-amz-checksum-*`ヘッダが優先的に適用されます。

<a id="supported-checksum-algorithms"></a>
### サポートするチェックサムアルゴリズム { #supported-checksum-algorithms }

| アルゴリズム | パラメータ値 | 単一パートアップロード | マルチパート FULL_OBJECT | マルチパート COMPOSITE |
| --- | --- | --- | --- | --- |
| CRC-64/NVME | `CRC64NVME` | ✓ | ✓ | - |
| CRC-32 | `CRC32` | ✓ | ✓ | ✓ |
| CRC-32C | `CRC32C` | ✓ | ✓ | ✓ |
| SHA-1 | `SHA1` | ✓ | - | ✓ |
| SHA-256 | `SHA256` | ✓ | - | ✓ |
| XXHash64 | `XXHASH64` | ✓ | - | ✓ |
| XXHash3 | `XXHASH3` | ✓ | - | ✓ |
| XXHash128 | `XXHASH128` | ✓ | - | ✓ |
| SHA-512 | `SHA512` | ✓ | - | ✓ |

!!! tip "ポイント"
    MD5は`ChecksumAlgorithm`パラメータで指定できません。MD5整合性検証が必要な場合は、`Content-MD5`ヘッダを使用してください。

!!! tip "ポイント"
    XXHash64、XXHash3、XXHash128、SHA-512アルゴリズムを使用するには、最新バージョンのAWS SDKが必要です。

<a id="checksum-type"></a>
### チェックサムタイプ { #checksum-type }

マルチパートアップロード時にチェックサムタイプを指定できます。

| タイプ | 説明 |
| --- | --- |
| `FULL_OBJECT` | 全てのオブジェクトデータに基づいてチェックサムを計算します。CRCベースのアルゴリズム(CRC64NVME、CRC32、CRC32C)のみサポートします。 |
| `COMPOSITE` | 各パート別のチェックサムに基づいて全体のチェックサムを計算します。CRC64NVMEを除く全てのアルゴリズムをサポートします。 |

!!! tip "ポイント"
    単一パートアップロード(PutObject)はチェックサムタイプを別途指定せず、レスポンス時に`x-amz-checksum-type`は常に`FULL_OBJECT`として返されます。

<a id="single-part-upload-checksum"></a>
### 単一パートアップロードのチェックサム { #single-part-upload-checksum }

`PutObject` APIの呼び出し時に`--checksum-algorithm`オプションでチェックサムアルゴリズムを指定できます。

```sh
$ aws --endpoint-url=${Endpoint} s3api put-object \
    --bucket ${Bucket} \
    --key ${Key} \
    --body ${FilePath} \
    --checksum-algorithm CRC32
```

<a id="multipart-upload-checksum"></a>
### マルチパートアップロードのチェックサム { #multipart-upload-checksum }

マルチパートアップロード時に`CreateMultipartUpload`でアルゴリズムとチェックサムタイプを指定し、その後`UploadPart`で同じアルゴリズムを使用する必要があります。

!!! danger "注意"
    `CreateMultipartUpload`で指定したアルゴリズムと`UploadPart`で指定したアルゴリズムが異なる場合、400エラーが返されます。

<a id="payload-signing-method"></a>
### ペイロード署名方式 { #payload-signing-method }

`x-amz-content-sha256`ヘッダでペイロード署名方式を指定できます。
Data Lake Storageでサポートする方式は次のとおりです。

| 方式 | ヘッダ値 | 説明 |
| --- | --- | --- |
| 非署名 | `UNSIGNED-PAYLOAD` | ペイロードに署名を含めません。 |
| チャンク + 後行チェックサム | `STREAMING-UNSIGNED-PAYLOAD-TRAILER` | ペイロードをチャンク単位で送信し、チェックサムをデータの末尾に追加します。 |

!!! tip "ポイント"
    AWS CLI v2.23.0以上及び最新のAWS SDKを使用する場合、チェックサムが含まれたアップロードリクエストはデフォルトで`STREAMING-UNSIGNED-PAYLOAD-TRAILER`方式で送信されます。

<a id="aws-command-line-interface-cli"></a>
## AWS コマンドラインインターフェース(CLI) { #aws-command-line-interface-cli }

S3互換APIを利用して、AWSコマンドラインインターフェースでNHN Cloud Data Lake Storageサービスを使用できます。

<a id="installation"></a>
### インストール { #installation }

[Installing past releases of the AWS CLI version 2](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-version.html) のドキュメントを参照し、AWSコマンドラインインターフェースをインストールします。

<a id="configuration"></a>
### 設定 { #configuration }

AWSコマンドラインインターフェースを使用するには、まずS3 API認証情報と環境を設定する必要があります。

```sh
$ aws configure
AWS Access Key ID [None]: ${Access Key}
AWS Secret Access Key [None]: ${Secret Key}
Default region name [None]: ${Region Name}
Default output format [None]:
```

| 名前 | 説明 |
| --- | --- |
| Access Key | S3 API認証情報のAccess Key |
| Secret Key | S3 API認証情報のSecret Key |
| Region Name | KR3 - 韓国(光州)リージョン |

<a id="how-to-use-the-s3-commands"></a>
### S3 コマンドの使用方法 { #how-to-use-the-s3-commands }

```sh
$ aws --endpoint-url=${Endpoint} s3 ${Command} s3://${Bucket}
```

| 名前 | 説明 |
| --- | --- |
| Endpoint | https://kr3-data-lake-storage.nhncloudservice.com - 韓国(光州)リージョン |
| Command | AWSコマンドラインインターフェースのコマンド |
| Bucket | バケット名 |

!!! tip 「ポイント」
    AWSコマンドラインインターフェースはAWSを使用するために提供されるツールであるため、AWSのドメインを使用するように設定されています。したがって、NHN Cloud Data Lake Storageサービスを使用するには、必ずコマンドごとにエンドポイントを指定する必要があります。
    AWSコマンドラインインターフェースのコマンドは、[AWS CLI での高レベル (s3) コマンドの使用](https://docs.aws.amazon.com/ja_jp/cli/latest/userguide/cli-services-s3-commands.html)のドキュメントをご参照ください。

<a id="aws-sdk"></a>
## AWS SDK { #aws-sdk }

AWSは様々なプログラミング言語向けのSDKを提供しています。S3互換APIを利用して、AWS SDKでNHN Cloud Data Lake Storageサービスを使用できます。

!!! tip 「ポイント」
    詳細は[AWS SDK](https://builder.aws.com/build/tools)のドキュメントをご参照ください。

<a id="java-sdk"></a>
### Java SDK { #java-sdk }

!!! tip 「ポイント」
    詳細は[AWS SDK for Java](https://docs.aws.amazon.com/ja_jp/sdk-for-java/)のドキュメントをご参照ください。

<a id="boto3---python-sdk"></a>
### Boto3 - Python SDK { #boto3---python-sdk }

!!! tip 「ポイント」
    詳細は[AWS SDK for Python(Boto3)](https://docs.aws.amazon.com/ja_jp/pythonsdk/)のドキュメントをご参照ください。
