# Amazon API Gateway

## Build, Secure, and Manage APIs at Scale

**Amazon API Gateway** is a fully managed service that enables developers to create, publish, maintain, monitor, and secure APIs at any scale. It acts as the front door for applications, allowing clients to access backend services such as AWS Lambda, Amazon EC2, Amazon ECS, AWS Step Functions, and HTTP endpoints through REST, HTTP, or WebSocket APIs.

API Gateway handles request routing, authentication, authorization, throttling, monitoring, caching, and API versioning without requiring you to manage infrastructure.

---

# Table of Contents

1. [What is Amazon API Gateway?](#1-what-is-amazon-api-gateway)
2. [Why Use Amazon API Gateway?](#2-why-use-amazon-api-gateway)
3. [API Gateway Architecture](#3-api-gateway-architecture)
4. [Core Components](#4-core-components)
   - [API](#api)
   - [Resources](#resources)
   - [Methods](#methods)
   - [Stages](#stages)
   - [Integrations](#integrations)
   - [Authorizers](#authorizers)
5. [API Types](#5-api-types)
6. [Workflow](#6-workflow)
7. [Common Use Cases](#7-common-use-cases)
8. [Advantages](#8-advantages)
9. [Limitations](#9-limitations)
10. [Creating an API using AWS Console](#10-creating-an-api-using-aws-console)
11. [Best Practices](#11-best-practices)
12. [API Gateway vs Application Load Balancer](#12-api-gateway-vs-application-load-balancer)
13. [Pricing](#13-pricing)
14. [Summary](#14-summary)

---

# 1. What is Amazon API Gateway?

Amazon API Gateway is a fully managed service that allows you to create and expose APIs for applications. It accepts client requests, authenticates users, routes requests to backend services, and returns responses securely.

Supported backend services include:

- AWS Lambda
- Amazon EC2
- Amazon ECS
- AWS Step Functions
- HTTP/HTTPS endpoints
- AWS services

---

# 2. Why Use Amazon API Gateway?

Without API Gateway:

- Build your own API server.
- Implement authentication manually.
- Handle rate limiting and throttling.
- Create custom monitoring and logging.
- Secure endpoints yourself.

With API Gateway:

- Fully managed service
- Automatic scaling
- Built-in authentication
- Request throttling
- Monitoring with CloudWatch
- Easy integration with AWS services

---

# 3. API Gateway Architecture

```text
          Client
             │
             ▼
     Amazon API Gateway
             │
    ┌────────┼────────┐
    ▼        ▼        ▼
 Lambda     EC2      ECS
             │
             ▼
        Database
```

---

# 4. Core Components

## API

The main entry point exposed to clients.

---

## Resources

Resources represent URL paths.

Example:

```text
/products
/users
/orders
```

---

## Methods

Each resource supports HTTP methods such as:

- GET
- POST
- PUT
- DELETE
- PATCH

---

## Stages

Stages represent deployment environments.

Examples:

- dev
- test
- staging
- production

---

## Integrations

API Gateway can integrate with:

- AWS Lambda
- HTTP endpoints
- Amazon ECS
- Amazon EC2
- AWS Step Functions
- Other AWS services

---

## Authorizers

Used to secure APIs.

Supported authorizers:

- IAM
- Amazon Cognito
- Lambda Authorizer
- JWT Authorizer (HTTP APIs)

---

# 5. API Types

## REST API

- Rich feature set
- API Keys
- Usage Plans
- Request Validation
- Caching

Best for enterprise applications.

---

## HTTP API

- Lower latency
- Lower cost
- JWT Authentication
- Simpler configuration

Best for serverless APIs.

---

## WebSocket API

Provides two-way communication between client and server.

Common use cases:

- Chat applications
- Live dashboards
- Gaming
- Notifications

---

# 6. Workflow

```text
Client
   │
   ▼
Amazon API Gateway
   │
Authentication
   │
   ▼
Route Request
   │
   ▼
Backend Service
(Lambda / EC2 / ECS)
   │
   ▼
Response
   │
   ▼
Client
```

---

# 7. Common Use Cases

- REST APIs
- Serverless applications
- Mobile backends
- Microservices
- Web applications
- IoT APIs
- Internal enterprise APIs

---

# 8. Advantages

- Fully managed
- Automatic scaling
- Secure authentication
- Multiple API types
- Built-in throttling
- CloudWatch monitoring
- Easy AWS integration
- Supports custom domains

---

# 9. Limitations

- Payload size limits
- Request timeout limits
- Some advanced features are only available in REST APIs
- Pricing depends on API requests

---

# 10. Creating an API using AWS Console

## Step 1

Open the AWS Management Console.

Navigate to:

```text
AWS Console
→ API Gateway
```

---

## Step 2

Click:

```text
Create API
```

---

## Step 3

Choose an API type:

- HTTP API
- REST API
- WebSocket API

Click **Build**.

---

## Step 4

Configure the API.

Provide:

- API Name
- Description
- Endpoint Type (Regional, Edge Optimized, or Private)

---

## Step 5

Create resources.

Example:

```text
/products
```

---

## Step 6

Add methods.

Example:

```text
GET
POST
PUT
DELETE
```

---

## Step 7

Configure the backend integration.

Choose one:

- AWS Lambda
- HTTP Endpoint
- EC2
- ECS
- Step Functions

---

## Step 8

(Optional) Configure authentication.

Supported options:

- IAM
- Amazon Cognito
- Lambda Authorizer
- JWT Authorizer

---

## Step 9

Deploy the API.

Create a stage such as:

```text
dev
```

or

```text
prod
```

---

## Step 10

Copy the Invoke URL.

Example:

```text
https://abc123.execute-api.us-east-1.amazonaws.com/prod
```

Use tools like **Postman**, **curl**, or a web browser to test the API.

---

# 11. Best Practices

- Use HTTP APIs when advanced REST features are not required.
- Protect APIs with IAM or Amazon Cognito.
- Enable CloudWatch logging.
- Use throttling to prevent abuse.
- Use stages for different environments.
- Enable caching for frequently accessed APIs.
- Follow the principle of least privilege for IAM roles.

---

# 12. API Gateway vs Application Load Balancer

| Feature | API Gateway | Application Load Balancer |
|----------|-------------|---------------------------|
| Purpose | API Management | Load Balancing |
| Authentication | Built-in | Limited |
| Rate Limiting | Yes | No |
| API Keys | Yes | No |
| Caching | Yes | No |
| WebSocket Support | Yes | Yes |
| Lambda Integration | Native | Supported |
| Best For | APIs & Serverless | Web Applications |

---

# 13. Pricing

Amazon API Gateway follows a **pay-as-you-go** pricing model based primarily on:

- Number of API requests
- Data transferred
- Cache usage (if enabled)

HTTP APIs are generally lower cost than REST APIs.

---

# 14. Summary

Amazon API Gateway is a fully managed service for building, securing, and monitoring APIs. It supports REST, HTTP, and WebSocket APIs and integrates seamlessly with AWS services such as Lambda, ECS, EC2, and Step Functions. It is an essential service for building serverless applications, microservices, and modern cloud-native architectures.
