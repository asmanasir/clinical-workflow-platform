# Domain Overview

The Clinical Workflow Platform models core clinical workflows found in EPJ
systems, focusing on journal notes, tasks, and notifications.

## Bounded Contexts

**Journal (Core Domain)**
- Manages clinical journal notes
- Emits events when notes are created or updated

**Task**
- Manages tasks triggered by clinical workflows
- Reacts to journal-related events

**Notification**
- Sends user notifications
- Non-blocking and failure-tolerant

## Key Rules
- Each context owns its data
- Contexts communicate via events
- No shared databases
