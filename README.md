# ControlTower_aws

<img width="575" alt="image" src="https://github.com/user-attachments/assets/8faba9d8-34f4-4afb-b0c9-b447cca427c9" />

#   AWS Control Tower Setup Guide

This guide walks you through setting up a secure multi-account environment in **AWS** using **Control Tower**, with detailed instructions tailored for your setup.

---

##  Account Overview

| Role              | Account Name    | Account ID     | Email Address              |
|------------------|------------------|----------------|----------------------------|
| Root / Management| `saikrishna.k`   | `623378151578` | saikakumanu2025@gmail.com  |
| Sandbox           | `sandbox`        | `278713373449` | saisandbox2025@gmail.com   |

---

##   Step 1: Set Up AWS Control Tower

1. **Log in** to your **Management account (623378151578)** at [AWS Console](https://console.aws.amazon.com/).
2. In the top search bar, search for **“Control Tower”** and open it.
3. Click on **“Set up landing zone”**.

---

###  What Is a Landing Zone?

A **Landing Zone** is a clean, secure, and automated environment that AWS sets up to help you manage multiple accounts. It includes:

- **Log Archive Account**  
  > Stores logs from all your accounts — like a camera recording every action.

- **Audit Account**  
  > Used for security checks — like an inspector.

- **Security Guardrails**  
  > Predefined rules such as:
  -  No public access to data
  -  Restrict usage to approved AWS regions

- **Organizational Units (OUs)**  
  > Logical folders to group accounts (e.g., Sandbox, Production)

- **CloudTrail & AWS Config**  
  > Tracks and logs all actions and changes in your AWS accounts.

---

##  Organizational Units (OUs) Configuration

- **Home Region**: _(choose your preferred AWS region)_
- **OU Name**: `Security`
- **Recommended OU**: `Sandbox-ou-created-by-control-tower`
- **Additional OU**: For any production or development accounts

---

##  Account Creation During Setup

When the landing zone is created, AWS will automatically generate two important accounts:

| Purpose           | Description                                   |
|-------------------|-----------------------------------------------|
| **Log Archive**   | Stores logs from all accounts                 |
| **Audit Account** | Monitors and inspects activity across accounts|

---

##  AWS CloudTrail & S3 Log Configuration

### CloudTrail
- Captures activity logs and events across all accounts.
- Integrated with AWS Control Tower.

### Amazon S3 Logging Defaults:
-  General logs: **1 year retention**
-  Access logs: **10 years retention**

---

##  Manual Step: Add Execution Role to Sandbox Account

To enroll the sandbox account into Control Tower, you need to manually add the required IAM execution role.

---

###  Step-by-Step Instructions

#### **Part 1: Log in to the Sandbox Account**
- Log in to **sandbox account (278713373449)** at [AWS Console](https://console.aws.amazon.com/)
- Use **Admin** or **Root user**

#### **Part 2: Create Role `AWSControlTowerExecution`**

1. Open **IAM > Roles > Create role**
2. Choose **Trusted Entity**:
   - Select **Another AWS Account**
   - Enter **Management Account ID**: `623378151578`
3. **Permissions**:
   - Attach policy: `AdministratorAccess` *(temporary, can be restricted later)*
4. **Role Name**:
   - Set to **`AWSControlTowerExecution`** (must match exactly)
5. Complete and create the role.

>  Note: Role created by Account: `623378151578`

---

#### **Part 3: Enroll the Sandbox Account**

1. Log back into the **Management account**
2. Navigate to **AWS Control Tower > Accounts**
3. Select the **Sandbox account (278713373449)**
4. Click **Enroll**
5. Choose OU: `Sandbox-ou-created-by-control-tower`

---

##  Post-Enrollment: Audit Account Info

| Field                    | Value                      |
|--------------------------|----------------------------|
| **Account ID**           | `246217239453`             |
| **Account Name**         | `Audit account`            |
| **Account Email**        | auditsai2025@gmail.com     |
| **OU**                   | `Security`                 |
| **Compliance Status**    |  Unknown                 |
| **Controls Enabled**     | Preventive<br>3 Detective<br>0 Proactive |
| **CloudTrail**           | Enabled                |
| **AWS Backup**           |  Not enabled            |
| **IAM Identity Center**  |  Not configured yet      |

>  Controls owned by **AWS Security Hub** are not aggregated into the Control Tower compliance dashboard.  
>  It's recommended to manually review Security Hub findings.
