# Server Specs
This directory holds backend planning and architecture notes for the StudyPath server implementation.
### Scope
- Controller, service, repository, and middleware design
- Database schema and migration planning
- Auth enforcement, moderation, and observability notes
- Implementation plans, rollout order, and handoff material
### Structure
- ackend/ contains architecture and transport-specific backend design notes.
- eatures/ contains phased implementation plans and handoff documents.
### Reference Order
- Start with ../Common/api for shared contracts.
- Use this directory for backend-only decisions that should not live in shared specs.
- Keep implementation order aligned with the client rollout plan whenever possible.
