# [Service Name] - [Procedure Name] Operations Playbook

## Purpose

Use this playbook to [briefly describe the operational task].

Examples:

- Complete a cloud deployment request.
- Rotate a certificate.
- Grant or revoke approved access.
- Perform a maintenance task.
- Run a standard operational procedure.
- Complete a customer or environment setup task.

Do not use this playbook when:

- [Out-of-scope condition]
- [Condition that requires incident response]
- [Condition that requires escalation or approval first]

---

## Preconditions

Before starting, confirm:

- [ ] The request is approved, if approval is required.
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
| Ticket / request link | [Jira / Salesforce / email / form] | Yes |
| Environment | Production / FedRAMP / Staging | Yes |
| Customer / tenant ID | [Example] | If applicable |
| Region | [Example] | If applicable |
| Service name | [Example] | Yes |
| Requested completion date | [Example] | If applicable |
| Approval link | [Example] | If required |

---

## Access Required

Required access:

- [Access requirement 1]
- [Access requirement 2]
- [Access requirement 3]

If access is missing:

1. Stop the procedure.
2. Update the ticket with the missing access.
3. Escalate to [team/channel].
4. Do not ask another person to perform the step manually unless that is the approved process.

---

## Procedure

### Step 1 — [Action name]

What to do:

1. [Simple instruction]
2. [Simple instruction]
3. [Simple instruction]

Expected result:

- [Expected result]

If this fails:

- [Troubleshooting step or escalation path]

---

### Step 2 — [Action name]

What to do:

1. [Simple instruction]
2. [Simple instruction]
3. [Simple instruction]

Expected result:

- [Expected result]

If this fails:

- [Troubleshooting step or escalation path]

---

### Step 3 — [Action name]

What to do:

1. [Simple instruction]
2. [Simple instruction]
3. [Simple instruction]

Expected result:

- [Expected result]

If this fails:

- [Troubleshooting step or escalation path]

---

## Validation

Confirm the task completed successfully.

Validation checklist:

- [ ] Requested change is complete.
- [ ] Correct environment was updated.
- [ ] Customer / tenant / service was updated correctly.
- [ ] Dashboard, portal, log, or command output confirms success.
- [ ] Ticket has been updated with evidence.
- [ ] Requester has been notified, if required.

Validation evidence:

```text
Paste evidence here.
```

---

## Rollback / Backout

Use this section if the procedure changes production, customer access, configuration, infrastructure, certificates, routing, deployment state, or service behavior.

Rollback steps:

1. [Rollback step 1]
2. [Rollback step 2]
3. [Rollback step 3]

If rollback is not possible:

- Explain why rollback is not possible.
- Escalate to [team/channel].
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
- Service:
- Environment:
- Customer / tenant:
- Change completed:
- Validation:
- Follow-up needed:
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
| Missing access | [Team / channel] | [Method] |
| Request unclear | [Team / channel] | [Method] |
| Procedure failure | [Team / channel] | [Method] |
| Production risk | [Team / channel] | [Method] |
| Security/compliance concern | [Team / channel] | [Method] |

---

## Related Links

- Service Overview: [Link]
- Related incident playbook: [Link]
- Dashboard / Workbook: [Link]
- Logs: [Link]
- Related automation runbook: [Link]
- Repository: [Link]
- Jira / request queue: [Link]

---

