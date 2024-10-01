
# Jenkins Backup Pipeline and Shell Script

This project automates the process of backing up a Jenkins instance and uploading the backup to an AWS S3 bucket. The Jenkins pipeline uses a shell script to perform the backup operation.

## Author
**Akash Nawin**

## Overview

The Jenkins pipeline (`Jenkinsfile`) triggers a backup of the Jenkins home directory (`/var/lib/jenkins`) and uploads the archive to an S3 bucket. The backup includes jobs, plugins, users, secrets, nodes, and essential configuration files.

## Components

### 1. **Jenkinsfile**

- **Environment Variables:**
  - `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` are stored as secret text credentials in Jenkins.
  
- **Pipeline Stages:**
  - **Run Backup Script:** Executes a shell script (`jenkins_backup.sh`) that performs the backup and uploads it to an S3 bucket.

### 2. **Shell Script (`jenkins_backup.sh`)**

- **Backup Process:**
  1. Verifies root privileges.
  2. Backs up the Jenkins home directory (`/var/lib/jenkins`).
  3. Archives the selected directories and files (jobs, plugins, users, secrets, nodes).
  4. Uploads the backup tarball to an AWS S3 bucket.
  
- **S3 Bucket:**
  - Ensure the script has the appropriate IAM role or access rights to upload the backup to the S3 bucket (`pipeline-jenkins-backup-shell`).

## Usage

### 1. **Jenkins Pipeline Setup:**

- **Credentials:**
  - Add AWS credentials in Jenkins as "Secret Text" under the IDs: 
    - `AWS_ACCESS_KEY_ID`
    - `AWS_SECRET_ACCESS_KEY`

- **Jenkinsfile:**
  - Ensure the `Jenkinsfile` is placed in the appropriate repository or project.

### 2. **Shell Script Usage:**

- **Running Manually:**

```bash
sudo ./jenkins_backup.sh /var/lib/jenkins <AWS_ACCESS_KEY_ID> <AWS_SECRET_ACCESS_KEY>
```

- **Backup Files:**
  - The backup will be stored temporarily in `/tmp/jenkins-archive-<timestamp>.tar.gz`.
  
- **Log File:**
  - The backup process logs messages in `/var/log/jenkins_backup.log`.

## Prerequisites

1. **AWS CLI:**
   - Ensure the AWS CLI is installed and configured on the Jenkins server or machine where the script runs.
   
   To install AWS CLI:
   ```bash
   sudo apt-get install awscli
   ```

2. **Permissions:**
   - Ensure the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` have the necessary permissions to upload files to the S3 bucket.

## Script Details

### Variables:

- **`JENKINS_HOME`**: Path to Jenkins home directory (`/var/lib/jenkins`).
- **`AWS_ACCESS_KEY_ID` / `AWS_SECRET_ACCESS_KEY`**: AWS credentials for S3 access.
- **`DEST_FILE`**: Destination file for the tar archive (`/tmp/jenkins-archive.tar.gz`).
- **`TMP_DIR`**: Temporary directory for creating the backup archive.
- **`LOG_FILE`**: Location of the log file for backup operations (`/var/log/jenkins_backup.log`).

### Functions:

- **`log_message`**: Writes log messages to the specified log file.
- **`copyto_s3`**: Uploads the backup archive to an AWS S3 bucket.
- **`backup_jobs`**: Recursively backs up Jenkins jobs.
- **`main`**: Manages the overall backup process and triggers functions.

## Example

To run the Jenkins pipeline:

1. **Configure AWS Credentials** in Jenkins.
2. **Run the pipeline**, which triggers the shell script and backs up Jenkins data.

```bash
sudo ./jenkins_backup.sh /var/lib/jenkins <AWS_ACCESS_KEY_ID> <AWS_SECRET_ACCESS_KEY>
```

## License

This script and pipeline are distributed under the MIT License.
