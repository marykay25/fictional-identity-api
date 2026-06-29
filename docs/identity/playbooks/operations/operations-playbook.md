# Fictional Identity API - Routine Operations Playbook

## Purpose

Use this playbook to perform standard operational tasks for the Fictional Identity API, including routine health checks, change validation, and low-severity recovery actions. This playbook is intended for day-to-day platform operations and should not be used for major incidents.

Do not use this playbook when:

- A customer-facing outage is ongoing and the service is under incident response
- A change requires broad production impact or a formal rollback decision
- A security or compliance concern needs immediate escalation

---

## Preconditions

Before starting, confirm:

- [ ] The task is approved, if approval is required.
- [ ] The request has a valid ticket or tracking record.
- [ ] You are working in the correct environment.
- [ ] You have the required access.
- [ ] You understand the expected outcome.
- [ ] Any customer, compliance, or change restrictions have been checked.

---

## Inputs Needed

Collect the following before starting:

| Input | Example | Required |
|---|---|---|
| Ticket / request link | Jira or change record | Yes |
| Environment | Production / Staging | Yes |
| Region | us-east-1 / eu-west-1 | If applicable |
| Service name | Fictional Identity API | Yes |
| Requested completion date | Next business day | If applicable |
| Approval link | Change approval record | If required |

---

## Access Required

Required access:

- Access to the monitoring dashboard for authentication and token metrics
- Access to deployment tooling for canary or rollback operations
- Access to service logs for validation and troubleshooting

If access is missing:

1. Stop the procedure.
2. Update the ticket with the missing access.
3. Escalate to the Identity Platform Team.
4. Do not ask another person to perform the step manually unless that is the approved process.

---

## Procedure

### Step 1 — Review service health

What to do:

1. Review authentication success rate, error rate, and login latency dashboards.
2. Check token issuance success and session validation errors.
3. Confirm there is no unusual spike in failed login attempts or account lockouts.

Expected result:

- The current health state is understood and no immediate incident is active.

If this fails:

- Treat the condition as a potential incident and follow the incident playbook.

---

### Step 2 — Validate recent changes

What to do:

1. Review recent deployments, feature flag updates, and configuration changes.
2. Confirm the change was deployed to the correct environment and region.
3. Compare current behavior against the expected outcome.

Expected result:

- Recent changes are either validated or linked to service impact.

If this fails:

- Pause the change, capture evidence, and escalate if the issue is customer-impacting.

---

### Step 3 — Perform routine maintenance or low-severity recovery

What to do:

1. Deploy canary releases with staged traffic increase when required.
2. Apply configuration changes using a documented change ticket.
3. Run smoke tests for login, token validation, and password reset flows.
4. If needed, restart a single unhealthy pod or re-enable a dependency circuit breaker after confirming the change is safe.

Expected result:

- The service remains healthy and the operational task is complete.

If this fails:

- Stop the procedure, capture logs and metrics, and escalate.

---

## Validation

Confirm the task completed successfully.

Validation checklist:

- [ ] Requested change is complete.
- [ ] Correct environment was updated.
- [ ] Authentication metrics remain within normal thresholds.
- [ ] Dashboard, logs, or command output confirm success.
- [ ] Ticket has been updated with evidence.
- [ ] Requester has been notified, if required.

Validation evidence:

```text
Example: authentication success rate remained above 99.5% and login latency stayed within baseline after the routine change.
```

---

## Rollback / Backout

Use this section if the procedure changes production, configuration, infrastructure, routing, deployment state, or service behavior.

Rollback steps:

1. Revert the most recent change if validation fails or customer impact is observed.
2. Restore the previous configuration or deployment version.
3. Re-run smoke tests and confirm the service returns to baseline.

If rollback is not possible:

- Explain why rollback is not possible.
- Escalate to the Identity on-call team.
- Update the ticket with the risk and current state.

---

## Communication

Update the ticket or request with:

- What was completed
- When it was completed
- Who completed it
- Validation evidence
- Any follow-up needed

If requester notification is required, use this format:

```text
The requested operation has been completed.

Summary:
- Service: Fictional Identity API
- Environment: Production / Staging
- Change completed: [summary]
- Validation: [summary]
- Follow-up needed: [summary]
```

---

## Escalation

Escalate when:

- Required access is missing.
- The request is unclear.
- The procedure fails.
- The requested action does not match the approved request.
- The task may impact production, FedRAMP, compliance, customer data, or security.
- The rollback path is unclear.

Escalation path:

| Situation | Escalate to | Method |
|---|---|---|
| Missing access | Identity Platform Team | Slack or Jira |
| Request unclear | Identity Platform Team | Slack or Jira |
| Procedure failure | Identity on-call and SRE | PagerDuty and incident channel |
| Production risk | Identity on-call and SRE | PagerDuty and incident channel |
| Security/compliance concern | Security team | Security incident process |

---

## Related Links

- Service Overview: [Service overview](../../service-overview/service-overview.md)
- Related incident playbook: [Login errors incident playbook](../incident/login-errors.md)
- Dashboard / Workbook: [Identity API Metrics](https://metrics.fictional.internal/identity-api)
- Logs: [Identity API Logs](https://logs.fictional.internal/identity-api)
- Related automation runbook: [Internal automation runbook](https://runbooks.fictional.internal/identity-api)
- Repository: [Fictional Identity API repository](https://github.com/fictional/identity-api)
