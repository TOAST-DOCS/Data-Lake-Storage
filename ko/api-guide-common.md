## Data Lake Storage API 가이드

**Data & Analytics > Data Lake Storage > API 가이드 > 공통**

## Data Lake Storage API 공통 정보

!!! tip "알아두기"
    NHN Cloud Data Lake Storage 서비스는 Amazon S3 API 2006-03-01 버전과 호환되도록 설계되어 있습니다.

### API 엔드포인트

| 리전 | 엔드포인트 |
| --- | ----- |
| KR3 | https://kr3-data-lake-storage.nhncloudservice.com |

### 인증 및 권한

Data Lake Storage는 API 호출 시 인증/인가를 위해 S3 API 자격 증명이 필요합니다. [S3 API 자격 증명(S3 API Credential)](console-user-guide/#_10)를 참고하여 API 사용에 필요한 정보를 준비합니다.

### 요청

#### 요청 헤더

| 필드 | 필수 여부 | 설명 |
| --- | ----- | --- |
| Authorization | Y | 인증을 위한 서명입니다. 콘솔에서 발급한 API 자격 증명 정보를 바탕으로 AWS Signature Version 4 서명을 만들어야 합니다. |
| Host | Y | 리전별 엔드포인트입니다. |
| x-amz-date | Y | ISO 8601 형식(UTC 기준) 요청 일시입니다. |

### 응답

#### 실패 응답 코드

| HTTP 상태 코드 | 코드 | 설명 |
| ---------- | --- | --- |
| 400 | InvalidPart | 지정한 파트 중 하나 이상을 찾을 수 없습니다. 파트가 업로드되지 않았거나, 지정한 ETag가 파트의 ETag와 일치하지 않을 수 있습니다. |
| 400 | InvalidPartOrder | 파트 목록이 오름차순으로 정렬되지 않았습니다. 파트 목록은 파트 번호 순서대로 지정해야 합니다. |
| 400 | EntityTooSmall | 업로드하려는 파트가 최소 허용 크기(5MiB)보다 작습니다. 마지막 파트를 제외한 모든 파트는 최소 크기 이상이어야 합니다. |
| 400 | EntityTooLarge | 업로드하려는 객체가 최대 허용 크기(5GiB)를 초과합니다. |
| 404 | NoSuchKey | 지정한 키가 존재하지 않습니다. |
| 404 | NoSuchBucket | 지정한 버킷이 존재하지 않습니다. |
| 405 | MethodNotAllowed | 해당 리소스에 대해 지정한 HTTP 메서드가 허용되지 않습니다. |
| 409 | BucketAlreadyOwnedByYou | 생성하려는 버킷이 이미 존재하며 사용자가 소유하고 있습니다. |
| 500 | InternalError | 서버 내부에 오류가 발생했습니다. |
| 503 | ServiceUnavailable | 서비스가 현재 요청을 처리할 수 없습니다. 잠시 후 다시 시도하세요. |
| 503 | SlowDown | 요청 속도를 줄이세요. |

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

## AWS 명령줄 인터페이스(CLI)

S3 호환 API를 이용하여 AWS 명령줄 인터페이스로 NHN Cloud Data Lake Storage 서비스를 사용할 수 있습니다.

### 설치

[Installing past releases of the AWS CLI version 2](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-version.html) 문서를 참고해 AWS 명령줄 인터페이스를 설치합니다.

### 설정

AWS 명령줄 인터페이스를 사용하려면 먼저 S3 API 자격 증명과 환경을 설정해야 합니다.

```sh
$ aws configure
AWS Access Key ID [None]: ${Access Key}
AWS Secret Access Key [None]: ${Secret Key}
Default region name [None]: ${Region Name}
Default output format [None]:
```

| 이름 | 설명 |
| --- | --- |
| Access Key | S3 API 자격 증명 접근 키 |
| Secret Key | S3 API 자격 증명 비밀 키 |
| Region Name | KR3 - 한국(광주) 리전 |

### S3 명령 사용 방법

```sh
$ aws --endpoint-url=${Endpoint} s3 ${Command} s3://${Bucket}
```

| 이름 | 설명 |
| --- | --- |
| Endpoint | https://kr3-data-lake-storage.nhncloudservice.com - 한국(광주) 리전 |
| Command | AWS 명령줄 인터페이스 명령 |
| Bucket | 버킷 이름 |

!!! tip "알아두기"
    AWS 명령줄 인터페이스는 AWS를 사용하기 위해 제공되는 도구이기 때문에 AWS 도메인을 사용하도록 설정되어 있습니다. 따라서 NHN Cloud Data Lake Storage 서비스를 사용하려면 반드시 명령마다 엔드포인트를 지정해야 합니다.
    AWS 명령줄 인터페이스 명령은 [AWS CLI에서 상위 수준(s3) 명령 사용](https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/cli-services-s3-commands.html) 문서를 참고하세요.

## AWS SDK

AWS는 여러 가지 프로그래밍 언어를 위한 SDK를 제공하고 있습니다. S3 호환 API를 이용하여 AWS SDK로 NHN Cloud Data Lake Storage 서비스를 사용할 수 있습니다.

!!! tip "알아두기"
    자세한 내용은 [AWS SDK](https://builder.aws.com/build/tools) 문서를 참고하세요.

### Java SDK

!!! tip "알아두기"
    자세한 내용은 [AWS SDK for Java](https://docs.aws.amazon.com/ko_kr/sdk-for-java/) 문서를 참고하세요.

### Boto3 - Python SDK

!!! tip "알아두기"
    자세한 내용은 [AWS SDK for Python(Boto3)](https://docs.aws.amazon.com/ko_kr/pythonsdk/) 문서를 참고하세요.
