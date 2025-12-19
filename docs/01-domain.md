# Domain Overview

The Clinical Workflow Platform (CWP) is a distributed system designed to model
core clinical workflows found in Electronic Patient Journal (EPJ) systems.

The goal of the platform is to demonstrate how clinical information flows across
multiple bounded contexts in a safe, reliable, and scalable way, without using
real medical data.

## Problem Statement

In clinical environments, a single action—such as writing a journal note—often
triggers multiple downstream processes. These processes may include task
creation, notifications, audits, and updates to clinical overviews.

Traditional monolithic systems struggle with scalability, failure isolation,
and clear ownership of responsibilities. This platform explores a distributed,
event-driven approach to modeling such workflows, focusing on reliability,
traceability, and domain boundaries.

## Ubiquitous Language

The following terms are used consistently across the system and documentation:

- **Patient**: A person receiving clinical care.
- **Journal Note**: A clinical note created by healthcare personnel.
- **Task**: An action that must be performed as part of a clinical workflow.
- **Notification**: A message informing a user that attention is required.
- **Clinical Workflow**: A sequence of steps triggered by clinical actions.
- **Event**: A fact that something has happened in the system.
  
## Bounded Contexts

The system is divided into distinct bounded contexts. Each context owns its
data, behavior, and business rules, and communicates with other contexts through
well-defined events and APIs.

### Patient Context

**Responsibility**  
The Patient Context is responsible for managing patient identity and basic
demographic information.

**Owned Data**
- Patient identifier
- Demographic metadata

**Key Characteristics**
- Data changes infrequently
- Referenced by multiple other contexts
- Acts as a stable source of truth for patient identity

### Journal Context (Core Domain)

**Responsibility**  
The Journal Context is the core domain of the system. It manages clinical
journal notes, including creation, updates, and versioning.

**Owned Data**
- Journal notes
- Note versions
- Audit metadata

**Key Characteristics**
- Central to most clinical workflows
- Emits events that trigger downstream processing
- High requirements for correctness and traceability

### Task Context

**Responsibility**  
The Task Context manages tasks that arise from clinical workflows, such as
reviewing or signing a journal note.

**Owned Data**
- Task definitions
- Task status and deadlines

**Key Characteristics**
- Reacts to events from other contexts
- Must handle duplicate and out-of-order events safely
- Supports eventual consistency

### Notification Context

**Responsibility**  
The Notification Context informs users about relevant events requiring
attention, such as newly created tasks.

**Owned Data**
- Notification records
- Delivery status

**Key Characteristics**
- Non-blocking
- Failure-tolerant
- Does not affect core clinical workflows
## Context Relationships

- The Journal Context emits events when journal notes are created or updated.
- The Task Context subscribes to journal events to create relevant tasks.
- The Notification Context subscribes to task-related events to notify users.
- No context directly accesses another context’s database.

 ## Non-Goals

The following concerns are explicitly out of scope for this platform:

- Real patient data
- Clinical decision support
- Regulatory compliance (e.g. GDPR, HL7, FHIR)
- User authentication and authorization



