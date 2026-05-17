# AWS Lambda EC2 Auto Backup Project

## Project Overview

This project automatically creates AMI backups for EC2 instances that contain a specific tag.

The automation is implemented using:

- AWS Lambda
- Amazon EventBridge
- Amazon EC2
- IAM Roles
- Python (boto3 SDK)

This is a production-style cloud automation project commonly used by DevOps and Cloud Engineers.

---

# Architecture

EventBridge Scheduler
        ↓
AWS Lambda Function
        ↓
EC2 API using boto3
        ↓
Create AMI Backup

---

# Use Case

Automatically create backups for production EC2 instances daily or every 2 days without manual intervention.

Only instances with the following tag are backed up:

Key:
Backup

Value:
True

---

# Technologies Used

| Service | Purpose |
|----------|----------|
| AWS Lambda | Serverless automation |
| Amazon EC2 | Virtual machines |
| Amazon EventBridge | Scheduled trigger |
| IAM | Permissions |
| CloudWatch | Logging and monitoring |
| Python boto3 | AWS SDK |

---

# Prerequisites

Before starting:

- AWS Account
- EC2 instance running
- Basic IAM knowledge
- Python understanding
- EC2 instance tagged for backup

---

# Step 1: Tag EC2 Instance

Go to:

EC2 Console → Instances → Tags

Add:

| Key | Value |
|------|------|
| Backup | True |

Only tagged instances will be backed up.

---

# Step 2: Create IAM Role for Lambda

Go to:

IAM → Roles → Create Role

Trusted Entity:
- AWS Service
- Lambda

Attach the following custom policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances",
        "ec2:CreateImage",
        "ec2:DescribeImages",
        "ec2:CreateTags"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "*"
    }
  ]
}
```

Role Name:
LambdaEC2BackupRole

---

# Step 3: Create Lambda Function

Go to AWS Lambda Console.

Create Function:
- Author from scratch
- Runtime: Python 3.12
- Execution Role: Use existing role
- Select: LambdaEC2BackupRole

Function Name:
ec2-auto-backup

---

# Step 4: Lambda Python Code

Replace default Lambda code with:

```python
import boto3
from datetime import datetime

ec2 = boto3.client('ec2')

def lambda_handler(event, context):

    print("===== Lambda Execution Started =====")

    response = ec2.describe_instances(
        Filters=[
            {
                'Name': 'tag:Backup',
                'Values': ['True']
            },
            {
                'Name': 'instance-state-name',
                'Values': ['running']
            }
        ]
    )

    instances_found = False

    for reservation in response['Reservations']:

        for instance in reservation['Instances']:

            instances_found = True

            instance_id = instance['InstanceId']

            print(f"Found EC2 Instance: {instance_id}")

            ami_name = f"{instance_id}-backup-{datetime.now().strftime('%Y-%m-%d-%H-%M')}"

            print(f"Creating AMI: {ami_name}")

            image = ec2.create_image(
                InstanceId=instance_id,
                Name=ami_name,
                NoReboot=True
            )

            image_id = image['ImageId']

            print(f"AMI Successfully Created: {image_id}")

    if not instances_found:

        print("No EC2 instances found with Backup=True tag")

        return {
            'statusCode': 404,
            'body': 'No tagged EC2 instances found'
        }

    print("===== Lambda Execution Completed Successfully =====")

    return {
        'statusCode': 200,
        'body': 'Backup Completed Successfully'
    }
```

---

# Step 5: Test Lambda Function

Create test event:

```json
{}
```

Click:
- Deploy
- Test

Check created AMIs:

EC2 Console → AMIs

---

# Step 6: Configure Automatic Scheduling Using EventBridge

## Why EventBridge?

Production systems use EventBridge for:
- Scheduled automation
- Cron jobs
- Daily tasks
- Backup orchestration

---

# Create EventBridge Rule

Go to:

Amazon EventBridge → Rules → Create Rule

---

# Rule Configuration

Rule Type:
Schedule

Rule Name:
daily-ec2-backup

---

# Daily Backup Schedule

Cron Expression:

```bash
cron(0 1 * * ? *)
```

Meaning:
- Runs every day
- At 1:00 AM UTC

---

# Every 2 Days Backup Schedule

Option 1 (Recommended Simple Method)

Use Rate Expression:

```bash
rate(2 days)
```

Meaning:
- Runs once every 2 days from creation time

---

# Production Recommendation

## Daily Backups

Production companies usually perform:
- Daily AMI backups
- Especially for critical servers

Typical schedule:
- Midnight or low-traffic hours

---

## Retention Policies

Production environments also:
- delete old AMIs
- keep last 7 or 30 backups

to avoid storage cost increase.

---

# Step 7: Attach Lambda as Target

In EventBridge Rule:
- Select Target
- Choose Lambda Function
- Select:
ec2-auto-backup

Save Rule.

Now backups are fully automated.

---

# Monitoring

Logs are available in:

CloudWatch → Log Groups

Useful for:
- troubleshooting
- monitoring failures
- auditing backups

---

# Production Improvements

Possible enhancements:

- Delete old AMIs automatically
- Send SNS email alerts
- Cross-region backup replication
- Cross-account disaster recovery
- Slack notifications
- Backup reporting dashboard

---

# Understanding AMI vs Snapshot

## Snapshot

- Backup of EBS volume only
- Disk-level backup

Use Cases:
- Database volume backup
- Storage recovery

---

## AMI

- Full EC2 machine image
- Includes:
  - OS
  - packages
  - configuration
  - snapshots

Use Cases:
- Disaster recovery
- Full server restoration
- Infrastructure cloning

---

# Why AMI Is Used Here

This project uses AMI because production environments usually require:

- complete server restoration
- infrastructure recovery
- fast redeployment

---

# Important Note About NoReboot

```python
NoReboot=True
```

Benefits:
- Faster backup
- No downtime

Tradeoff:
- Slight risk of inconsistent filesystem state

Critical production workloads often use:

```python
NoReboot=False
```

for safer backups.

---

# Security Best Practices

- Follow least privilege IAM policy
- Restrict Lambda permissions
- Encrypt EBS snapshots
- Monitor CloudWatch logs
- Rotate old backups

---

# Learning Outcomes

After completing this project you will understand:

- AWS Lambda
- boto3 SDK
- Event-driven automation
- IAM roles
- EC2 APIs
- EventBridge scheduling
- Production backup strategy

---

# Future Enhancements

You can extend this project by adding:

- Snapshot cleanup automation
- Terraform deployment
- Multi-account backup system
- Backup compliance reporting
- AWS Organizations integration

---

# Author

Cloud Automation Practice Project
Using AWS Lambda + EventBridge + EC2
