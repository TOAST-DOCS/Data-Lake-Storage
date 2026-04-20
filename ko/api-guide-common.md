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

## AWS 명령줄 인터페이스(CLI)

S3 호환 API를 이용하여 AWS 명령줄 인터페이스로 NHN Cloud Data Lake Storage 서비스를 사용할 수 있습니다.

### 설치

[Installing past releases of the AWS CLI version 2](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-version.html) 문서를 참고해 AWS 명령줄 인터페이스를 설치합니다.

!!! tip "알아두기"
    NHN Cloud Data Lake Storage 서비스는 AWS CLI 버전 2.22.35까지 지원합니다.

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
