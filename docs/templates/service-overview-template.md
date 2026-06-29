# [Service Name] - Service Overview

## Purpose

Describe what this service does in plain language.

This service is responsible for:

- [Responsibility 1]
- [Responsibility 2]
- [Responsibility 3]

This service is not responsible for:

- [Out-of-scope item]
- [Out-of-scope item]

---

## Business / Operational Importance

Explain why this service matters.

Examples:

- What customer or internal function depends on it?
- What happens if it is unavailable?
- Is it part of a critical workflow, deployment path, customer onboarding flow, or compliance process?

---

## Service Ownership

| Area | Owner |
|---|---|
| Engineering owner | [Team / person] |
| Escalation owner | [Team / person / channel] |
| Product or business owner | [If applicable] |

---

## Architecture Summary

Provide a simple explanation of how the service works.

Include:

- Main components
- Major dependencies
- Data flow
- Internal integrations
- External integrations
- Environments where the service runs
- VLAN IDs
- Service Bus Subscriptions


Architecture diagram:

- [Link to diagram]

---

## Environments

| Environment | Purpose | Region / Location | Notes |
|---|---|---|---|
| Development | [Purpose] | [Region] | [Notes] |
| Staging | [Purpose] | [Region] | [Notes] |
| Production | [Purpose] | [Region] | [Notes] |
| FedRAMP / Gov | [Purpose] | [Region] | [Notes] |

---

## Dependencies

| Dependency | Type | Purpose | Impact if unavailable |
|---|---|---|---|
| [Dependency name] | Internal / External | [Purpose] | [Impact] |
| [Dependency name] | Internal / External | [Purpose] | [Impact] |

---

## Authentication and Access

Describe how access works.

Include:

- Required roles or permissions
- Service accounts or managed identities
- Admin portals or consoles
- Any environment-specific access notes
- Any known CloudOps access limitations

Required access for CloudOps and Engineering:

- [Access requirement 1]
- [Access requirement 2]
- [Access requirement 3]

---

## Dashboards, Logs, Alerts, and Monitoring

| Resource | Link | Purpose |
|---|---|---|
| Dashboard / Workbook | [Link] | Service health and investigation |
| Logs | [Link] | Troubleshooting and diagnostics |
| Alerts | [Link] | Detection and notification |
| Metrics | [Link] | Performance and reliability tracking |

---

## Health Indicators

List the main signals that indicate whether the service is healthy.

| Signal | Healthy state | Where to check |
|---|---|---|
| Availability | [Expected state] | [Dashboard/log link] |
| Error rate | [Expected state] | [Dashboard/log link] |
| Latency | [Expected state] | [Dashboard/log link] |
| Throughput | [Expected state] | [Dashboard/log link] |
| Queue depth / backlog | [If applicable] | [Dashboard/log link] |

---

## Common Failure Modes

| Failure mode | Symptom | Related incident playbook |
|---|---|---|
| [Failure mode] | [What CloudOps/Engineering would see] | [Link] |
| [Failure mode] | [What CloudOps/Engineering would see] | [Link] |

---

## Escalation Path

Use this section to explain who to contact and when.

| Situation | Escalate to | Method |
|---|---|---|
| Service outage | [Team / channel] | [Jira / Teams / email / incident process] |
| Access issue | [Team / channel] | [Jira / Teams / email] |
| Customer-impacting issue | [Team / channel] | [Incident process] |
| Security concern | [Team / channel] | [Security process] |

---

## Known Limitations / Open Gaps

List anything CloudOps or Engineering should know before operating this service.

- [Known limitation]
- [Known issue]
- [Missing dashboard, alert, access, or automation]
- [Manual process that should eventually be automated]

---

## Source and Repository Information

Repository:

- [Repository link]

Documentation location:

```text
/docs/service-overview.md
/docs/playbooks/
```

Primary code locations:

```text
/src
/deployments
/infrastructure
```
