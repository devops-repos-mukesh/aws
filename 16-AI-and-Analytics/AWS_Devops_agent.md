# AWS DevOps Agent

## Autonomous AI Operations Teammate

AWS DevOps Agent is a fully managed frontier agent that investigates incidents, reviews release readiness, runs change-specific tests, and recommends operational improvements. It is commonly used as the **SRE / incident investigation layer** of an AWS DevOps toolchain.

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

AWS DevOps Agent is an AI-powered, always-available agent used to review software changes, investigate operational issues, identify root cause, and recommend preventative improvements. It correlates telemetry, code, deployments, and runbooks across AWS, multicloud, and on-premises environments.

Administrators configure it in the **AWS Management Console**. Operators use the **DevOps Agent web app** for investigations, chat, topology, and recommendations.

---

# 2. Why Use AWS DevOps Agent?

Without it, teams often correlate logs, metrics, traces, tickets, and deployments by hand, switch consoles during incidents, and staff 24/7 investigation coverage.

With it, you get autonomous investigation when an alert or ticket arrives, topology discovery, root-cause hypotheses with mitigation steps, release readiness review (preview), natural-language SRE tasks, and pay-for-agent-time billing (per second).

---

# 3. AWS DevOps Agent Architecture

```text
AWS Management Console          DevOps Agent web app
 (admins: Agent Space, IAM,      (operators: chat, topology,
  accounts, integrations)         investigations, prevention)
                 \                     /
                  \                   /
                   v                 v
                      Agent Space
                           |
           +---------------+---------------+
           v               v               v
     AWS accounts     Observability      Code / CI/CD
     CloudWatch       Datadog, Splunk    GitHub / GitLab
                      Dynatrace, Grafana Azure DevOps
```

---

# 4. Core Components

### Agent Space

Logical container for AWS accounts, integrations, IAM roles, user access, and investigation history. Isolate teams, environments, and compliance boundaries with separate spaces.

### Topology

Automatic map of applications, services, and resource relationships, used during investigations and prevention recommendations.

### Operator Web App

Standalone app for chat, investigations, topology, and scheduled custom agents. Auth can use IAM Identity Center, an OIDC IdP, or a short IAM admin access link.

### Skills, Integrations, and Service Roles

Skills encode runbooks. Integrations connect observability, source, CI/CD, and incident tools (including MCP). IAM roles let the agent query CloudWatch, describe resources, and access associated accounts.

---

# 5. DevOps Agent Workflow

The agent receives an alert, ticket, or chat prompt, loads topology and integrations, correlates telemetry with code and deployments, applies skills, and returns observations, root cause, and mitigation.

```text
Trigger → Investigate → Gather context → Analyze → Report → Notify / hand off
```

Release management (preview): code change → readiness review → change-specific tests → findings in PR / IDE / pipeline.

---

# 6. Agent Skills

Skills teach the agent your runbooks and standards. Create them in the console or CLI, optionally with a schedule (for example, a daily health report).

```bash
aws devops-agent associate-service \
  --agent-space-id <AGENT_SPACE_ID> \
  --service-id <SERVICE_ID> \
  --configuration '{"github":{"repoName":"<REPO>","owner":"<OWNER>"}}' \
  --region <REGION>
```

---

# 7. Supported Integrations

- **Observability:** CloudWatch, Datadog, Dynatrace, New Relic, Splunk, Grafana
- **Source / CI/CD:** GitHub, GitLab, Azure DevOps
- **Incident / collab:** ServiceNow, PagerDuty, Slack, Microsoft Teams, AWS Support
- **Extensibility:** private or remote MCP servers; MCP, ACP, and A2A

CloudWatch uses IAM roles. OAuth tools such as GitHub are registered once at the account level, then associated per Agent Space.

---

# 8. Creating an Agent Space Using AWS Console

**Step 1:** AWS Console → AWS DevOps Agent → Agent Spaces → **Create Agent Space**.

**Step 2:** Name the space (for example `ecommerce-prod-agent-space`).

**Step 3:** Choose or create an IAM service role for CloudWatch, topology, and source accounts.

**Step 4:** Associate a primary monitoring account and optional secondary application accounts.

**Step 5:** Enable the operator web app (IAM Identity Center, OIDC IdP, or IAM admin link).

**Step 6:** Connect observability, source/CI/CD, and optionally Slack / ServiceNow / PagerDuty.

**Step 7:** Review and click **Create Agent Space**. Open the web app and confirm Chat, Topology, and Investigations.

---

# 9. Running an Investigation

```text
AWS DevOps Agent → Agent Spaces → Select space → Open web app → Start investigation / Chat
```

Start from an alert webhook, support ticket, Chat prompt, or scheduled custom agent. Monitor status, evidence, duration, and follow-up. You can open an AWS Support case from an investigation with that context attached.

---

# 10. Common Use Cases

Incident investigation, root-cause analysis, MTTR reduction, recurring-incident prevention, on-demand SRE questions, release readiness and change-specific testing (preview), AI-generated PR review, and daily health reports.

```text
Alert → AWS DevOps Agent (investigate / correlate / mitigate / prevent)
      → Slack / ServiceNow / PagerDuty
      → Fix in GitHub / GitLab
      → CodePipeline / ECS / EKS
```

---

# 11. Advantages

Fully managed, no investigation servers to maintain, starts as soon as an alert arrives, learns topology over time, works across AWS / multicloud / on-prem, integrates with existing tools, pay per second of agent time.

---

# 12. Best Practices

Use a dedicated monitoring account. Separate prod and non-prod Agent Spaces. Use least-privilege IAM. Connect CloudWatch plus an APM/log platform. Enable CloudTrail. Encode runbooks as skills. Route findings to on-call tools. Do not put secrets in chat. Manage Agent Spaces and roles with IaC.

---
