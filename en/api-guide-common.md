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

## AWS Command Line Interface (CLI)

You can use the NHN Cloud Data Lake Storage service with the AWS command-line interface using the S3-compatible API.

### Installation

See [Installing past releases of the AWS CLI version 2](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-version.html) to install the AWS command-line interface.

!!! tip "Note"
    NHN Cloud Data Lake Storage service supports up to version 2.22.35 of the AWS CLI.

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
