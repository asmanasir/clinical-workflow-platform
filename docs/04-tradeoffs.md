# Architectural Trade-offs

This document summarizes key architectural decisions made in the Clinical
Workflow Platform and the trade-offs associated with them.

## Event-Driven Communication
**Decision:** Use asynchronous domain events for cross-context workflows.  
**Trade-off:** Increased complexity and eventual consistency in exchange for
loose coupling, scalability, and failure isolation.

## CQRS
**Decision:** Separate write operations (REST) from read operations (GraphQL).  
**Trade-off:** Additional infrastructure and read models, but simpler write-side
logic and optimized read performance for clinical dashboards.

## Database per Service
**Decision:** Each service owns its own database.  
**Trade-off:** No cross-service transactions, but clearer ownership and safer
independent deployments.

## Eventual Consistency
**Decision:** Accept eventual consistency across bounded contexts.  
**Trade-off:** Data may be temporarily out of sync, but system remains available
and resilient during partial failures.

## Minimal UI
**Decision:** Keep UI simple and focused on workflows.  
**Trade-off:** Less visual polish, but more time spent on backend architecture,
which is the core focus of the project.
