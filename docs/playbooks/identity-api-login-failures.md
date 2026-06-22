---
title: Fictional Identity API - Login Failures
owner: Identity Platform Team
severity: High
last_reviewed: 2026-06-22
automation_available: true
---

# Fictional Identity API - Login Failures Playbook

## When to use this playbook

Use this playbook when:

- Users report being unable to log in to applications using the Fictional Identity API
- Login success rate drops below 95% (automated alert threshold)
- Multiple authentication attempts fail with credential validation errors
- External systems integrating with the Identity API report authentication timeouts
- The AuthenticationService experiences elevated error rates (>5% of requests)

**Do not use this playbook for:**
- Password reset requests (see: [Password Reset Playbook](./identity-api-password-reset.md))
- Multi-factor authentication failures (see: [MFA Issues Playbook](./identity-api-mfa-issues.md))
- Account lockout procedures (see: [Account Security Playbook](./identity-api-account-security.md))

## Symptoms

Users or systems may exhibit the following symptoms:

- **Authentication rejection**: Login attempts fail with error code `AUTH_INVALID_CREDENTIALS`
- **Session timeout**: Users are logged out unexpectedly after 5-10 minutes of inactivity
- **Slow authentication**: Login requests take >30 seconds to complete
- **Token generation failures**: Error response: `TOKEN_GENERATION_FAILED` (HTTP 500)
- **Service unavailable**: Identity API responds with HTTP 503 or connection timeouts
- **Cascading failures**: Multiple client applications unable to authenticate simultaneously
- **Error logs**: CloudWatch logs show spike in `InvalidCredentialsException` or `ServiceUnavailableException`

## Initial checks

Perform these checks in order to diagnose the issue:

### 1. Verify service health (< 2 minutes)

```bash
# Check the Identity API health endpoint
curl -s https://identity-api.fictional.internal/health | jq '.status'

# Expected response: { "status": "healthy", "timestamp": "..." }
```

If unhealthy, proceed to Approved Actions - Service Recovery.

### 2. Check authentication service metrics (< 2 minutes)

In the CloudWatch dashboard `Identity-API-Metrics`:
- Monitor `AuthenticationAttempts` (counter)
- Monitor `FailedAuthentications` (counter)
- Check `TokenGenerationLatency` (p95 should be < 500ms)
- Verify database connection pool utilization (< 80%)

### 3. Review recent deployments (< 5 minutes)

Check the deployment log in the Identity Team Slack channel or deployment dashboard:
- Was there a deployment in the last 2 hours?
- Are there any ongoing canary deployments to the AuthenticationService?
- Check feature flags: has `enforce_mfa_for_legacy_clients` been enabled recently?

### 4. Verify database connectivity (< 2 minutes)

Connect to the Identity database read replica:

```bash
# SSH into the database bastion
ssh identity-db-bastion

# Test connection to primary database
psql -h identity-db-primary.rds.fictional.internal \
  -U health_check \
  -d identity_service \
  -c "SELECT now();"
```

If the connection fails, database connectivity is the issue.

### 5. Check credential validation service (< 3 minutes)

Query the internal credential validation service:

```bash
# Test credential validation with a known test account
curl -X POST https://identity-api.fictional.internal/validate \
  -H "Content-Type: application/json" \
  -d '{"username":"testuser@fictional.internal","password":"test-password-hash"}'
```

Expected response should be a validation status (not a 5xx error).

## Approved actions

### Action 1: Restart the AuthenticationService

**Conditions**: Elevated latency observed but health checks pass.

1. Access the Kubernetes cluster: `kubectl config use-context identity-prod`
2. Identify the affected pods: `kubectl get pods -n identity-api -l app=auth-service`
3. Perform a rolling restart:
   ```bash
   kubectl rollout restart deployment/auth-service -n identity-api
   ```
4. Monitor rollout: `kubectl rollout status deployment/auth-service -n identity-api`
5. Validate: Confirm login success rate returns to > 95% within 5 minutes

### Action 2: Clear authentication cache

**Conditions**: Invalid credentials errors persist but database connectivity is confirmed.

1. Access the Redis cache cluster:
   ```bash
   redis-cli -h identity-cache-primary.fictional.internal -p 6379
   ```
2. Clear the token blacklist cache:
   ```
   FLUSHDB
   ```
3. Verify cache is empty: `DBSIZE` should return 0
4. Monitor: Confirm login attempts succeed within 2 minutes

**Note**: Clearing the cache may invalidate existing sessions. Users may need to log in again.

### Action 3: Enable connection pooling recovery

**Conditions**: Database connection pool is exhausted (> 90% utilization).

1. Connect to the Identity API configuration service:
   ```bash
   kubectl port-forward svc/config-service 8080:8080 -n identity-api
   ```
2. Update connection pool settings:
   ```bash
   curl -X PATCH http://localhost:8080/config/db-pool \
     -H "Content-Type: application/json" \
     -d '{"max_connections": 50, "idle_timeout_ms": 30000}'
   ```
3. Monitor pool utilization: Check CloudWatch metric `DBConnectionPoolUtilization`
4. Verify authentication latency returns to normal (< 500ms p95)

### Action 4: Rollback recent deployment

**Conditions**: Deployment occurred < 2 hours ago and login failures correlate with deployment time.

1. Check recent deployments: `kubectl rollout history deployment/auth-service -n identity-api`
2. Identify the previous stable revision
3. Rollback:
   ```bash
   kubectl rollout undo deployment/auth-service -n identity-api --to-revision=<PREVIOUS_REVISION>
   ```
4. Verify: Confirm login success rate returns to > 95% within 5 minutes
5. Notify: Post in #identity-team Slack channel with rollback reason

### Action 5: Failover to secondary data center

**Conditions**: Primary data center connectivity is compromised and local recovery is not working.

1. Verify secondary data center status: `https://status.fictional.internal/identity-api-secondary`
2. Update DNS records to point to secondary:
   ```bash
   # Contact Infrastructure team or use DNS management tool
   # Update A records for identity-api.fictional.internal to secondary IPs
   ```
3. Monitor login success metrics for secondary (allow 60-90 seconds for DNS propagation)
4. Post incident to #incidents and #identity-team
5. Coordinate with Infrastructure team for primary data center investigation

## Do NOT actions

**⛔ Do NOT attempt these actions:**

- **Do NOT reset user passwords in bulk** - This will cascade to additional login failures. Users must reset passwords individually through the password reset flow.
- **Do NOT disable MFA enforcement** - Even if MFA is causing delays, disabling it requires security review. Use the feature flag `strict_mfa_enforcement: false` instead (requires audit log entry).
- **Do NOT delete authentication tokens from the database directly** - Use the token revocation API (`POST /tokens/revoke`) instead.
- **Do NOT modify credential validation rules** without approval from the Security team. Contact @security-oncall.
- **Do NOT restart the database** - Contact the DBA on-call team. Unplanned restarts can cause data inconsistency.
- **Do NOT clear the entire Redis cache in production** - Use targeted cache invalidation (`FLUSHDB` only in emergency escalation scenarios).
- **Do NOT expose internal logs or stack traces to end users** - Always return generic error messages. Log detailed information for internal diagnostics only.

## Validation

After implementing any approved action, validate the fix:

### Success criteria

- [ ] Login success rate is > 95% (check CloudWatch dashboard)
- [ ] AuthenticationService error rate is < 2%
- [ ] Token generation latency (p95) is < 500ms
- [ ] No authentication-related errors in the past 5 minutes
- [ ] At least 10 successful test logins from different client applications
- [ ] Database connection pool utilization is < 70%

### Validation commands

```bash
# Check overall metrics
curl https://identity-api.fictional.internal/metrics | grep login_success_rate

# Perform a test login
curl -X POST https://identity-api.fictional.internal/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username":"qa-testuser@fictional.internal","password":"secure-test-password"}'

# Verify token is valid
# Extract the token from the response and:
curl https://identity-api.fictional.internal/auth/validate \
  -H "Authorization: Bearer <TOKEN>"

# Expected response: { "valid": true, "username": "qa-testuser@fictional.internal", ... }
```

### Rollback conditions

If validation fails, immediately rollback:
1. Undo the action taken (reverse deploy, restore cache, etc.)
2. Wait 2-3 minutes for metrics to stabilize
3. If still failing, escalate to the on-call engineer

## Escalation

Escalate the incident if any of the following conditions apply:

### Escalation triggers

1. **Unresolved after 15 minutes**: Issue persists after completing all approved actions
2. **Multiple failures**: Login failures spanning multiple data centers or regions
3. **Data integrity concerns**: Errors indicating potential database corruption
4. **Security incident**: Suspicious patterns suggesting unauthorized access attempts or credential stuffing
5. **Customer impact**: Business-critical integrations unable to authenticate
6. **Unknown root cause**: None of the initial checks identify the problem

### Escalation procedure

1. **Notify the on-call engineer**:
   - Slack: `@identity-api-oncall`
   - PagerDuty: Trigger incident via `#incidents` channel
   - Phone: Use escalation tree (see: [On-Call Escalation List](../escalation-contacts.md))

2. **Provide context**:
   - Summary of symptoms and initial checks performed
   - Relevant CloudWatch dashboard link
   - Recent deployments or changes
   - Customer/system impact estimate

3. **Involve Infrastructure team** (if database or network issues suspected):
   - Slack: `@infrastructure-oncall`
   - Reference ticket: INFRA-ONCALL-{incident-id}

4. **Involve Security team** (if authentication patterns suggest attacks):
   - Slack: `@security-oncall`
   - Do not share credentials or sensitive authentication data

5. **Open a P1 incident** if:
   - Login failure rate > 10%
   - Duration > 30 minutes
   - Multiple critical services affected

## References

### Related documentation

- [Identity API Architecture](../architecture/identity-api.md)
- [Authentication Flow Diagram](../diagrams/authentication-flow.md)
- [Credential Validation Service API](../api/credential-validation.md)
- [CloudWatch Dashboards Guide](../monitoring/dashboards.md)

### Useful links

- **CloudWatch Dashboard**: [Identity API Metrics](https://console.aws.fictional.internal/cloudwatch/home?region=us-east-1#dashboards:name=Identity-API-Metrics)
- **Kubernetes Dashboard**: [identity-api namespace](https://k8s-dashboard.fictional.internal/#!/namespace/identity-api)
- **Deployment Pipeline**: [Identity API Builds](https://ci.fictional.internal/job/identity-api)
- **Status Page**: [Fictional Services Status](https://status.fictional.internal)
- **Incident History**: [Past Authentication Incidents](https://incidents.fictional.internal/search?service=identity-api&issue=login-failures)

### Common error codes

| Error Code | Meaning | Typical Cause |
|------------|---------|---------------|
| `AUTH_INVALID_CREDENTIALS` | Username or password incorrect | User error or account compromise |
| `TOKEN_GENERATION_FAILED` | System unable to create session token | Service malfunction, database issue |
| `SERVICE_UNAVAILABLE` | Identity API is down or unreachable | Deployment issue, infrastructure failure |
| `DB_CONNECTION_TIMEOUT` | Database connection pool exhausted | High load, connection leak |
| `CREDENTIAL_VALIDATION_ERROR` | Credential validation service unreachable | Service dependency failure |

### Contacts

- **Identity Platform Team Lead**: alice.chen@fictional.internal
- **On-Call Rotation**: See #identity-api-oncall Slack channel
- **Infrastructure Support**: infrastructure-team@fictional.internal
- **Security Team**: security@fictional.internal
