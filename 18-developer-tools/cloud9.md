# AWS Cloud9

<img width="645" height="78" alt="image" src="https://github.com/user-attachments/assets/1c5918bb-9950-49cf-ad94-fd2ccfa1d8ea" />

## Cloud-Based Integrated Development Environment (IDE)

**AWS Cloud9** is a fully managed, cloud-based Integrated Development Environment (IDE) that enables developers to write, run, debug, and collaborate on code directly from a web browser. It comes preconfigured with popular programming languages, SDKs, and developer tools, eliminating the need to install software locally.

Cloud9 provides seamless integration with AWS services and allows developers to build, test, and deploy applications from a single environment.

---

# Table of Contents

1. [What is AWS Cloud9?](#1-what-is-aws-cloud9)
2. [Why Use AWS Cloud9?](#2-why-use-aws-cloud9)
3. [AWS Cloud9 Architecture](#3-aws-cloud9-architecture)
4. [Core Components](#4-core-components)
   - [Cloud9 Environment](#cloud9-environment)
   - [EC2 Instance](#ec2-instance)
   - [IDE Interface](#ide-interface)
   - [Terminal](#terminal)
   - [Collaboration](#collaboration)
5. [Workflow](#5-workflow)
6. [Common Use Cases](#6-common-use-cases)
7. [Advantages](#7-advantages)
8. [Limitations](#8-limitations)
9. [Creating an AWS Cloud9 Environment](#9-creating-an-aws-cloud9-environment)
10. [Best Practices](#10-best-practices)
11. [Cloud9 vs VS Code](#11-cloud9-vs-vs-code)
12. [Pricing](#12-pricing)
13. [Summary](#13-summary)

---

# 1. What is AWS Cloud9?

AWS Cloud9 is a browser-based IDE that provides everything needed to develop applications in the cloud. It includes a code editor, integrated terminal, debugger, and access to AWS resources without requiring local setup.

Cloud9 supports languages such as:

- Python
- Java
- JavaScript
- Node.js
- Go
- PHP
- Ruby
- C++
- C#

---

# 2. Why Use AWS Cloud9?

Without Cloud9:

- Install IDE manually.
- Configure SDKs and CLI.
- Manage dependencies.
- Set up development environments.

With Cloud9:

- Browser-based IDE
- Preconfigured development tools
- Built-in AWS CLI
- Easy collaboration
- Direct access to AWS services
- No local installation required

---

# 3. AWS Cloud9 Architecture

<img width="504" height="603" alt="image" src="https://github.com/user-attachments/assets/68d41d63-8c50-40eb-beef-824e92c81e0d" />


---

# 4. Core Components

## Cloud9 Environment

The workspace where your application code, terminal, and project files are stored.

---

## EC2 Instance

Cloud9 provisions or connects to an Amazon EC2 instance where your code executes.

---

## IDE Interface

Provides:

- Code editor
- File explorer
- Debugger
- Search
- Extensions
- Syntax highlighting

---

## Terminal

Built-in terminal supporting:

- Bash
- AWS CLI
- Git
- Docker
- Python
- Node.js

---

## Collaboration

Allows multiple developers to edit and debug the same project in real time.

---

# 5. Workflow

<img width="504" height="678" alt="image" src="https://github.com/user-attachments/assets/32df337e-0fb9-4a01-b345-3996eb8aa7c0" />


---

# 6. Common Use Cases

- Cloud application development
- Serverless application development
- AWS SDK testing
- Infrastructure automation
- DevOps scripting
- Learning AWS
- Collaborative coding

---

# 7. Advantages

- Fully managed
- Browser-based
- No software installation
- Built-in terminal
- Real-time collaboration
- AWS CLI preinstalled
- Easy integration with AWS services

---

# 8. Limitations

- Depends on an EC2 instance for compute.
- Internet connection required.
- Fewer customization options compared to desktop IDEs.
- AWS announced that Cloud9 is no longer available for new customers, although existing environments continue to function.

---

# 9. Creating an AWS Cloud9 Environment

## Step 1

Sign in to the AWS Management Console.

Navigate to:

```text
AWS Console
→ Cloud9
```

---

## Step 2

Click:

```text
Create Environment
```

---

## Step 3

Enter:

- Environment Name
- Description (Optional)

---

## Step 4

Choose the compute type.

Options include:

- New EC2 Instance
- Existing EC2 Instance (if supported)

---

## Step 5

Configure the EC2 instance.

Example:

- Instance Type: t2.micro
- Platform: Amazon Linux 2
- Timeout Settings

---

## Step 6

Review the configuration.

Click:

```text
Create
```

AWS provisions the environment.

---

## Step 7

The Cloud9 IDE opens automatically.

You can now:

- Create files
- Write code
- Run programs
- Access AWS CLI
- Use Git

---

# 10. Best Practices

- Use IAM roles instead of long-term credentials.
- Stop unused EC2 instances to reduce costs.
- Use Git for version control.
- Enable least-privilege IAM permissions.
- Regularly back up project files.
- Monitor EC2 usage with CloudWatch.

---

# 11. Cloud9 vs VS Code

| Feature | AWS Cloud9 | VS Code |
|----------|------------|----------|
| Installation | Browser-based | Local Installation |
| AWS Integration | Native | Extensions Required |
| Collaboration | Built-in | Live Share Extension |
| Terminal | Built-in | Built-in |
| Offline Support | No | Yes |
| Compute | EC2 Instance | Local Machine |

---

# 12. Pricing

AWS Cloud9 itself has no additional service charge. However, you pay for the AWS resources used by your environment, such as:

- Amazon EC2 instance
- Amazon EBS storage
- Data transfer

---

# 13. Summary

AWS Cloud9 is a cloud-based IDE that enables developers to write, run, debug, and deploy applications directly from a web browser. It integrates seamlessly with AWS services and provides a collaborative development environment without requiring local software installation. Although AWS no longer offers Cloud9 to new customers, it remains a useful service for understanding browser-based cloud development environments.
