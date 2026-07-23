# AWS Managed Microsoft Active Directory (AWS Directory Service)

This guide explains how to create an **AWS Managed Microsoft Active Directory (AD)** using the AWS Management Console. AWS Directory Service provides a fully managed Microsoft Active Directory that can be used with Windows EC2 instances, Amazon FSx, Amazon WorkSpaces, and other AWS services.

---

# Table of Contents

1. [Prerequisites](#1-prerequisites)
2. [Open AWS Directory Service](#2-open-aws-directory-service)
3. [Choose Directory Type](#3-choose-directory-type)
4. [Select Directory Edition](#4-select-directory-edition)
5. [Configure Directory Details](#5-configure-directory-details)
6. [Configure Networking](#6-configure-networking)
7. [Review and Create Directory](#7-review-and-create-directory)
8. [Verify Directory Creation](#8-verify-directory-creation)
9. [Join an EC2 Instance to the Domain](#9-join-an-ec2-instance-to-the-domain)
10. [Architecture](#10-architecture)
11. [Best Practices](#11-best-practices)

---

# 1. Prerequisites

Before creating the directory, ensure you have:

* An AWS account
* A VPC
* At least **two subnets in different Availability Zones**
* IAM permissions to create AWS Directory Service resources

Example:

```text
VPC: 10.0.0.0/16

Private Subnet A
10.0.1.0/24

Private Subnet B
10.0.2.0/24
```
<img width="1919" height="961" alt="image" src="https://github.com/user-attachments/assets/0a75d0ae-1ee8-479e-bc7d-2995313b4052" />

---

# 2. Open AWS Directory Service

1. Sign in to the **AWS Management Console**.
2. Search for **Directory Service**.
3. Open **AWS Directory Service**.
4. Click **Set up directory** (or **Create directory**).

---

# 3. Choose Directory Type

AWS provides three directory options:

### AWS Managed Microsoft AD (Recommended)

* Fully managed Microsoft Active Directory
* Supports Group Policies (GPO)
* Supports domain join
* Supports trust relationships
* Best for enterprise workloads

### AD Connector

* Connects AWS to an existing on-premises Active Directory.
* No directory data is stored in AWS.

### Simple AD

* Lightweight directory based on Samba.
* Suitable for small applications.

> **Recommendation:** Choose **AWS Managed Microsoft AD**.

<img width="1919" height="961" alt="image" src="https://github.com/user-attachments/assets/d770a2c0-628f-4fd1-a1f8-4349cb99c1a0" />

---

# 4. Select Directory Edition

Choose one of the following editions:

### Standard Edition

Suitable for:

* Small and medium-sized organizations
* Lower cost
* Moderate number of directory objects

### Enterprise Edition

Suitable for:

* Large enterprises
* Higher scalability
* Large number of users and objects

<img width="1919" height="961" alt="image" src="https://github.com/user-attachments/assets/ece2ae92-ae47-4be7-9335-2a5cc802d881" />

---

# 5. Configure Directory Details

Provide the following information:

| Field                  | Example                               |
| ---------------------- | ------------------------------------- |
| Directory DNS Name     | `corp.example.com`                    |
| NetBIOS Name           | `CORP`                                |
| Administrator Password | Strong password                       |
| Description            | Corporate Active Directory (Optional) |

Click **Next** after entering the required details.

<img width="1919" height="961" alt="image" src="https://github.com/user-attachments/assets/f8e8255e-32d7-40fe-b049-63c7397e93a5" />

---

# 6. Configure Networking

Select:

* Your **VPC**
* **Two private subnets** located in different Availability Zones

AWS automatically deploys two domain controllers for high availability.

Example:

```text
VPC
│
├── Private Subnet (AZ-1)
└── Private Subnet (AZ-2)
```

Click **Next**.

<img width="1919" height="961" alt="image" src="https://github.com/user-attachments/assets/e11bb0ac-b982-4047-8720-220df2c2dd48" />

---

# 7. Review and Create Directory

Review the configuration:

* Directory Type
* Edition
* DNS Name
* NetBIOS Name
* VPC
* Subnets

Click **Create Directory**.

The directory creation process typically takes **20–45 minutes**.

<img width="1919" height="961" alt="image" src="https://github.com/user-attachments/assets/dd59a185-44a8-4b7a-aacf-1ef15265e84c" />
<img width="1919" height="961" alt="image" src="https://github.com/user-attachments/assets/2baa8411-71da-4dfd-8d4e-ed6d14fdb1b0" />

---

# 8. Verify Directory Creation

Once provisioning is complete, the directory status changes from:

```text
Creating
```

to

```text
Active
```

You can view details such as:

* Directory ID
* DNS Name
* VPC ID
* DNS Server IP Addresses
* Connected Subnets
* Edition

<img width="1919" height="961" alt="image" src="https://github.com/user-attachments/assets/a880551d-8b1f-4870-8e9b-6db30f8b2955" />

---

# 9. Join an EC2 Instance to the Domain

After the directory becomes active:

1. Launch a **Windows EC2 instance** in the same VPC.
2. Connect using **Remote Desktop (RDP)**.
3. Open **System Properties**.
4. Select **Change** under Computer Name.
5. Choose **Domain**.
6. Enter your directory DNS name (e.g., `corp.example.com`).
7. Provide the administrator credentials.
8. Restart the instance.

The EC2 instance is now joined to the Active Directory domain.

---

# 10. Architecture

<img width="695" height="710" alt="Untitled-2026-07-23-1225" src="https://github.com/user-attachments/assets/25d297f3-40b7-47ff-be65-e427bfc18c73" />

---

# 11. Best Practices

* Deploy the directory in **private subnets**.
* Use at least **two Availability Zones** for high availability.
* Choose a strong administrator password.
* Apply the principle of least privilege using IAM.
* Restrict administrative access with security groups.
* Monitor the directory using Amazon CloudWatch.
* Integrate with services like Amazon FSx, Amazon WorkSpaces, and Windows EC2 where appropriate.

---

# Summary

AWS Managed Microsoft AD provides a fully managed Active Directory environment without requiring you to manage domain controllers. The setup process involves selecting the directory type, configuring the domain details, choosing a VPC and subnets, and allowing AWS to provision redundant domain controllers. Once the directory is active, Windows instances and supported AWS services can securely authenticate and integrate with the domain.
