# System Architecture

This document describes the high-level architecture of the Clinical Workflow
Platform (CWP), including service boundaries, communication patterns, and core
architectural decisions.

The architecture is inspired by modern EPJ systems and is designed to support
scalability, reliability, and clear ownership of responsibilities.

## Architectural Principles

The system is built according to the following principles:

- Clear separation of bounded contexts
- Each service owns its data
- Asynchronous communication for cross-context workflows
- Eventual consistency where appropriate
- Observability and failure handling as first-class concerns

## High-Level Overview

The Clinical Workflow Platform consists of multiple independently deployable
services, each representing a bounded context. Services communicate using a
combination of synchronous APIs and asynchronous domain events.

```mermaid
flowchart LR
  UI --> GraphQL
  GraphQL --> Journal
  GraphQL --> Task

  Journal -->|Domain Events| ServiceBus
  ServiceBus --> Task
  ServiceBus --> Notification

### Journal Service

**Responsibility**
- Manage clinical journal notes
- Enforce journal-related business rules
- Publish journal-related domain events

**Communication**
- Exposes REST APIs for write operations
- Publishes domain events to the message broker

**Data Ownership**
- Owns all journal note data
- No other service accesses this data directly


### Task Service

**Responsibility**
- Manage tasks created as part of clinical workflows
- Track task state and completion

**Communication**
- Subscribes to domain events from the Journal Service
- Exposes REST APIs for task updates
- Publishes task-related domain events

**Data Ownership**
- Owns all task-related data

### Notification Service

**Responsibility**
- Deliver notifications to users
- React to task and journal-related events

**Communication**
- Subscribes to domain events
- Does not expose public APIs

**Data Ownership**
- Owns notification delivery state
## Communication Patterns
### Synchronous Communication (REST)

Synchronous REST APIs are used for command-style operations that require
immediate validation and feedback, such as creating a journal note or completing
a task.

### Asynchronous Communication (Domain Events)

Asynchronous messaging is used for cross-context workflows. This allows services
to remain loosely coupled and failure-tolerant.

Domain events are published to a message broker and consumed independently by
interested services.
## Command Query Responsibility Segregation (CQRS)

The system applies CQRS by separating write operations from read operations.

- Write operations are handled by REST APIs within individual services.
- Read operations are handled by a dedicated GraphQL gateway that aggregates
  data from read models.

This approach allows read models to be optimized for clinical dashboards and
overview screens without impacting write-side complexity.

## Data Management

Each service uses its own database and schema.

- Journal Service: Document-oriented storage
- Task Service: Relational or document storage
- Read models: Optimized projections

Shared databases are explicitly avoided to maintain service independence.

## Observability and Reliability

The system is designed to be observable and resilient in production.

- Distributed tracing across services
- Structured logging with correlation IDs
- Metrics for event processing and failures
- Graceful handling of partial failures

## Technology Choices (High-Level)

- Backend services: ASP.NET Core (.NET)
- Messaging: Azure Service Bus
- Data storage: Cosmos DB / Azure SQL
- Read API: GraphQL
- Observability: OpenTelemetry and Azure Application Insights
- Deployment: Azure Kubernetes Service (AKS)

