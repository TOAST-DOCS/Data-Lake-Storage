## Console Guide

**Data & Analytics > Data Lake Storage > Console User Guide**

Data Lake Storage service console provides the following features:

* View and manage buckets
* Manage and view credentials
* View object lists and details

## View Bucket

### List Buckets

You can check the list of created buckets in the Data Lake Storage service console.

* You can use the checkbox on the left side of the name to select a bucket.
* You can update the list by clicking **Refresh** on the top right of the list.

| Item | Description |
| --- | --- |
| Name | Bucket name |
| Number of Objects | Number of objects stored in the bucket |
| Size | The total size of the object stored in the bucket |

### Search Bucket

You can search for buckets whose names contain the string you entered, using the input field and search button at the top left of the bucket list.

| Item | Required | Description | Restriction |
| --- | --- | --- | --- |
| Keyword | O | Bucket name to search for | English lower case letters, numbers, and some special characters ('`.`', '`-`') are allowed. |

### View Bucket Details

You can view the details of a bucket by clicking anywhere in the bucket list other than the bucket name.

| Item | Description |
| --- | --- |
| Bucket name | Bucket name |
| Description | Description registered when creating the bucket |
| Number of Objects | Number of objects stored in the bucket |
| Size | The total size of the object stored in the bucket |
| Created on | Created time for bucket |
| Last updated time | Last modified time of the bucket |

## Manage Bucket

### Create Bucket

You can create a bucket by clicking **Create Bucket** at the top of the bucket list. The input values required to create a bucket are as shown in the table below.

!!! tip "Note"
    The bucket name of the Data Lake Storage service is region-unique.

| Item | Required | Description | Restriction |
| --- | --- | --- | --- |
| Name | O | Bucket name | It must be between 3 and 63 characters.<br>English lower case letters, numbers, and some special characters ('`.`', '`-`') are allowed.<br>The name must begin and end with English lowercase letter or number.<br>Consecutive `.` (`..`), IPv4 pattern, some prefixes and suffixes are not allowed.<br>Unavailable prefixes: `xn--`, `sthree-`, and `amzn-s3-demo-`<br>Unavailable suffixes: `-s3alias`, `--ol-s3`, `.mrap`, `--x-s3`, and `--table-s3` |
| Description | X | Bucket Description | Enter 100 characters or fewer. |

### Delete Bucket

To delete a bucket, select it using the checkboxes in the bucket list, then click **Delete** at the top of the bucket list.

!!! danger "Caution"
    The bucket to be deleted must be empty, with all objects removed. If the bucket is not empty, use the object removal API or the Empty Bucket feature to empty the bucket first.

Deleting a bucket cannot be undone.

### Empty Bucket

After selecting a bucket using the checkbox in the bucket list, you can click **Empty Bucket** at the top of the bucket list to request removal of all objects in the bucket.

| Item | Required | Description | Restriction |
| --- | --- | --- | --- |
| Name | O | Name of the bucket to empty |  |

!!! danger "Caution"
    It may take time depending on the number of objects.

You cannot cancel or undo the bucket emptying during the process.

## Manage Credentials

### View Credentials

Clicking **S3 API Credentials** at the top of the bucket list page displays the Credentials Management pop-up. You can view the list of issued credentials in the pop-up.

| Item | Description |
| --- | --- |
| Access Key | Access Key for credentials |
| Description | Description registered for credential issuance |
| Created at | Creation time of the credentials |
| Last used date | Last used time of the credentials |
| Management | Options to edit and delete |

### Issue Credentials

You can issue new credentials by clicking **Register S3 API Credentials** at the top of the S3 API Credentials popup. When issuing credentials, you can add a description, such as the purpose.

| Item | Required | Description | Restriction |
| --- | --- | --- | --- |
| Description | X | Description for the purpose of credentials | Up to 100 characters |

!!! tip "Note"
    You can issue up to 3 S3 API credentials per project, per user account.

!!! danger "Caution"
    Credentials are used for user authentication when using the API. If leaked, others may be able to access your storage features and objects. Always store them in a secure location and do not share them with others.
    The Access Key and Secret Key are displayed immediately after the credentials are issued. The Secret Key can only be viewed once, immediately after issuance.

### Modify Credentials

You can modify the description of S3 API credentials by clicking **Modify** in the Management column of the S3 API Credentials list.

| Item | Required | Description | Restriction |
| --- | --- | --- | --- |
| Description | X | Description for the purpose of credentials | Up to 100 characters |

### Delete Credentials

You can delete credentials by clicking **Delete** in the S3 API credentials management area. A confirmation popup will be displayed.

!!! danger "Caution"
    API authentication cannot be done with deleted credentials. Also, deleted credentials cannot be recovered.

## View Object

### List Objects

You can view the list of objects in the bucket by clicking the bucket name in the bucket list.

| Item | Description |
| --- | --- |
| Name | Object name |
| Size | Object size |
| Last updated time | Upload time of the object |

### View Object Details

You can view the details of an object by clicking the file object in the object list.

| Item | Description |
| --- | --- |
| Object name | Object name |
| Size | Object size |
| ETag | Object version information |
| Last updated time | Upload time of the object |
| Storage class | Storage class of the object |
