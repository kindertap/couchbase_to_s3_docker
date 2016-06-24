
# Couchbase backup to S3 using Docker


Docker container to run Couchbase backup and copy it to AWS S3 bucket.

Version 4.5 is at the time of writing Entrprise editions and contains
new backup binary `cbbackupmgr`, which is missing in pre 4.5
versions. Backup script in version 4.0.0 uses previous backup binary
`cbbackup`.

# Running scripts

## AWS configuration

Create AWS user and set permissions for S3. User should be able to both
upload and download files from the specified bucket.

See
http://docs.aws.amazon.com/AmazonS3/latest/dev/s3-access-control.html
for the reference.

### AWS Credentials

Create `~/.aws` directory and configure profile for S3 access as
desribed in
[AWS CLI Documentaion](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-config-files). Name
of the profile in AWS configuration should correspond to the value of
the variable `${AWS_PROFILE}` in both scripts `run_backup.sh` and `run_restore.sh`


## run_backup.sh

Before running script first time, create directory defined by
`${HOST_BACKUP_PATH}` variable: `mkdir /var/backup` (example).

If host server hs SELinux enabled execute following command:

```
# chcon -Rt svirt_sandbox_file_t ${HOST_BACKUP_PATH}

```


## run_restore.sh

Before executing script create directory for downloading backup files
from S3. This directory should be different from the backup directory
above. Downloaded files will overwrite current backup otherwise.

```
mkdir /var/restore
```

Edit file `run_restore.sh` and change variables at the top of the file
correspondingly.

## Variables

Following varaibles should be configured accordingly to your
environment:

- AWS_PROFILE - AWS profile name for writing and reading from S3.
- AWS_REGION - Region where you S3 bucket is located (default: `us-east-1`).
- S3_BUCKET - bucket name for the backup.
- SERVER_USER - Couchbase server user connecting. Default is Administrator.
- SERVER_PASSWORD - Password for the backup user on the Couchbase server.

## Logging

Both scripts create log file on each run. Filename of the log is like:
`run_backup.sh.13950.160624.log`, `run_restore.sh.28052.160624.log`.

# Author

Dmytro Kovalov, dmytro.kovalov@gmail.com

June 2016, Tokyo
