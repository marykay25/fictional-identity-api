# Fictional Identity API - Service Overview

## Purpose

The Fictional Identity API is the central authentication and authorization gateway for fictional customer-facing applications. It handles sign-in, session management, token issuance, password reset, and identity lookups for internal and partner systems.

This service is responsible for:

- Authenticating users for web, mobile, and partner applications
- Issuing and validating access tokens and refresh tokens
- Managing the session lifecycle for active sign-ins
- Enforcing MFA and account security policies

This service is not responsible for:

- Customer support for individual account issues
- Password storage outside the approved identity platform
- User-facing billing or subscription management

---

## Business / Operational Importance

The Fictional Identity API is part of the login and access path for nearly every customer-facing experience. If it is unavailable or degraded, users may be unable to sign in, refresh sessions, or use protected features. It is a critical dependency for onboarding, support workflows, and partner integrations.

---

## Service Ownership

| Area | Owner |
|---|---|
| Engineering owner | Identity Platform Team |
| Escalation owner | Identity on-call and SRE |
| Product or business owner | Internal Platform Product |

---

## Architecture Summary

The service is composed of a gateway layer, an authentication service, a token service, a session store, and an identity data store. Requests flow through the gateway, are validated by the authentication service, and then use the token service and session store to complete sign-in and refresh operations. The service runs in development, staging, and production environments and publishes audit events to downstream systems.

Architecture diagram:

- [Internal auth flow diagram](../diagrams/authentication-flow.md)

---

## Environments

| Environment | Purpose | Region / Location | Notes |
|---|---|---|---|
| Development | Local feature validation and integration testing | us-east-1 | Shared sandbox environment |
| Staging | Pre-release verification | us-east-1 | Mirrors production configuration |
| Production | Customer-facing authentication | us-east-1, eu-west-1 | Regional failover support |
| FedRAMP / Gov | Regulated deployment for internal workloads | us-gov-west-1 | Limited access and enhanced audit controls |

---

## Dependencies

| Dependency | Type | Purpose | Impact if unavailable |
|---|---|---|---|
| PostgreSQL identity store | Internal | Stores profiles, credentials, and policy state | Login and profile lookups fail |
| Redis session cache | Internal | Stores session state and token revocation data | Session validation and refresh fail |
| Kafka event bus | Internal | Publishes audit and notification events | Auditing and downstream notifications are delayed |
| Secret management service | Internal | Provides signing keys and credentials | Token issuance and validation become unreliable |
| Email and SMS providers | External | Sends password reset and MFA notifications | Recovery workflows degrade |

---

## Authentication and Access

Access to the service is managed through role-based permissions for engineering, SRE, and cloud operations. Engineering teams require access to deploy, inspect logs, and review metrics in the production environment. Service accounts are used for internal integrations and automated job execution.

Required access for CloudOps and Engineering:

- Access to production deployment pipelines
- Read access to application logs and monitoring dashboards
- Access to the secret management service for incident support

---

## Dashboards, Logs, Alerts, and Monitoring

| Resource | Link | Purpose |
|---|---|---|
| Dashboard / Workbook | [Identity API Metrics](https://console.aws.fictional.internal/cloudwatch/home?region=us-east-1#dashboards:name=Identity-API-Metrics) | Service health and investigation |
| Logs | [Identity API Logs](https://logs.fictional.internal/identity-api) | Troubleshooting and diagnostics |
| Alerts | [Identity API Alerts](https://alerts.fictional.internal/identity-api) | Detection and notification |
| Metrics | [Identity API Metrics](https://metrics.fictional.internal/identity-api) | Performance and reliability tracking |

---

## Health Indicators

| Signal | Healthy state | Where to check |
|---|---|---|
| Availability | 99.9% monthly or better | Identity API Metrics |
| Error rate | Less than 1% on authentication endpoints | Identity API Metrics |
| Latency | p95 login latency under 800ms | Identity API Metrics |
| Throughput | Normal request volume for active customer traffic | Identity API Metrics |
| Queue depth / backlog | No sustained queue buildup in downstream workers | Kafka and worker dashboards |

---

## Common Failure Modes

| Failure mode | Symptom | Related incident playbook |
|---|---|---|
| Token issuance regression | Login failures and refresh-token errors | [Login errors incident playbook](../playbooks/incident/login-errors.md) |
| Dependency outage | 5xx responses and increased latency | [Incident playbook](../playbooks/incident/login-errors.md) |

---

## Escalation Path

| Situation | Escalate to | Method |
|---|---|---|
| Service outage | Identity on-call and SRE | PagerDuty and incident channel |
| Access issue | Identity Platform Team | Slack or Jira |
| Customer-impacting issue | Identity on-call | Incident process |
| Security concern | Security team | Security incident process |

---

## Known Limitations / Open Gaps

- The service does not yet have full automated rollback coverage for all auth changes
- Some regional dashboards still require manual validation during incidents
- Access to secret rotation tooling is restricted to a small set of engineers

---

## Source and Repository Information

Repository:

- [Fictional Identity API repository](https://github.com/fictional/identity-api)

Documentation location:

```text
/docs/identity/service-overview/service-overview.md
/docs/identity/playbooks/
```

Primary code locations:

```text
/src/auth
/src/token
/src/session
/deployments
/infrastructure
```
