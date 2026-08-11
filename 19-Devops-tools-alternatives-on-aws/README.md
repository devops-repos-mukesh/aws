# AWS CodeBuild

## Managed Continuous Integration (CI) Build Service

AWS CodeBuild is a fully managed service that compiles source code, runs tests, performs build commands, and produces deployable artifacts without requiring you to manage build servers. It is commonly used as the **CI/build stage** of an AWS DevOps pipeline.

---

## Table of Contents

1. [What is AWS CodeBuild?](#1-what-is-aws-codebuild)
2. [Why Use AWS CodeBuild?](#2-why-use-aws-codebuild)
3. [AWS CodeBuild Architecture](#3-aws-codebuild-architecture)
4. [Core Components](#4-core-components)
5. [CodeBuild Workflow](#5-codebuild-workflow)
6. [Buildspec File](#6-buildspec-file)
7. [Supported Source Providers](#7-supported-source-providers)
8. [Creating a CodeBuild Project Using AWS Console](#8-creating-a-codebuild-project-using-aws-console)
9. [Running the Build](#9-running-the-build)
10. [Common Use Cases](#10-common-use-cases)
11. [Advantages](#11-advantages)
12. [Best Practices](#12-best-practices)
13. [Summary](#13-summary)

---

# 1. What is AWS CodeBuild?

AWS CodeBuild is a fully managed build service used to compile source code, run automated tests, execute build commands, and generate build artifacts. It removes the need to provision and maintain dedicated build servers.

It supports common programming languages and build tools such as Java, Maven, Gradle, Node.js, Python, and Docker.

---

# 2. Why Use AWS CodeBuild?

Without CodeBuild, organizations may need to maintain build servers, install build tools and dependencies, configure build environments, handle scaling, and maintain CI infrastructure.

With CodeBuild, you get managed build environments, automatic scaling, pay-as-you-go usage, and integration with AWS CI/CD services.

---

# 3. AWS CodeBuild Architecture

```text
Developer
    |
    v
Source Repository
(GitHub / GitLab / S3)
    |
    v
AWS CodeBuild
    |
    +--> Install Dependencies
    +--> Pre-Build
    +--> Build
    +--> Test
    +--> Post-Build
    |
    v
Build Artifact
    |
    v
S3 / ECR / Deployment Service
```

---

# 4. Core Components

### Build Project

A CodeBuild project contains the configuration required to execute a build, including source, environment, build commands, artifacts, IAM role, and logging.

### Source

The location from which CodeBuild obtains application source code, such as GitHub, GitLab, Bitbucket, Amazon S3, or AWS CodePipeline.

### Build Environment

The environment in which CodeBuild executes the build. It defines the operating system, runtime, compute resources, and tools available during the build.

### Buildspec

A `buildspec.yml` file defines the commands that CodeBuild executes during different phases of the build.

### Artifacts

Artifacts are files produced by the build, such as JAR, WAR, ZIP, static website files, or Docker images.

### Service Role

An IAM role that gives CodeBuild permission to access required AWS resources.

### CloudWatch Logs

Build logs can be sent to Amazon CloudWatch Logs for monitoring and troubleshooting.

---

# 5. CodeBuild Workflow

```text
Source Code
     |
     v
CodeBuild Project
     |
     v
Build Environment
     |
     v
Install
     |
     v
Pre-Build
     |
     v
Build
     |
     v
Post-Build
     |
     v
Artifacts
```

CodeBuild downloads the source code, creates the configured build environment, reads the buildspec, executes the commands, and produces the configured output.

---

# 6. Buildspec File

The `buildspec.yml` file defines the commands executed by CodeBuild.

Example:

```yaml
version: 0.2

phases:
  install:
    commands:
      - echo Installing dependencies

  pre_build:
    commands:
      - echo Running pre-build steps

  build:
    commands:
      - mvn clean package

  post_build:
    commands:
      - echo Build completed

artifacts:
  files:
    - target/*.jar
```

Common build phases are:

- `install`
- `pre_build`
- `build`
- `post_build`

---

# 7. Supported Source Providers

CodeBuild can obtain source code from:

- GitHub
- GitLab
- Bitbucket
- Amazon S3
- AWS CodePipeline

---

# 8. Creating a CodeBuild Project Using AWS Console

## Step 1: Open CodeBuild

Open the AWS Management Console and navigate to:

```text
AWS Console
→ CodeBuild
→ Build projects
```

Click **Create build project**.

## Step 2: Configure Project

Enter a project name, for example:

```text
my-codebuild-project
```

## Step 3: Configure Source

Choose the required source provider, such as GitHub, GitLab, Bitbucket, Amazon S3, or CodePipeline.

For GitHub, connect your GitHub account and select the repository and source version.

## Step 4: Configure Environment

Choose a managed build environment.

Example:

```text
Environment image → Managed image
Operating system → Linux
Runtime → Standard
Compute type → Small
```

Select the runtime and compute resources appropriate for your application.

## Step 5: Configure Service Role

Choose an existing IAM service role or allow the console to create one.

The role must provide CodeBuild with the permissions required to access the source, artifacts, logs, and other AWS resources used by the build.

## Step 6: Configure Buildspec

Choose:

```text
Use a buildspec file
```

If your repository contains:

```text
buildspec.yml
```

CodeBuild can use it automatically.

## Step 7: Configure Artifacts

If the build produces an output artifact, configure the artifact destination.

For example:

```text
Artifact type → Amazon S3
```

Then select the required S3 bucket.

## Step 8: Configure Logs

Enable CloudWatch Logs so that build output can be viewed and troubleshooting can be performed easily.

## Step 9: Create the Project

Review the configuration and click:

```text
Create build project
```

---

# 9. Running the Build

After creating the project:

```text
CodeBuild
   |
   v
Build projects
   |
   v
Select Project
   |
   v
Start build
```

Click **Start build**.

CodeBuild will download the source code, create the build environment, read the buildspec, execute the build phases, run tests and build commands, and generate the configured artifacts.

You can monitor the build status, phases, logs, duration, and artifacts.

---

# 10. Common Use Cases

AWS CodeBuild is commonly used for:

- Continuous Integration
- Java/Maven builds
- Node.js builds
- Python builds
- Docker image builds
- Automated unit testing
- Application packaging
- Security scanning
- CI/CD pipelines

Typical workflow:

```text
GitHub
   |
   v
CodeBuild
   |
   +--> Build
   +--> Test
   +--> Security Scan
   |
   v
S3 / ECR
   |
   v
CodeDeploy / ECS / EKS
```

---

# 11. Advantages

- Fully managed.
- No build servers to maintain.
- Automatically scalable.
- Pay-as-you-go.
- Supports multiple programming languages and build tools.
- Integrates with AWS CI/CD services.
- Supports automated testing.
- Integrates with CloudWatch Logs.

---

# 12. Best Practices

- Store `buildspec.yml` in source control.
- Use least-privilege IAM roles.
- Never hard-code credentials in `buildspec.yml`.
- Store sensitive values in AWS Secrets Manager or Systems Manager Parameter Store.
- Enable CloudWatch Logs.
- Cache dependencies where appropriate.
- Use separate projects for different applications when required.
- Integrate CodeBuild with CodePipeline for complete CI/CD workflows.

---

# 13. Summary

AWS CodeBuild is a managed CI service that automates compiling, testing, packaging, and building applications. A CodeBuild project defines the source repository, build environment, buildspec, IAM permissions, artifacts, and logging configuration.

A common AWS CI/CD architecture is:

```text
Developer
    |
    v
GitHub
    |
    v
CodeBuild
    |
    +--> Build
    +--> Test
    +--> Scan
    |
    v
Artifact
    |
    v
CodePipeline / CodeDeploy / ECS / EKS
```
