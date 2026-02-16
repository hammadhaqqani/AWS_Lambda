# AWS Lambda Functions

[![AWS Lambda](https://img.shields.io/badge/AWS_Lambda-%23FF9900.svg?logo=aws-lambda&logoColor=white)](https://aws.amazon.com/lambda/)
[![Python](https://img.shields.io/badge/Python-3776AB.svg?logo=python&logoColor=white)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-ffdd00?style=flat&logo=buy-me-a-coffee&logoColor=black)](https://buymeacoffee.com/hammadhaqqani)

AWS Lambda functions for automating routine cloud operations including EBS volume backups, RDS cross-region disaster recovery, and snapshot lifecycle management.

## Lambda Functions

| Function | Description |
|----------|-------------|
| **backupvolume.py** | Automated EBS volume snapshots with tag-based selection |
| **prunevolume.py** | Snapshot retention cleanup to manage storage costs |
| **snapshot_creation.py** | RDS automated snapshot creation |
| **copy_snapshot.py** | Cross-region RDS snapshot replication for DR |
| **resotring_snapshot.py** | RDS instance restoration from snapshot |
| **pruning_snapshot.py** | RDS snapshot retention and lifecycle management |

## Architecture

```
CloudWatch Events (Schedule)
        │
        ▼
   AWS Lambda ──► EBS Snapshots / RDS Snapshots
        │
        ▼
   SNS Notifications (Optional)
```

## Project Structure

```
.
├── lambdafunctions/
│   ├── Automating Backups using AWS Lambda/
│   │   ├── backupvolume.py      # Create EBS snapshots
│   │   └── prunevolume.py       # Clean up old snapshots
│   └── RDS failover/
│       ├── snapshot_creation.py  # Create RDS snapshots
│       ├── copy_snapshot.py      # Cross-region copy
│       ├── resotring_snapshot.py # Restore from snapshot
│       └── pruning_snapshot.py   # Retention cleanup
├── iam/
│   ├── iam_role_ebs.json        # IAM role for EBS Lambda
│   └── iam_rds.json             # IAM role for RDS Lambda
└── images/                       # Architecture diagrams
```

## Prerequisites

- AWS account with Lambda, EBS, RDS, CloudWatch, and IAM permissions
- Python 3.9+ runtime (AWS Lambda)
- AWS CLI configured for deployment

## Quick Start

1. **Create the IAM role** using the policies in `iam/`:
   ```bash
   aws iam create-role --role-name LambdaEBSBackup \
     --assume-role-policy-document file://iam/iam_role_ebs.json
   ```

2. **Deploy a Lambda function** (example: EBS backup):
   ```bash
   cd "lambdafunctions/Automating Backups using AWS Lambda"
   zip backupvolume.zip backupvolume.py
   
   aws lambda create-function \
     --function-name EBSBackup \
     --runtime python3.9 \
     --handler backupvolume.lambda_handler \
     --role arn:aws:iam::YOUR_ACCOUNT:role/LambdaEBSBackup \
     --zip-file fileb://backupvolume.zip
   ```

3. **Schedule with CloudWatch Events**:
   ```bash
   aws events put-rule --name DailyEBSBackup \
     --schedule-expression "rate(1 day)"
   ```

## Configuration

- **EBS Backup**: Tag your volumes with `Backup: true` to include them in automated snapshots
- **RDS DR**: Configure the source and destination regions in the Lambda environment variables
- **Retention**: Set `RETENTION_DAYS` environment variable to control snapshot cleanup

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## License

[Apache 2.0](LICENSE)

## Author

**Hammad Haqqani** - DevOps Architect & Cloud Engineer

- Website: [hammadhaqqani.com](https://hammadhaqqani.com)
- LinkedIn: [linkedin.com/in/haqqani](https://www.linkedin.com/in/haqqani)
- Email: phaqqani@gmail.com

---

## Support

If you find this useful, consider buying me a coffee!

[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-ffdd00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black)](https://buymeacoffee.com/hammadhaqqani)
