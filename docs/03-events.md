# Domain Events

Domain events represent facts that have occurred and are used for asynchronous
communication between bounded contexts.

## Events

**JournalNoteCreated**
- Owner: Journal
- Consumers: Task, Notification
- Purpose: Trigger downstream workflows

**JournalNoteUpdated**
- Owner: Journal
- Consumers: Read models
- Purpose: Refresh projections and caches

**TaskCreated**
- Owner: Task
- Consumers: Notification, Read models

**TaskCompleted**
- Owner: Task
- Consumers: Read models

## Delivery Rules
- Events are delivered at least once
- Consumers must be idempotent
- Eventual consistency is expected
