# Fictional Identity API - Login Errors Incident Playbook

## Purpose

Use this playbook when authentication failures, token issuance errors, or unexpected sign-out events are affecting the Fictional Identity API. This includes cases where login success rate drops below expected thresholds, refresh-token flows fail, or customer-facing authentication experiences degrade.

Examples:

- The service is returning elevated 5xx responses on authentication endpoints.
- Customers are reporting repeated sign-in loops or forced reauthentication.
- An alert has fired for authentication error rate or token issuance latency.
- A recent deployment may have introduced a regression in token handling.

---

## Symptoms

- Alert name or symptom: `identity-api-auth-error-rate-critical`
- Customer-facing symptom: Users cannot sign in or are repeatedly prompted to authenticate
- Dashboard symptom: Authentication error rate and token issuance latency spike above baseline
- Log symptom: `TOKEN_GENERATION_FAILED`, `SESSION_STORE_TIMEOUT`, or `INVALID_JWT_CLAIM`
- Performance symptom: p95 login latency rises above 1.5 seconds

---

## Step-by-Step Investigation

### Step 1 — Confirm the scope

What to do:

1. Open the Identity API monitoring dashboard.
2. Check whether the issue affects one customer, multiple customers, one region, or the full service.
3. Record the scope in the incident record.

Expected result:

- Scope is identified as regional, customer-specific, or service-wide.

If this fails:

- Escalate to the Identity on-call team with the ticket link and what could not be confirmed.

---

### Step 2 — Check service health

What to do:

1. Open the authentication and token service health dashboard.
2. Check authentication error rate, login latency, token issuance success, and session validation errors.
3. Compare current behavior against normal behavior.

Expected result:

- You can determine whether the identity service itself is unhealthy.

If this fails:

- Capture screenshots or log snippets and continue to dependency checks.

---

### Step 3 — Check dependencies

What to do:

1. Check PostgreSQL identity store health and connection pool utilization.
2. Check Redis session cache health and error rates.
3. Check Kafka and the secret management service for delay or outage symptoms.

Expected result:

- Dependencies are confirmed healthy or the failing dependency is identified.

If this fails:

- Escalate to the dependency owner listed in the Service Overview.

---

### Step 4 — Check recent changes

What to do:

1. Review recent deployments, feature flag changes, and authentication configuration updates.
2. Check whether the issue started after a canary rollout or token policy change.
3. Link any related deployment or pull request in the incident ticket.

Expected result:

- Recent changes are either ruled out or identified as a possible cause.

If this fails:

- Continue the investigation and note that recent changes could not be confirmed.

---

### Step 5 — Apply approved mitigation

Only perform approved actions listed below.

Approved actions:

1. Pause the canary rollout if the issue started after a deployment.
2. Roll back the authentication service to the last known-good version if the regression is confirmed.
3. Scale up authentication worker capacity if the issue is caused by saturation.
4. Refresh affected session cache entries if session corruption is suspected.

Do not perform:

- Reset passwords in bulk.
- Disable MFA enforcement globally without security approval.
- Clear the entire Redis cluster without explicit approval.
- Continue a canary rollout while the issue is under investigation.

Expected result:

- Service begins recovering, the alert clears, or customer impact is reduced.

If this fails:

- Escalate using the escalation section below.

---

## Validation

Confirm recovery by checking:

- [ ] Alert has cleared or severity has reduced.
- [ ] Authentication error rate is back to expected levels.
- [ ] Customer-facing sign-in functionality is working.
- [ ] Logs no longer show the failure pattern.
- [ ] Monitoring dashboard shows recovery.
- [ ] Incident record has been updated.

Validation evidence:

```text
Authentication success rate recovered above 99.5% after rollback and cache refresh.
```

---

## Escalation

Escalate when:

- The issue is customer-impacting and cannot be mitigated quickly.
- The cause is unknown after completing the investigation steps.
- Required access is missing.
- The fix requires Engineering, SRE, Security, or another team.
- The issue affects customer data, compliance, or security-sensitive systems.

Escalation path:

| Situation | Escalate to | Method |
|---|---|---|
| Service outage | Identity on-call and SRE | PagerDuty and incident channel |
| Dependency failure | Dependency owner / Infrastructure | Slack or incident process |
| Access issue | Identity Platform Team | Slack or Jira |
| Security concern | Security team | Security incident process |

Include this information when escalating:

- Environment
- Customer / tenant, if applicable
- Incident link
- Alert name
- Time issue started
- Scope
- Steps completed
- Validation evidence
- Any suspected cause

---

## Related Links

- Service Overview: [Service overview](../../service-overview/service-overview.md)
- Dashboard / Workbook: [Identity API Metrics](https://metrics.fictional.internal/identity-api)
- Logs: [Identity API Logs](https://logs.fictional.internal/identity-api)
- Alerts: [Identity API Alerts](https://alerts.fictional.internal/identity-api)
- Related operations playbook: [Operations playbook](../operations/operations-playbook.md)
- Related automation runbook: [Internal automation runbook](https://runbooks.fictional.internal/identity-api)
- Repository: [Fictional Identity API repository](https://github.com/fictional/identity-api)
