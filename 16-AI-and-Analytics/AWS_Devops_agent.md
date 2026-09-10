# AWS DevOps Agent
AWS DevOps Agent is an always-available AI operations teammate for **release management** and **production operations**.

AWS DevOps Agent is a fully managed frontier agent that investigates incidents, reviews release readiness, runs change-specific tests, and recommends operational improvements without requiring you to staff a dedicated on-call investigation team around the clock. It is commonly used as the **SRE / incident investigation layer** of an AWS DevOps toolchain.
---
## Table of Contents
1. [What is AWS DevOps Agent?](#1-what-is-aws-devops-agent)
2. [Why Use AWS DevOps Agent?](#2-why-use-aws-devops-agent)
3. [AWS DevOps Agent Architecture](#3-aws-devops-agent-architecture)
4. [Core Components](#4-core-components)
5. [DevOps Agent Workflow](#5-devops-agent-workflow)
6. [Agent Skills](#6-agent-skills)
7. [Supported Integrations](#7-supported-integrations)
8. [Creating an Agent Space Using AWS Console](#8-creating-an-agent-space-using-aws-console)
9. [Running an Investigation](#9-running-an-investigation)
10. [Common Use Cases](#10-common-use-cases)
11. [Advantages](#11-advantages)
12. [Best Practices](#12-best-practices)
---
# 1. What is AWS DevOps Agent?
AWS DevOps Agent is an AI-powered, always-available agent used to review software changes for production risk, investigate operational issues, identify root cause, and recommend preventative improvements. It removes the need to manually correlate telemetry, code, deployments, and runbooks during incidents.
It works across AWS, multicloud, and on-premises environments by learning your resources and their relationships, then correlating observability data, source repositories, and CI/CD pipelines.
You manage configuration in the **AWS Management Console**. Operators use the **DevOps Agent web app** for day-to-day investigations, chat, topology browsing, and prevention recommendations.
---
# 2. Why Use AWS DevOps Agent?
Without AWS DevOps Agent, organizations may need to:
- Manually correlate logs, metrics, traces, tickets, and deployments
- Switch between multiple observability consoles during incidents
- Maintain tribal knowledge of service dependencies
- Staff 24/7 investigation coverage
- Discover recurring incidents only after they happen again
With AWS DevOps Agent, you get:
- Autonomous incident investigation when an alert or ticket arrives
- Application topology discovery
- Root-cause hypotheses with mitigation steps
- Release readiness review and autonomous release testing (preview)
- Natural-language SRE tasks in the operator web app
- Pay-for-agent-time usage, billed per second
---
# 3. AWS DevOps Agent Architecture
AWS DevOps Agent uses a **dual-console** model:
```text
Administrators                         Operators
AWS Management Console                 DevOps Agent web app
        |                                      |
        v                                      v
 Create Agent Space                    Chat / investigations
 Configure IAM roles                   Topology browser
 Associate AWS accounts                Prevention recommendations
 Register integrations                 Custom charts / reports
 Manage access                         Custom agents / schedules
        |                                      |
        +------------------+-------------------+
                           |
                           v
                    Agent Space
                           |
           +---------------+---------------+
           |               |               |
           v               v               v
     AWS accounts     Observability      Code / CI/CD
     CloudWatch       Datadog            GitHub / GitLab
     Topology         Dynatrace          Azure DevOps
                      Splunk / Grafana
                      New Relic
```
Typical investigation path:
```text
Alert / ticket / chat request
   |
   v
AWS DevOps Agent
   |
   +--> Topology and resource map
   +--> Telemetry (logs, metrics, traces)
   +--> Recent deployments and code changes
   +--> Skills / runbooks
   |
   v
Hypotheses, observations, root cause, mitigation
   |
   v
Slack / ServiceNow / PagerDuty / AWS Support
```
---
# 4. Core Components
### Agent Space
An Agent Space is a logical container that defines what AWS DevOps Agent can access. It includes AWS account associations, third-party integrations, IAM roles, user access, and investigation history.
Use separate Agent Spaces for different teams, production vs non-production, or compliance boundaries. Data, chat history, and recommendations are isolated per Agent Space.
### Topology
AWS DevOps Agent automatically discovers applications, services, and resources, then maps relationships using resource discovery, CloudFormation and tags, CI/CD mapping, and observability behavior.
Operators can browse the topology graph in the web app or ask Chat questions such as which Lambda functions connect to a DynamoDB table.
### Operator Web App
A dedicated web application outside the AWS Management Console. Operators launch investigations, chat in natural language, view topology, review recommendations, and create charts or scheduled custom agents.
Authentication can use IAM Identity Center, an external OIDC identity provider, or a short-lived IAM admin access link from the console.
### Skills
Reusable modules that encode runbooks, architectural standards, and operational practices so the agent executes specialized tasks consistently.
### Integrations
Built-in connections to observability tools, source control, CI/CD, and incident communication channels. You can also connect private or remote MCP servers, and invoke the agent through MCP, ACP, or A2A.
### Service Roles
IAM roles that the Agent Space assumes to query CloudWatch, describe resources, build topology, and access associated AWS accounts. Secondary source accounts use additional roles.
### Recommendations and Artifacts
Investigation findings, mitigation plans, prevention recommendations, topology memories, and summary reports produced by the agent.
---
# 5. DevOps Agent Workflow
AWS DevOps Agent receives an alert, ticket, or operator prompt, loads the Agent Space topology and integrations, correlates telemetry with code and deployments, applies skills, and returns observations, root-cause hypotheses, and mitigation steps.
```text
Trigger
   |
   v
Start investigation
   |
   v
Gather context
 (topology, telemetry, code, CI/CD, skills)
   |
   v
Analyze
 (hypotheses, blast radius, recent changes)
   |
   v
Report
 (root cause, mitigation, prevention)
   |
   v
Notify / hand off
 (Slack, ticketing, coding agent, AWS Support)
```
Release management (preview) follows a similar loop before production:
```text
Code change / pull request
   |
   v
Release readiness review
   |
   v
Change-specific tests
   |
   v
Findings in PR / IDE / pipeline
```
