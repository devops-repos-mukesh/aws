# AWS Step Functions

## Workflow Orchestration

AWS Step Functions is a fully managed, serverless workflow orchestration service that enables you to coordinate multiple AWS services into a sequence of steps called a **State Machine**. It helps automate business processes by managing the execution flow, handling retries, managing errors, and maintaining workflow state without requiring custom orchestration code.

Step Functions integrates with over 200 AWS services, including AWS Lambda, Amazon S3, AWS Glue, Amazon SNS, Amazon SQS, Amazon DynamoDB, Amazon SageMaker, Amazon ECS, and many more.

---

# Table of Contents

1. [What is AWS Step Functions?](#1-what-is-aws-step-functions)
2. [Why Use Step Functions?](#2-why-use-step-functions)
3. [Workflow Architecture](#3-workflow-architecture)
4. [Core Components](#4-core-components)
   - [State Machine](#state-machine)
   - [States](#states)
   - [Task State](#task-state)
   - [Wait State](#wait-state)
   - [Choice State](#choice-state)
   - [Success State](#success-state)
   - [Fail State](#fail-state)
5. [Workflow Used in this Demo](#5-workflow-used-in-this-demo)
6. [Creating a Workflow using AWS Console](#6-creating-a-workflow-using-aws-console)
7. [State Machine Definition](#7-state-machine-definition)
8. [Best Practices](#8-best-practices)
9. [Summary](#9-summary)

---

# 1. What is AWS Step Functions?

AWS Step Functions is a serverless orchestration service that allows you to connect multiple AWS services into a visual workflow.

Instead of writing application logic to coordinate different services, Step Functions manages:

- Workflow execution
- State management
- Error handling
- Retries
- Conditional branching
- Parallel execution
- Logging and monitoring

A workflow is represented as a **State Machine**, where each state performs a specific task.

---

# 2. Why Use Step Functions?

Without Step Functions:

- Write custom orchestration code
- Handle retries manually
- Implement timeout logic
- Maintain execution state
- Build conditional workflows

With Step Functions:

- Serverless workflow orchestration
- Visual workflow designer
- Automatic retries
- Built-in error handling
- State tracking
- Integration with AWS services

---

# 3. Workflow Architecture

```text
                Client
                   │
                   ▼
        AWS Step Functions
                   │
    ┌──────────────┼──────────────┐
    ▼              ▼              ▼
 Amazon S3     AWS Lambda      AWS Glue
                   │
                   ▼
             Success / Fail
```

Each step is executed sequentially unless configured for parallel execution.

---

# 4. Core Components

## State Machine

A State Machine is the complete workflow definition.

It contains:

- States
- Transitions
- Input and Output
- Retry policies
- Error handling

---

## States

Each step inside a workflow is called a **State**.

Common state types include:

- Task
- Choice
- Wait
- Parallel
- Map
- Pass
- Succeed
- Fail

---

## Task State

Performs an action such as:

- Invoke AWS Lambda
- Create an S3 bucket
- Start a Glue Job
- Publish an SNS message

---

## Wait State

Pauses the workflow for:

- Fixed number of seconds
- Timestamp
- Duration

---

## Choice State

Implements conditional logic similar to an **if-else** statement.

Example:

```text
Object Exists?
      │
 ┌────┴────┐
 │         │
Yes        No
 │         │
Success   Fail
```

---

## Success State

Marks successful completion of the workflow.

---

## Fail State

Stops workflow execution due to an error or failed condition.

---

# 5. Workflow Used in this Demo

This workflow demonstrates how Step Functions can orchestrate Amazon S3 operations.

```text
Start
   │
   ▼
CreateBucket
   │
   ▼
Wait (10 sec)
   │
   ▼
CopyObject
   │
   ▼
Wait (5 sec)
   │
   ▼
Choice
   │
 ┌─┴─────────┐
 │           │
 ▼           ▼
Success     Fail
```

### Workflow Explanation

### Step 1 – CreateBucket

Creates a new Amazon S3 bucket.

---

### Step 2 – Wait

Waits for **10 seconds** before executing the next step.

---

### Step 3 – CopyObject

Copies an existing object from a source S3 bucket into the newly created bucket.

---

### Step 4 – Wait

Waits for **5 seconds** to allow the copy operation to complete.

---

### Step 5 – Choice

Checks whether the object copy was successful using the JSONata expression:

```text
{% $exists($states.input.CopyObjectResult.ETag) %}
```

If the **ETag** exists:

→ Success

Otherwise:

→ Fail

---

# 6. Creating a Workflow using AWS Console

## Step 1

Open the AWS Management Console.

Navigate to:

```text
AWS Console
→ Step Functions
```

---

## Step 2

Click

```text
Create state machine
```

---

## Step 3

Choose:

- Workflow Studio
- Standard Workflow (recommended for learning)

Click **Design your workflow visually**.

---

## Step 4

Add the required states:

- CreateBucket
- Wait
- CopyObject
- Wait
- Choice
- Success
- Fail

Connect them in the desired order.

---

## Step 5

Configure each state.

### CreateBucket

Configure:

- Bucket Name

---

### Wait

Configure:

- Wait Time = 10 Seconds

---

### CopyObject

Provide:

- Destination Bucket
- Source Bucket
- Source Object Key

---

### Second Wait

Configure:

- Wait Time = 5 Seconds

---

### Choice

Configure the condition:

```text
{% $exists($states.input.CopyObjectResult.ETag) %}
```

If **True**

→ Success

Else

→ Fail

---

## Step 6

Review the generated Amazon States Language (ASL) definition.

---

## Step 7

Create or select an IAM execution role with permissions for:

- s3:CreateBucket
- s3:CopyObject
- s3:GetObject
- s3:PutObject
- states:StartExecution

---

## Step 8

Click **Create**.

---

## Step 9

Click **Start Execution**.

Monitor the workflow execution from the **Execution History** page.

---

# 7. State Machine Definition

The following state machine was used for this demonstration.

```json
{
  "StartAt": "CreateBucket",
  "States": {
    "CreateBucket": {
      "Type": "Task",
      "Resource": "arn:aws:states:::aws-sdk:s3:createBucket",
      "Next": "Wait for 10 sec"
    },
    "Wait for 10 sec": {
      "Type": "Wait",
      "Seconds": 10,
      "Next": "CopyObject"
    },
    "CopyObject": {
      "Type": "Task",
      "Resource": "arn:aws:states:::aws-sdk:s3:copyObject",
      "Next": "Wait for 5 sec"
    },
    "Wait for 5 sec": {
      "Type": "Wait",
      "Seconds": 5,
      "Next": "Choice"
    },
    "Choice": {
      "Type": "Choice",
      "Choices": [
        {
          "Condition": "{% $exists($states.input.CopyObjectResult.ETag) %}",
          "Next": "Success"
        }
      ],
      "Default": "Fail"
    },
    "Success": {
      "Type": "Succeed"
    },
    "Fail": {
      "Type": "Fail"
    }
  },
  "QueryLanguage": "JSONata"
}
```

---

# 8. Best Practices

- Design workflows with a single responsibility for each state.
- Use **Choice** states for conditional branching.
- Use **Wait** states only when necessary.
- Enable CloudWatch logging for easier debugging.
- Grant least-privilege IAM permissions to the execution role.
- Use Standard Workflows for long-running business processes.
- Use Express Workflows for high-volume, short-duration workloads.

---

# 9. Summary

AWS Step Functions simplifies the orchestration of distributed applications by coordinating multiple AWS services through visual workflows. Using state machines, developers can build reliable, scalable, and fault-tolerant applications with minimal code. In this demo, a workflow was created to provision an Amazon S3 bucket, copy an object into it, validate the copy operation using a Choice state with a JSONata expression, and complete the execution with either a Success or Fail state.
