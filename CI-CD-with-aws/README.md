# AWS CI/CD Pipeline Architecture

**Continuous Integration and Continuous Delivery using AWS**

AWS provides a set of managed services that can be combined to build an automated CI/CD pipeline. A typical pipeline automatically detects source-code changes, builds and tests the application, performs quality/security checks, packages the application, and deploys it to the target environment.

AWS CodePipeline is the orchestration service that connects the different stages of the release process. CodeBuild can compile code, run tests, and generate deployment artifacts. CodePipeline can then pass those artifacts to deployment services such as CodeDeploy, Amazon ECS, Lambda, or other supported targets.

## Table of Contents

- [What is CI/CD?](#1-what-is-cicd)
- [AWS CI/CD Services](#2-aws-cicd-services)
- [CI/CD Pipeline Architecture](#3-cicd-pipeline-architecture)
- [Pipeline Workflow](#4-pipeline-workflow)
- [AWS Services Used](#5-aws-services-used)
- [Creating the CI/CD Pipeline](#6-creating-the-cicd-pipeline)
  - [Step 1 - Create Source Repository](#step-1---create-source-repository)
  - [Step 2 - Create CodeBuild Project](#step-2---create-codebuild-project)
  - [Step 3 - Create Deployment Target](#step-3---create-deployment-target)
  - [Step 4 - Create CodePipeline](#step-4---create-codepipeline)
  - [Step 5 - Configure Source Stage](#step-5---configure-source-stage)
  - [Step 6 - Configure Build Stage](#step-6---configure-build-stage)
  - [Step 7 - Configure Deploy Stage](#step-7---configure-deploy-stage)
  - [Step 8 - Test the Pipeline](#step-8---test-the-pipeline)
- [Buildspec Example](#buildspec-example)
- [IAM Roles](#iam-roles)
- [Security and Quality Checks](#security-and-quality-checks)
- [Monitoring](#monitoring)
- [Production CI/CD Architecture](#production-cicd-architecture)
- [Recommended AWS CI/CD Learning Path](#recommended-aws-cicd-learning-path)
- [Best Practices](#best-practices)
- [Summary](#summary)

---

## 1. What is CI/CD?

### Continuous Integration

Continuous Integration (CI) is the practice of frequently integrating code changes into a shared repository and automatically building and testing those changes.

Typical CI process:

<img width="804" height="804" alt="image" src="https://github.com/user-attachments/assets/5673a5f3-c512-4582-b2d9-78e8651d2f95" />


The goal is to identify compilation errors, test failures, and code-quality problems as early as possible.

### Continuous Delivery

Continuous Delivery (CD) extends CI by automatically preparing validated application changes for deployment.

<img width="800" height="795" alt="image" src="https://github.com/user-attachments/assets/0f77f616-3d33-4a33-8a6b-14fc8c996ac0" />


AWS describes CodePipeline as a continuous delivery service that automates building, testing, and deployment activities.

## 2. AWS CI/CD Services

A complete AWS CI/CD architecture can use the following services:

| Stage | AWS Service | Purpose |
|---|---|---|
| Source | GitHub / CodeCommit / S3 | Store source code |
| Orchestration | CodePipeline | Coordinate pipeline stages |
| Build | CodeBuild | Compile and build application |
| Testing | CodeBuild | Run automated tests |
| Artifact Storage | S3 | Store build artifacts |
| Package Management | CodeArtifact | Store Maven/npm/Python packages |
| Deployment | CodeDeploy | Deploy applications |
| Containers | ECR | Store Docker images |
| Container Deployment | ECS / EKS | Run containers |
| Serverless Deployment | Lambda | Deploy serverless applications |
| Infrastructure | CloudFormation / CDK | Provision infrastructure |
| Security | Inspector / IAM / Secrets Manager | Security and secret management |
| Monitoring | CloudWatch | Logs, metrics, alarms |
| Notifications | SNS | Send deployment notifications |

AWS CodePipeline supports different source providers and can integrate CodeBuild with deployment targets such as CodeDeploy, Elastic Beanstalk, ECS, and others.

## 3. CI/CD Pipeline Architecture

A typical AWS CI/CD architecture can look like this:
<img width="497" height="1311" alt="Untitled-2026-08-11-1132" src="https://github.com/user-attachments/assets/2c4aea4b-1ba3-46ec-b0fb-4921ac74295f" />


CodePipeline represents the release workflow as stages and actions, allowing build, test, deployment, and other actions to be connected into one automated process.

## 4. Pipeline Workflow

The complete workflow is:

<img width="267" height="1070" alt="Untitled-2026-08-11-1132" src="https://github.com/user-attachments/assets/a0b540c8-6b90-49fc-98ad-d7bd8b2593f1" />


## 5. AWS Services Used

### AWS CodePipeline

CodePipeline is the orchestration layer.

It connects:

```
Source → Build → Test → Deploy
```

It automatically moves artifacts between pipeline stages.

### AWS CodeBuild

CodeBuild is the CI/build engine.

It can:

- Compile source code
- Run unit tests
- Run integration tests
- Package applications
- Generate artifacts
- Run code-quality tools
- Run security scanners

CodeBuild is a fully managed build service and can process builds without requiring you to manage build servers.

### Amazon S3

S3 can be used to store:

- Pipeline artifacts
- Build packages
- ZIP files
- Deployment packages
- Logs or reports

CodePipeline commonly uses an S3 artifact store to pass artifacts between stages.

### AWS CodeDeploy

CodeDeploy automates application deployments to supported compute environments such as EC2.

Example:

<img width="800" height="795" alt="image" src="https://github.com/user-attachments/assets/d39fb97d-3795-4eb0-9b02-f443e149fc45" />


### Amazon ECR

For containerized applications:


<img width="800" height="795" alt="image" src="https://github.com/user-attachments/assets/2454c53d-3b4d-4f6f-8905-7c0b60318d6f" />

### Amazon CloudWatch

CloudWatch can be used to monitor:

- CodeBuild logs
- Application logs
- Deployment events
- Metrics
- Alarms

## 6. Creating the CI/CD Pipeline

The following example creates:

<img width="800" height="795" alt="image" src="https://github.com/user-attachments/assets/38263e5c-32a1-4afe-9dc0-0d9ac5335982" />


For a simple learning environment, you can start with:

<img width="800" height="795" alt="image" src="https://github.com/user-attachments/assets/31a62408-f3a6-4384-8ca3-95f0af5c63c1" />


and add CodeDeploy/ECS/Lambda later.

### Step 1 - Create Source Repository

You can use:

- GitHub
- AWS CodeCommit
- Bitbucket
- Amazon S3

For this example, use GitHub.

Create a repository containing your application:

```
my-java-application/
├── src/
├── pom.xml
└── buildspec.yml
```

Commit and push the project:

```bash
git add .
git commit -m "Initial commit"
git push origin main
```

CodePipeline can use a source repository as the trigger for the pipeline when source changes are detected.

### Step 2 - Create CodeBuild Project

Open:

```
AWS Console
    ↓
CodeBuild
    ↓
Build projects
    ↓
Create build project
```

Configure:

**Project configuration**

- Project name: `my-codebuild-project`

**Source**

Select:

- Source provider: `GitHub`

Connect your GitHub account and select:

- Repository
- Branch

**Environment**

For a standard Java/Maven application:

- Environment image: `Managed image`
- Compute: `EC2`
- Running mode: `Container`
- Operating system: `Ubuntu`
- Runtime: `Standard`
- Compute type: `Small`

**Buildspec**

Choose:

- Use a buildspec file
- Keep: `buildspec.yml`

CodeBuild can be used as a build action inside CodePipeline.

### Step 3 - Create Deployment Target

You can choose different deployment targets depending on your application.

**EC2**

```
CodePipeline
     |
     v
CodeBuild
     |
     v
CodeDeploy
     |
     v
EC2
```

**ECS**

```
CodePipeline
     |
     v
CodeBuild
     |
     v
ECR
     |
     v
ECS
```

**Lambda**

```
CodePipeline
     |
     v
CodeBuild
     |
     v
Lambda
```

For your first CI/CD project, EC2 + CodeDeploy is a good way to understand the traditional deployment workflow.

### Step 4 - Create CodePipeline

Open:

```
AWS Console
    ↓
CodePipeline
    ↓
Create pipeline
```

Enter:

- Pipeline name: `my-cicd-pipeline`

Select or create the CodePipeline service role.

AWS provides a console wizard for creating pipelines and adding CodeBuild actions.

### Step 5 - Configure Source Stage

Select:

- Source provider: `GitHub`

Connect to GitHub.

Select:

- Repository: `my-java-application`
- Branch: `main`

Enable automatic change detection.

The workflow becomes:

```
GitHub
   |
   | Push
   v
CodePipeline
   |
   v
Source Stage
```

### Step 6 - Configure Build Stage

Select:

- Build provider: `AWS CodeBuild`

Select your project:

- `my-codebuild-project`

The workflow becomes:

```
GitHub
   |
   v
CodePipeline
   |
   v
Source
   |
   v
CodeBuild
```

CodePipeline passes the source artifact to CodeBuild, which uses the buildspec to perform the build and tests.

### Step 7 - Configure Deploy Stage

For EC2 deployment:

- Deploy provider: `AWS CodeDeploy`

Select:

- Application
- Deployment group

The resulting pipeline becomes:

```
GitHub
   ↓
CodePipeline
   ↓
CodeBuild
   ↓
CodeDeploy
   ↓
EC2
```

You can also deploy to ECS, Lambda, Elastic Beanstalk, or other supported targets.

### Step 8 - Test the Pipeline

Make a change to your application:

```bash
echo "new change" >> README.md
```

Commit:

```bash
git add .
git commit -m "Test CI/CD pipeline"
git push origin main
```

The pipeline should automatically execute:

```
Source
  ↓
Build
  ↓
Test
  ↓
Deploy
```

Open:

```
AWS Console
    ↓
CodePipeline
    ↓
my-cicd-pipeline
```

You can see the execution of each stage.

## Buildspec Example

For a Maven-based Java application:

```yaml
version: 0.2

phases:

  install:
    commands:
      - echo Installing dependencies

  pre_build:
    commands:
      - echo Running pre-build phase
      - mvn --version

  build:
    commands:
      - echo Build started
      - mvn clean package

  post_build:
    commands:
      - echo Build completed

artifacts:
  files:
    - target/*.jar
```

For a WAR application:

```yaml
artifacts:
  files:
    - target/*.war
```

## IAM Roles

A CI/CD pipeline requires several IAM roles.

### CodePipeline Role

CodePipeline needs permission to interact with services such as:

- CodeBuild
- S3
- CodeDeploy
- CloudFormation
- Lambda
- ECS

### CodeBuild Role

CodeBuild may require access to:

- S3
- CloudWatch Logs
- ECR
- CodeArtifact
- Secrets Manager
- SSM

depending on the build.

### CodeDeploy Role

CodeDeploy requires permissions to perform deployment operations.

For example:

```
CodeDeploy
    |
    +---- EC2
    |
    +---- S3
    |
    +---- CloudWatch
```

AWS's CodePipeline documentation emphasizes that the pipeline service role must have permission to interact with the services used by the pipeline.

> Use least-privilege IAM policies in production rather than broad FullAccess policies.

## Security and Quality Checks

A production CI/CD pipeline can add security and quality stages:

<img width="1440" height="1424" alt="image" src="https://github.com/user-attachments/assets/4605aacf-9477-4516-99bd-bde956bc2c7c" />


Possible tools:

| Tool | Purpose |
|---|---|
| SonarQube | Static code analysis |
| Trivy | Container/filesystem vulnerability scanning |
| OWASP Dependency-Check | Dependency vulnerability scanning |
| Gitleaks | Secret detection |
| CodeBuild | Execute the checks |
| CodePipeline | Orchestrate the stages |

AWS Prescriptive Guidance also demonstrates CI/CD pipelines containing testing and security validation stages before deployment.

## Monitoring

Use Amazon CloudWatch for monitoring.

<img width="1440" height="1152" alt="image" src="https://github.com/user-attachments/assets/3242e680-dfa2-46c0-848e-eb5ed6a60205" />


Monitor:

- Build failures
- Deployment failures
- Application errors
- CPU/memory
- Request metrics
- Logs
- Alarms

## Production CI/CD Architecture

A more complete architecture can look like:

<img width="1440" height="1464" alt="image" src="https://github.com/user-attachments/assets/1e0a5486-aa4b-4d8f-89af-2cd6f8d50903" />

<img width="1440" height="1168" alt="image" src="https://github.com/user-attachments/assets/9e12c25f-8ccd-4020-818a-05ee34d3dfe4" />


## Recommended AWS CI/CD Learning Path

For learning AWS DevOps, build your pipeline incrementally:

**Level 1 — Basic CI**

```
GitHub
   ↓
CodeBuild
   ↓
Build + Test
```

**Level 2 — CI/CD**

```
GitHub
   ↓
CodePipeline
   ↓
CodeBuild
   ↓
S3
```

**Level 3 — EC2 Deployment**

```
GitHub
   ↓
CodePipeline
   ↓
CodeBuild
   ↓
CodeDeploy
   ↓
EC2
```

**Level 4 — Container CI/CD**

```
GitHub
   ↓
CodePipeline
   ↓
CodeBuild
   ↓
ECR
   ↓
ECS
```

**Level 5 — Production Pipeline**

```
GitHub
   ↓
CodePipeline
   ↓
CodeBuild
   ↓
Unit Tests
   ↓
SonarQube
   ↓
Security Scan
   ↓
S3 / ECR
   ↓
Staging
   ↓
Manual Approval
   ↓
Production
   ↓
CloudWatch
```

## Best Practices

- Use separate build and deployment stages.
- Store build artifacts in S3 or container images in ECR.
- Use IAM roles with least privilege.
- Keep secrets out of source code and `buildspec.yml`.
- Use AWS Secrets Manager or Systems Manager Parameter Store for secrets.
- Run unit tests before deployment.
- Add static code analysis and vulnerability scanning.
- Use separate staging and production environments.
- Add manual approval before production when required.
- Enable CloudWatch logging.
- Use deployment strategies such as rolling, blue/green, or canary where appropriate.
- Keep infrastructure as code using CloudFormation, CDK, or Terraform.
- Monitor pipeline failures and deployment metrics.

## Summary

An AWS CI/CD pipeline automates the software delivery process from source-code changes to production deployment.

The core architecture is:

```
Developer
    ↓
GitHub
    ↓
CodePipeline
    ↓
CodeBuild
    ↓
Test / Security / Quality
    ↓
Artifact
    ↓
CodeDeploy / ECS / Lambda
    ↓
Application
    ↓
CloudWatch
```

The most important AWS services to understand are:

| Service | Role |
|---|---|
| CodePipeline | Orchestration |
| CodeBuild | Build & Test |
| S3 | Artifacts |
| CodeDeploy | EC2 Deployment |
| ECR | Container Images |
| ECS/EKS | Container Deployment |
| Lambda | Serverless Deployment |
| CloudWatch | Monitoring |
| IAM | Access Control |
| Secrets Manager | Secrets |
| CodeArtifact | Package Management |
