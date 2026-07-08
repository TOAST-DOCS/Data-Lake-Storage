## Amazon S3-Compatible API Guide

**Data & Analytics > Data Lake Storage > Amazon S3-Compatible API Guide > Common**

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
| 404 | NoSuchBucketPolicy | The specified bucket has no policy. |
| 405 | MethodNotAllowed | The HTTP method specified for the resource is not allowed. |
| 409 | BucketAlreadyOwnedByYou | The bucket you want to create already exists and is owned by the user. |
| 500 | InternalError | An internal server error occurred. |
| 503 | ServiceUnavailable | The service can't process the request right now. Please try again later. |
| 503 | SlowDown | Reduce the request speed. |

## Data Integrity Verification

Data Lake Storage supports data integrity verification through checksums during upload and download.
When uploading, calculate and send the checksum value using the specified checksum algorithm. The server independently calculates the checksum and verifies that the values match before storing the object.

!!! tip "Note"
    If both the `x-amz-checksum-*` header and the `Content-MD5` header are included in the request, the `x-amz-checksum-*` header takes precedence.

### Supported Checksum Algorithms

| Algorithm | Parameter value | Single-part upload | Multipart FULL_OBJECT | Multipart COMPOSITE |
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

!!! tip "Note"
    MD5 cannot be specified using the `ChecksumAlgorithm` parameter. If MD5 integrity verification is required, use the `Content-MD5` header.

!!! tip "Note"
    The latest version of the AWS SDK is required to use the XXHash64, XXHash3, XXHash128, and SHA-512 algorithms.

### Checksum Type

A checksum type can be specified for multipart uploads.

| Type | Description |
| --- | --- |
| `FULL_OBJECT` | Calculates the checksum based on the entire object data. Only CRC-based algorithms (CRC64NVME, CRC32, CRC32C) are supported. |
| `COMPOSITE` | Calculates the overall checksum based on the checksum of each part. All algorithms except CRC64NVME are supported. |

!!! tip "Note"
    For single-part uploads (PutObject), no checksum type is specified separately, and `x-amz-checksum-type` is always returned as `FULL_OBJECT` in the response.

### Single-Part Upload Checksum

When calling the `PutObject` API, a checksum algorithm can be specified using the `--checksum-algorithm` option.

```sh
$ aws --endpoint-url=${Endpoint} s3api put-object \
    --bucket ${Bucket} \
    --key ${Key} \
    --body ${FilePath} \
    --checksum-algorithm CRC32
```

### Multipart Upload Checksum

For multipart uploads, specify the algorithm and checksum type in `CreateMultipartUpload`, and use the same algorithm in subsequent `UploadPart` calls.

!!! danger "Caution"
    If the algorithm specified in `CreateMultipartUpload` differs from the algorithm specified in `UploadPart`, a 400 error is returned.

### Payload Signing Method

The payload signing method can be specified using the `x-amz-content-sha256` header.
The methods supported by Data Lake Storage are as follows:

| Method | Header value | Description |
| --- | --- | --- |
| Unsigned | `UNSIGNED-PAYLOAD` | Does not include a signature in the payload. |
| Chunked + trailing checksum | `STREAMING-UNSIGNED-PAYLOAD-TRAILER` | Sends the payload in chunks and appends the checksum at the end of the data. |

!!! tip "Note"
    When using AWS CLI v2.23.0 or later and the latest AWS SDK, upload requests that include a checksum are sent using the `STREAMING-UNSIGNED-PAYLOAD-TRAILER` method by default.

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
