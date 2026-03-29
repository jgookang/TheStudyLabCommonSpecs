# Common Specs
This directory holds the shared contracts consumed by both the client and the backend.
### Scope
- API endpoint contracts
- Request and response payload shape
- Auth rules and error envelopes
- Validation, pagination, timezone, and transport rules
### Structure
- api/ stores endpoint-level specs that both the web and mobile clients can reference.
### Editing Rules
- Update the shared contract first when a transport rule or payload field changes.
- Document web and mobile auth separately whenever the transport differs.
- Prefer stable, implementation-ready wording so adapters and smoke tests can use the same source.
