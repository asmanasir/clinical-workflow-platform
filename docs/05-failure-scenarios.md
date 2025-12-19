# Failure Scenarios

- If a consumer is down, events accumulate and are processed later
- Duplicate events are handled via idempotency
- Partial failures do not block core workflows
