# System Architecture

The system consists of multiple independent services representing bounded
contexts.

## Overview

- UI consumes data via GraphQL
- Write operations use REST APIs
- Cross-context workflows use asynchronous events

```mermaid
flowchart LR
  UI --> GraphQL
  GraphQL --> Journal
  Journal --> ServiceBus
  ServiceBus --> Task
