---

# Jenkins Backup Automation

## Overview

This project automates the process of backing up a Jenkins server's critical configuration files, jobs, plugins, users, and secrets. It provides a shell script that archives these important files and securely uploads them to an Amazon S3 bucket. This automation ensures that Jenkins backups are performed consistently and can be easily restored in case of data loss.

## Features

- **Automated Backup:** The script automatically archives Jenkins home directories, jobs, plugins, users, and other essential data.
- **S3 Upload:** The backup file is uploaded to a pre-configured Amazon S3 bucket for secure storage.
- **Logging:** Detailed logs are generated for each backup operation, providing traceability and troubleshooting information.
- **Jenkins Pipeline Integration:** The script can be run manually or integrated with a Jenkins Pipeline for automated backups.

## How It Works

1. **Jenkins Pipeline**:
   A Jenkinsfile triggers the backup process through a shell script, passing AWS credentials and the Jenkins home directory as parameters.

2. **Backup Script**:
   The script checks for root privileges, archives the Jenkins home directory, and uploads the generated `.tar.gz` file to a specified S3 bucket. The backup process includes Jenkins jobs, plugins, users, secrets, and nodes.

3. **Logging**:
   Backup operations are logged in `/var/log/jenkins_backup.log`, helping track the process and identify any issues.

## Prerequisites

- **Jenkins Server**: Ensure you have a Jenkins server with necessary permissions to execute scripts.
- **AWS Credentials**: AWS credentials (`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`) must be stored as Jenkins credentials for the script to access the S3 bucket.
- **S3 Bucket**: Set up an S3 bucket for storing the backup archives.

## Setup Instructions

### 1. Configure AWS Credentials in Jenkins
- Go to **Jenkins Dashboard > Manage Jenkins > Manage Credentials**.
- Add your `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` as secret text credentials.

### 2. Add Jenkins Pipeline
- Create or update your Jenkins Pipeline with the provided `Jenkinsfile`. This file will trigger the backup script and handle environment variables securely.

### 3. Backup Script
- The `jenkins_backup.sh` script is responsible for performing the backup. It takes the Jenkins home directory and AWS credentials as inputs, creating a timestamped backup archive and uploading it to the S3 bucket.

### 4. Schedule Automated Backups
- You can schedule the Jenkins Pipeline to run at regular intervals to automate backups (e.g., daily or weekly backups).

## Usage

### Running the Backup Manually
To manually trigger the backup process, run the following command on your Jenkins server:

```bash
sudo ./jenkins_backup.sh /path/to/jenkins_home AWS_ACCESS_KEY_ID AWS_SECRET_ACCESS_KEY
```

### Pipeline Triggered Backup
When the Jenkins Pipeline is executed, the backup script will be automatically triggered, and the archive will be created and uploaded to the specified S3 bucket.

## Logging

Backup events and errors are logged in `/var/log/jenkins_backup.log`. This log file provides detailed information about the operations, helping with monitoring and troubleshooting.

## Future Enhancements

- **Email Notifications**: Add support for email alerts on successful or failed backups.
- **Retention Policy**: Implement an S3 lifecycle policy to automatically delete old backups after a specified period.
- **Error Handling**: Improve error handling to capture and report all potential failure points.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---
