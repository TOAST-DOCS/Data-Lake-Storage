## Data Lake Storage API Guide

**Data & Analytics > Data Lake Storage > API Guide > Common**

## Common Information for Data Lake Storage API

!!! tip "Note"
    NHN Cloud Data Lake Storage is designed to be compatible with Amazon S3 API 2006-03-01.

### API Endpoint

| Region | Endpoint |
| --- | ----- |
| KR3 | https://kr3-data-lake-storage.nhncloudservice.com |

### Authentication and Authorization

Data Lake Storage requires S3 API credentials for authentication/authorization when making API calls. Refer to [S3 API Credential](console-user-guide/#manage-credentials) to prepare the information required to use the API.

### Request

#### Request Header

| Field | Required | Description |
| --- | ----- | --- |
| Authorization | Y | A signature for authentication. You must create an AWS Signature Version 4 signature based on the API credentials issued from the console. |
| Host | Y | Endpoint per region. |
| x-amz-date | Y | Request time in ISO 8601 format (UTC). |

### Response

#### Failure Response Code

| HTTP Status Code | Code | Description |
| ---------- | --- | --- |
| 400 | InvalidPart | Could not find one or more of the specified parts. The part has not been uploaded, or the specified ETag may not match the part's ETag. |
| 400 | InvalidPartOrder | The part list is not sorted in ascending order. Parts must be specified in order of part number. |
| 400 | EntityTooSmall | The part to be uploaded is smaller than the minimum allowed size (5 MiB). All parts except the last must meet the minimum size requirement. |
| 400 | EntityTooLarge | The object to be uploaded exceeds the maximum allowed size (5 GiB). |
| 404 | NoSuchKey | The specified key does not exist. |
| 404 | NoSuchBucket | The specified bucket does not exist. |
| 405 | MethodNotAllowed | The HTTP method specified for the resource is not allowed. |
| 409 | BucketAlreadyOwnedByYou | The bucket you want to create already exists and is owned by the user. |
| 500 | InternalError | An internal server error occurred. |
| 503 | ServiceUnavailable | The service can't process the request right now. Please try again later. |
| 503 | SlowDown | Reduce the request speed. |

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

## AWS Command Line Interface (CLI)

You can use the NHN Cloud Data Lake Storage service with the AWS command-line interface using the S3-compatible API.

### Installation

See [Installing past releases of the AWS CLI version 2](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-version.html) to install the AWS command-line interface.

### Configuration

To use the AWS Command Line Interface, you must first configure the S3 API credentials and environment.

```sh
$ aws configure
AWS Access Key ID [None]: ${Access Key}
AWS Secret Access Key [None]: ${Secret Key}
Default region name [None]: ${Region Name}
Default output format [None]:
```

| Name | Description |
| --- | --- |
| Access Key | S3 API credentials access key |
| Secret Key | S3 API credentials secret key |
| Region Name | KR3 - Korea (Gwangju) region |

### How to Use the S3 Commands

```sh
$ aws --endpoint-url=${Endpoint} s3 ${Command} s3://${Bucket}
```

| Name | Description |
| --- | --- |
| Endpoint | https://kr3-data-lake-storage.nhncloudservice.com - Korea (Gwangju) region: |
| Command | Command for AWS Command Line Interface |
| Bucket | Bucket name |

!!! tip "Note"
    Since the AWS CLI is provided for use with AWS, it is configured to use the AWS domain. Therefore, to use NHN Cloud Data Lake Storage, you must specify an endpoint for every command.
    For AWS CLI commands, see [Using high-level (s3) commands with the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-s3-commands.html).

## AWS SDK

AWS provides SDKs for many types of programming languages. By using the S3 compatible API, you can use NHN Cloud Data Lake Storage with AWS SDK.

!!! tip "Note"
    For more information, see the [AWS SDK](https://builder.aws.com/build/tools) documentation.

### Java SDK

!!! tip "Note"
    For more information, see the [AWS SDK for Java](https://docs.aws.amazon.com/en_us/sdk-for-java/) documentation.

### Boto3 - Python SDK

!!! tip "Note"
    For more information, see the [AWS SDK for Python(Boto3)](https://docs.aws.amazon.com/en_us/pythonsdk/).
