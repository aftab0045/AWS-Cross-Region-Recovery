# AWS Backup Cross-Region Disaster Recovery Project

## Project Overview

This project demonstrates a real-world disaster recovery architecture using AWS services.

The project focuses on:

- Automated EC2 backups
- Cross-region disaster recovery
- AWS Backup automation
- Secure backup vault configuration
- Recovery point replication
- EC2 restoration testing
- Backup validation
- Disaster recovery simulation

The infrastructure was deployed using:

- Amazon EC2
- AWS Backup
- IAM
- EBS Encryption
- Cross-Region Backup Vault Replication

---




# Architecture Overview
![](./img/Architect%20Dig.png)
## Primary Region

- North Virginia (`us-east-1`)

Contains:

- EC2 Instance
- Source Backup Vault
- Backup Plan
- Recovery Points

## Disaster Recovery Region

- Singapore (`ap-southeast-1`)

Contains:

- Destination Backup Vault
- Replicated Recovery Points
- Restored EC2 Instance

---

# AWS Services Used

| Service | Purpose |
|---|---|
| Amazon EC2 | Host application server |
| AWS Backup | Automated backup management |
| IAM | Backup permissions |
| EBS | Persistent block storage |
| KMS | EBS volume encryption |
| Apache2 | Web server hosting dashboard |

---

# Project Features

- Automated scheduled EC2 backups
- On-demand backup creation
- Cross-region disaster recovery
- Backup vault lifecycle management
- Secure encrypted EBS backups
- Disaster recovery testing
- EC2 restoration validation
- Interactive dashboard application
- Realistic business data simulation

---

# Step 1 — Launch EC2 Instance

## Open EC2 Console

Go to AWS EC2 Console and switch region to:

```text
North Virginia (us-east-1)
```

---

## Launch EC2

### Configuration

| Setting | Value |
|---|---|
| Name | Backup-Demo-Server |
| AMI | Ubuntu Server 22.04 LTS |
| Instance Type | t2.micro |
| Storage | 8 GB gp3 |
| Key Pair | backup-key.pem |

---

## Security Group Rules

| Type | Port |
|---|---|
| SSH | 22 |
| HTTP | 80 |

SSH source configured as:

```text
My IP
```


![](./img/Screenshot%202026-05-27%20181316.png)


---

# Step 2 — Connect to EC2

## Connect Using SSH

```bash
chmod 400 backup-key.pem
```

```bash
ssh -i backup-key.pem ubuntu@YOUR_PUBLIC_IP
```

---

# Step 3 — Install Apache Web Server

## Update Packages

```bash
sudo apt update
```

---

## Install Apache

```bash
sudo apt install apache2 -y
```

---

## Enable Apache

```bash
sudo systemctl enable apache2
```

```bash
sudo systemctl start apache2
```
![](./img/Screenshot%202026-05-27%20181303.png)

---

# Step 4 — Create Backup Demo Data

## Create Directory

```bash
mkdir backup-data
```

---

## Create customers.txt

```bash
nano backup-data/customers.txt
```

Paste:

```text
Customer ID: CUST-1001
Name: Rahul Sharma
Email: rahul.sharma@example.com
Region: Mumbai
Subscription: Premium

Customer ID: CUST-1002
Name: Aisha Khan
Email: aisha.khan@example.com
Region: Pune
Subscription: Enterprise

Customer ID: CUST-1003
Name: John Mathew
Email: john.mathew@example.com
Region: Bangalore
Subscription: Standard
```

![](./img/Screenshot%202026-05-27%20182249.png)

---

## Create payment.txt

```bash
nano backup-data/payment.txt
```

Paste:

```text
Transaction ID: TXN-9001
Customer: Rahul Sharma
Amount: ₹12,500
Status: Successful

Transaction ID: TXN-9002
Customer: Aisha Khan
Amount: ₹28,000
Status: Successful

Transaction ID: TXN-9003
Customer: John Mathew
Amount: ₹7,500
Status: Pending
```
![](./img/Screenshot%202026-05-27%20182224.png)

---

# Step 5 — Deploy Interactive Dashboard

## Edit Apache Homepage

```bash
sudo nano /var/www/html/index.html
```

Added a professional disaster recovery dashboard containing:

- Cross-region backup information
- Disaster recovery status
- Interactive validation button
- Monitoring dashboard
- Security information
- System health cards

---

## Restart Apache

```bash
sudo systemctl restart apache2
```

---

# Step 6 — Verify Application

Open browser:

```text
http://YOUR_PUBLIC_IP
```
![](./img/Screenshot%202026-05-27%20182450.png)

Verified:

- Interactive dashboard working
- Apache service active
- EC2 accessible publicly

---

# Step 7 — Create IAM Role for AWS Backup

## Open IAM Console

Created role:

```text
AWSBackupDefaultServiceRole
```
![](./img/Screenshot%202026-05-27%20182608.png)

---

## Attached Policies

| Policy |
|---|
| AWSBackupServiceRolePolicyForBackup |
| AWSBackupServiceRolePolicyForRestores |

![](./img/Screenshot%202026-05-27%20182659.png)

![](./img/Screenshot%202026-05-27%20182754.png)
---

# Step 8 — Create Backup Vaults

## Source Vault

### Region

```text
North Virginia (us-east-1)
```

### Vault Name

```text
primary-backup-vault
```

![](./img/Screenshot%202026-05-27%20183006.png)

![](./img/Screenshot%202026-05-27%20183022.png)

---

## Destination Vault

### Region

```text
Singapore (ap-southeast-1)
```

### Vault Name

```text
disaster-recovery-vault
```

![](./img/Screenshot%202026-05-27%20183144.png)


---

# Step 9 — Create Backup Plan

## Backup Plan Name

```text
ec2-cross-region-backup-plan
```

---

## Backup Rule Configuration

| Setting | Value |
|---|---|
| Rule Name | daily-ec2-backup |
| Backup Frequency | Daily |
| Backup Vault | primary-backup-vault |
| Start Window | 1 Hour |
| Completion Window | 7 Days |
| Retention | 120 Days |


![](./img/Screenshot%202026-05-27%20183439.png)

![](./img/Screenshot%202026-05-27%20183641.png)

---

# Step 10 — Configure Cross-Region Replication

## Copy Configuration

| Setting | Value |
|---|---|
| Destination Region | Singapore |
| Destination Vault | disaster-recovery-vault |
| Retention | 120 Days |

This configuration automatically replicates recovery points to the disaster recovery region.

![](./img/Screenshot%202026-05-27%20183950.png)

---

# Step 11 — Assign EC2 Resource

## Resource Assignment

| Setting | Value |
|---|---|
| Assignment Name | ec2-assignment |
| Resource Type | EC2 |
| Assigned Instance | Backup-Demo-Server |

![](./img/Screenshot%202026-05-27%20183620.png)

---

# Step 12 — On-Demand Backup Testing

## Manual Backup Creation

Created an on-demand backup manually to test:

- Recovery point generation
- Backup vault functionality
- AWS Backup integration
- Initial backup verification

![](./img/Screenshot%202026-05-27%20184313.png)

---

## Verified Backup Job

Verified:

```text
Backup Jobs → Completed
```

![](./img/Screenshot%202026-05-27%20191819.png)


---

# Step 13 — Automated Scheduled Backup Testing

## Scheduled Backup Validation

Configured scheduled backup automation using AWS Backup plan rules.

The scheduled backup automatically:

- Created EC2 backup
- Applied lifecycle policy
- Triggered cross-region replication
- Generated recovery points in Singapore

![](./img/Screenshot%202026-05-27%20195032.png)
---

## Verified Automatic Backup

Verified:

```text
Backup Jobs → Completed
```

---

## Verified Copy Job

Verified:

```text
Copy Jobs → Completed
```

![](./img/Screenshot%202026-05-27%20204653.png)


This confirmed successful cross-region disaster recovery replication.

---

# Step 14 — Verify Recovery Point in Singapore

## Open Singapore Region

Verified replicated recovery point inside:

```text
disaster-recovery-vault
```

This confirmed:

- Disaster recovery readiness
- Cross-region backup replication
- Recovery point synchronization

![](./img/Screenshot%202026-05-27%20204851.png)
---

# Step 15 — Disaster Recovery Restore Testing

## Restore EC2 Instance

Performed restoration testing from Singapore recovery point.

Restore testing validated:

- Backup integrity
- Disaster recovery functionality
- Cross-region restoration
- EC2 recovery workflow

![](./img/Screenshot%202026-05-27%20193512.png)





---

## Restore Issue Encountered

During restoration, the following error occurred:

```text
Cannot create unencrypted device /dev/sda1 from encrypted snapshot
```

---

## Resolution

Resolved the issue by:

- Enabling EBS encryption
- Using active KMS encryption configuration
- Restoring encrypted EBS volumes correctly

---

# Step 16 — Validation Testing

## SSH Validation

Connected to restored EC2 instance:

![](./img/Screenshot%202026-05-27%20193633.png)

```bash
ssh -i backup-key.pem ubuntu@RESTORED_PUBLIC_IP
```

---

## Verify Restored Data

```bash
ls backup-data
```

Expected output:

```text
customers.txt
payment.txt
```
![](./img/Screenshot%202026-05-27%20194113.png)

---

## Verify Application

Opened restored application:

```text
http://RESTORED_PUBLIC_IP
```
![](./img/Screenshot%202026-05-27%20194038.png)

Verified:

- Dashboard accessible
- Apache running
- Backup files restored successfully

---


# Common Issues Faced

## Issue 1 — Copy Jobs Empty

### Reason

On-demand backup did not use backup plan copy configuration.

### Solution

Validated scheduled backup automation using backup plan rule execution.

---

## Issue 2 — Restore Failed

### Error

```text
Cannot create unencrypted device from encrypted snapshot
```

### Solution

Restored EC2 using encrypted EBS configuration with active KMS key.

---

## Issue 3 — KMS Key Disabled

### Reason

Selected KMS key was disabled in Singapore region.

### Solution

Enabled the KMS key and retried restore successfully.

---

# Security Best Practices Implemented

- Restricted SSH access
- IAM least privilege
- Encrypted EBS volumes
- Cross-region disaster recovery
- Automated backup retention policies

---

# Project Outcome

Successfully implemented:

- Automated EC2 backup solution
- Cross-region disaster recovery architecture
- AWS Backup automation
- Recovery point replication
- EC2 restoration testing
- Disaster recovery validation

---

