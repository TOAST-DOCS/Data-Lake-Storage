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

Data Lake Storageは、API呼び出し時の認証と認可のためにS3 API認証情報が必要です。[S3 API 認証情報(S3 API Credential)](console-user-guide/#_10)を参照して、APIの使用に必要な情報を準備してください。

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

## 데이터 무결성 검증

Data Lake Storage는 업로드 및 다운로드 시 체크섬을 통한 데이터 무결성 검증을 지원합니다.
업로드 시 지정한 체크섬 알고리즘으로 체크섬 값을 계산하여 전송하면, 서버에서 독립적으로 체크섬을 계산하여 일치 여부를 확인한 후 오브젝트를 저장합니다.

!!! tip "알아두기"
    `x-amz-checksum-*` 헤더와 `Content-MD5` 헤더가 동시에 요청에 포함된 경우, `x-amz-checksum-*` 헤더가 우선 적용됩니다.

### 지원 체크섬 알고리즘

| 알고리즘 | 파라미터 값 | 단일 파트 업로드 | 멀티파트 FULL_OBJECT | 멀티파트 COMPOSITE |
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

!!! tip "알아두기"
    MD5는 `ChecksumAlgorithm` 파라미터로 지정할 수 없습니다. MD5 무결성 검증이 필요한 경우 `Content-MD5` 헤더를 사용하세요.

!!! tip "알아두기"
    XXHash64, XXHash3, XXHash128, SHA-512 알고리즘을 사용하려면 최신 버전의 AWS SDK가 필요합니다.

### 체크섬 타입

멀티파트 업로드 시 체크섬 타입을 지정할 수 있습니다.

| 타입 | 설명 |
| --- | --- |
| `FULL_OBJECT` | 전체 오브젝트 데이터를 기반으로 체크섬을 계산합니다. CRC 기반 알고리즘(CRC64NVME, CRC32, CRC32C)만 지원합니다. |
| `COMPOSITE` | 각 파트별 체크섬을 기반으로 전체 체크섬을 계산합니다. CRC64NVME를 제외한 모든 알고리즘을 지원합니다. |

!!! tip "알아두기"
    단일 파트 업로드(PutObject)는 체크섬 타입을 별도로 지정하지 않으며, 응답 시 `x-amz-checksum-type`은 항상 `FULL_OBJECT`로 반환됩니다.

### 단일 파트 업로드 체크섬

`PutObject` API 호출 시 `--checksum-algorithm` 옵션으로 체크섬 알고리즘을 지정할 수 있습니다.

```sh
$ aws --endpoint-url=${Endpoint} s3api put-object \
    --bucket ${Bucket} \
    --key ${Key} \
    --body ${FilePath} \
    --checksum-algorithm CRC32
```

### 멀티파트 업로드 체크섬

멀티파트 업로드 시 `CreateMultipartUpload`에서 알고리즘과 체크섬 타입을 지정하고, 이후 `UploadPart`에서 동일한 알고리즘을 사용해야 합니다.

!!! danger "주의"
    `CreateMultipartUpload`에서 지정한 알고리즘과 `UploadPart`에서 지정한 알고리즘이 다를 경우 400 오류가 반환됩니다.

### 페이로드 서명 방식

`x-amz-content-sha256` 헤더를 통해 페이로드 서명 방식을 지정할 수 있습니다.
Data Lake Storage에서 지원하는 방식은 아래와 같습니다.

| 방식 | 헤더 값 | 설명 |
| --- | --- | --- |
| 비서명 | `UNSIGNED-PAYLOAD` | 페이로드에 서명을 포함하지 않습니다. |
| 청크 + 후행 체크섬 | `STREAMING-UNSIGNED-PAYLOAD-TRAILER` | 페이로드를 청크 단위로 전송하며, 체크섬을 데이터 끝에 추가합니다. |

!!! tip "알아두기"
    AWS CLI v2.23.0 이상 및 최신 AWS SDK를 사용하는 경우, 체크섬이 포함된 업로드 요청은 기본적으로 `STREAMING-UNSIGNED-PAYLOAD-TRAILER` 방식으로 전송됩니다.

## AWS コマンドラインインターフェース(CLI)

S3互換APIを利用して、AWSコマンドラインインターフェースでNHN Cloud Data Lake Storageサービスを使用できます。

### インストール

[Installing past releases of the AWS CLI version 2](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-version.html) のドキュメントを参照し、AWSコマンドラインインターフェースをインストールします。

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
