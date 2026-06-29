## CloudOps Playbook Templates

This package contains three GitHub-ready Markdown templates:

- `service-overview-template.md`
- `incident-playbook-template.md`
- `operations-playbook-template.md`

## Recommended repository structure

Playbook documentation should be stored under `/docs/<service-name>/...`.

This structure is required because some repositories may contain more than one service. Each service should have its own documentation folder so the portal aggregation process can identify, validate, and publish service documentation independently.

```text
/service-repo
  /docs
    /<service-name>
      /service-overview
        service-overview.md
      /playbooks
        /incident
          high-error-rate.md
        /operations
          cloud-deployment-request.md
```

## Example: repository with one service

```text
/payments-service-repo
  /docs
    /payments-api
      /service-overview
        service-overview.md
      /playbooks
        /incident
          high-error-rate.md
          queue-backlog.md
        /operations
          certificate-rotation.md
          customer-access-update.md
```

## Example: repository with multiple services

```text
/platform-services-repo
  /docs
    /identity-service
      /service-overview
        service-overview.md
      /playbooks
        /incident
          authentication-failure.md
        /operations
          rotate-client-secret.md
    /notification-service
      /service-overview
        service-overview.md
      /playbooks
        /incident
          message-delivery-delay.md
        /operations
          update-notification-template.md
```

## Documentation categories

Each service folder should contain the following documentation areas:

### Service Overview

The Service Overview explains what the service is, how it works, who owns it, and where to monitor or support it.

Expected path:

```text
/docs/<service-name>/service-overview/service-overview.md
```

### Incident Playbooks

Incident Playbooks provide short, crisis-friendly instructions for responding to service issues, alerts, degradation, or customer-impacting incidents.

Expected path:

```text
/docs/<service-name>/playbooks/incident/<incident-name>.md
```

### Operations Playbooks

Operations Playbooks provide step-by-step instructions for repeatable operational work, including procedures, maintenance, request fulfillment, deployments, rotations, and standard service tasks.

Expected path:

```text
/docs/<service-name>/playbooks/operations/<procedure-name>.md
```

## Naming guidance

Use lowercase, hyphenated file and folder names where possible.

Examples:

```text
/docs/payments-api/service-overview/service-overview.md
/docs/payments-api/playbooks/incident/high-error-rate.md
/docs/payments-api/playbooks/operations/certificate-rotation.md
```

## Portal aggregation notes

The playbook portal will use the service-specific path to aggregate documentation into the centralized portal.

Each service entry in the portal registry should point to the correct service documentation path, such as:

```yaml
service_name: payments-api
repository: forescout/payments-service-repo
branch: main
docs_path: docs/payments-api
```

For repositories with multiple services, each service should be registered separately:

```yaml
service_name: identity-service
repository: forescout/platform-services-repo
branch: main
docs_path: docs/identity-service

service_name: notification-service
repository: forescout/platform-services-repo
branch: main
docs_path: docs/notification-service
```
