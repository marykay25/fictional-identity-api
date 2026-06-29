# [Service Name] - [Incident Scenario] Incident Playbook

## Purpose

Use this playbook when [briefly describe the incident scenario].

Examples:

- The service is unavailable.
- Error rate is above expected levels.
- Customers are reporting failures.
- An alert has fired for this service.
- Data is delayed, missing, or failing to process.

---

## Symptoms

List what CloudOps or Engineering may observe.

- [Alert name or symptom]
- [Customer-facing symptom]
- [Dashboard symptom]
- [Log symptom]
- [Performance symptom]

---

## Step-by-Step Investigation

### Step 1 — Confirm the scope

What to do:

1. Open [dashboard/workbook/log link].
2. Check whether the issue affects one customer, multiple customers, one region, or the full service.
3. Record the scope in the ticket or incident record.

Expected result:

- Scope is identified as customer-specific, regional, environment-specific, or service-wide.

If this fails:

- Escalate to [team/channel] with the ticket link and what could not be confirmed.

---

### Step 2 — Check service health

What to do:

1. Open [service health dashboard].
2. Check [metric 1], [metric 2], and [metric 3].
3. Compare current behavior against normal behavior.

Expected result:

- You can determine whether the service itself is unhealthy.

If this fails:

- Capture screenshots or log snippets.
- Continue to dependency checks.

---

### Step 3 — Check dependencies

What to do:

1. Check [dependency 1].
2. Check [dependency 2].
3. Check [dependency 3].

Expected result:

- Dependencies are confirmed healthy or the failing dependency is identified.

If this fails:

- Escalate to the dependency owner listed in the Service Overview.

---

### Step 4 — Check recent changes

What to do:

1. Review recent deployments, configuration changes, or infrastructure changes.
2. Check whether the issue started after a known change.
3. Link any related change, deployment, or pull request in the ticket.

Expected result:

- Recent changes are either ruled out or identified as a possible cause.

If this fails:

- Continue investigation and note that recent changes could not be confirmed.

---

### Step 5 — Apply approved mitigation

Only perform approved actions listed below.

Approved actions:

1. [Approved action 1]
2. [Approved action 2]
3. [Approved action 3]

Do not perform:

- [Unsafe or unapproved action]
- [Action requiring Engineering approval]
- [Action requiring change approval]

Expected result:

- Service begins recovering, alert clears, or customer impact is reduced.

If this fails:

- Escalate using the escalation section below.

---

## Validation

Confirm recovery by checking:

- [ ] Alert has cleared or severity has reduced.
- [ ] Error rate is back to expected levels.
- [ ] Customer-facing functionality is working.
- [ ] Logs no longer show the failure pattern.
- [ ] Dashboard/workbook shows recovery.
- [ ] Ticket or incident record has been updated.

Validation evidence:

```text
Paste dashboard, log, or ticket evidence here.
```

---

## Escalation

Escalate when:

- The issue is customer-impacting and cannot be mitigated quickly.
- The cause is unknown after completing the investigation steps.
- Required access is missing.
- The fix requires Engineering, DevOps, Security, or another team.
- The issue affects FedRAMP, compliance, customer data, or security-sensitive systems.

Escalation path:

| Situation | Escalate to | Method |
|---|---|---|
| Service outage | [Team / channel] | [Method] |
| Dependency failure | [Team / channel] | [Method] |
| Access issue | [Team / channel] | [Method] |
| Security concern | [Team / channel] | [Method] |
| FedRAMP / compliance concern | [Team / channel] | [Method] |

Include this information when escalating:

- Environment
- Customer / tenant, if applicable
- Ticket or incident link
- Alert name
- Time issue started
- Scope
- Steps completed
- Validation evidence
- Any suspected cause

---

## Related Links

- Service Overview: [Link]
- Dashboard / Workbook: [Link]
- Logs: [Link]
- Alerts: [Link]
- Related operations playbook: [Link]
- Related automation runbook: [Link]
- Repository: [Link]

---
