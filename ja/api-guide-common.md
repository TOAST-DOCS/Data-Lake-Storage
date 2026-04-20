## Data Lake Storage API ガイド

**Data & Analytics > Data Lake Storage > API ガイド > 共通**

## Data Lake Storage API 共通情報

!!! tip 「ポイント」
    NHN Cloud Data Lake Storageサービスは、Amazon S3 API 2006-03-01バージョンと互換性を持つように設計されています。

### API エンドポイント

| リージョン | エンドポイント |
| --- | ----- |
| KR3 | https://kr3-data-lake-storage.nhncloudservice.com |

### 認証及び権限

Data Lake Storageは、API呼び出し時の認証と認可のためにS3 API認証情報が必要です。[S3 API 認証情報(S3 API Credential)](https://docs.beta-nhncloud.com/ja/Data%20&%20Analytics/Data%20Lake%20Storage/ko/console-user-guide/#_10)を参照して、APIの使用に必要な情報を準備してください。

### リクエスト

#### リクエストヘッダ

| フィールド | 必須 | 説明 |
| --- | ----- | --- |
| Authorization | Y | 認証のための署名です。コンソールで発行したAPI認証情報を基に、AWS Signature Version 4の署名を作成する必要があります。 |
| Host | Y | リージョン別のエンドポイントです。 |
| x-amz-date | Y | ISO 8601 形式(UTC 基準) のリクエスト日時です。 |

### レスポンス

#### エラーレスポンスコード

| HTTP ステータスコード | コード | 説明 |
| ---------- | --- | --- |
| 400 | InvalidPart | 指定したパートのうち1つ以上が見つかりません。パートがアップロードされていないか、指定したETagがパートのETagと一致しない可能性があります。 |
| 400 | InvalidPartOrder | パート一覧が昇順に並べ替えられていません。パート一覧はパート番号の順に指定する必要があります。 |
| 400 | EntityTooSmall | アップロードしようとしているパートが最小許容サイズ(5MiB)より小さいです。最後のパートを除く全てのパートは最小サイズ以上である必要があります。 |
| 400 | EntityTooLarge | アップロードしようとしているオブジェクトが最大許容サイズ(5GiB)を超えています。 |
| 404 | NoSuchKey | 指定したキーが存在しません。 |
| 404 | NoSuchBucket | 指定したバケットが存在しません。 |
| 405 | MethodNotAllowed | 該当リソースに対して指定したHTTPメソッドは許可されていません。 |
| 409 | BucketAlreadyOwnedByYou | 作成しようとしているバケットはすでに存在しており、ユーザー自身が所有しています。 |
| 500 | InternalError | サーバー内部でエラーが発生しました。 |
| 503 | ServiceUnavailable | サービスが現在リクエストを処理できません。しばらくしてからもう一度お試しください。 |
| 503 | SlowDown | リクエストの送信頻度を下げてください。 |

## AWS コマンドラインインターフェース(CLI)

S3互換APIを利用して、AWSコマンドラインインターフェースでNHN Cloud Data Lake Storageサービスを使用できます。

### インストール

[Installing past releases of the AWS CLI version 2](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-version.html) のドキュメントを参照し、AWSコマンドラインインターフェースをインストールします。

!!! tip 「ポイント」
    NHN Cloud Data Lake Storageサービスは、AWS CLI バージョン 2.22.35までサポートしています。

### 設定

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

### S3 コマンドの使用方法

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

## AWS SDK

AWSは様々なプログラミング言語向けのSDKを提供しています。S3互換APIを利用して、AWS SDKでNHN Cloud Data Lake Storageサービスを使用できます。

!!! tip 「ポイント」
    詳細は[AWS SDK](https://builder.aws.com/build/tools)のドキュメントをご参照ください。

### Java SDK

!!! tip 「ポイント」
    詳細は[AWS SDK for Java](https://docs.aws.amazon.com/ja_jp/sdk-for-java/)のドキュメントをご参照ください。

### Boto3 - Python SDK

!!! tip 「ポイント」
    詳細は[AWS SDK for Python(Boto3)](https://docs.aws.amazon.com/ja_jp/pythonsdk/)のドキュメントをご参照ください。
