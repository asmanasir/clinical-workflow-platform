# Domain Events

This document describes the domain events used in the Clinical Workflow Platform
(CWP). Domain events represent facts that have occurred in the system and are
used to communicate between bounded contexts in an asynchronous and loosely
coupled manner.

## What is a Domain Event?

A domain event is an immutable statement that something meaningful has happened
within a bounded context.

Domain events:
- Are named in the past tense
- Describe *what happened*, not *what should happen*
- Are owned and published by a single bounded context
- May be consumed by multiple other contexts

## Event Ownership Rules

- Each domain event is owned by exactly one bounded context.
- Only the owning context is allowed to publish the event.
- Other contexts may subscribe to the event but must not assume
  implementation details or internal state.
- Events are treated as public contracts and versioned carefully.


## Domain Event Catalog


### JournalNoteCreated

**Owning Context**  
Journal Context

**Description**  
Emitted when a new journal note has been created successfully.

**Why this event exists**  
Creating a journal note is a core clinical action that triggers downstream
processes such as task creation and notifications.

**Consumers**
- Task Context
- Notification Context

**Payload (logical)**
- JournalNoteId
- PatientId
- CreatedBy
- CreatedAt


### JournalNoteUpdated

**Owning Context**  
Journal Context

**Description**  
Emitted when an existing journal note is updated or a new version is created.

**Why this event exists**  
Downstream systems may need to refresh projections or invalidate caches when
journal content changes.

**Consumers**
- Read Model
- Caching layer

**Payload (logical)**
- JournalNoteId
- PatientId
- UpdatedAt
- Version

### TaskCreated

**Owning Context**  
Task Context

**Description**  
Emitted when a new task has been created as a result of a clinical workflow.

**Why this event exists**  
Tasks represent actionable items for healthcare personnel and may trigger
notifications or UI updates.

**Consumers**
- Notification Context
- Read Model

**Payload (logical)**
- TaskId
- RelatedEntityId
- TaskType
- DueDate
### TaskCompleted

**Owning Context**  
Task Context

**Description**  
Emitted when a task has been completed.

**Why this event exists**  
Completion of a task may affect clinical workflows, dashboards, and audits.

**Consumers**
- Read Model
- Notification Context

**Payload (logical)**
- TaskId
- CompletedBy
- CompletedAt

## Event Delivery Guarantees

- Events are delivered at least once.
- Consumers must be idempotent.
- Events may arrive out of order.
- Consumers must not assume immediate consistency.

## Event Versioning

Domain events are versioned to allow safe evolution over time.

- New fields may be added in a backward-compatible way.
- Breaking changes require a new event version.
- Consumers must tolerate unknown fields.

## What Domain Events Do Not Do

- Events do not contain business logic.
- Events do not trigger synchronous behavior.
- Events do not guarantee immediate reactions.

