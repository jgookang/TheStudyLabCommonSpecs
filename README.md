# CommonSpecs
CommonSpecs is the shared specification workspace for StudyPath. It collects cross-team contracts, client plans, and server implementation notes in one place.
### Workspace Layout
- Common/ contains shared API contracts and payload rules.
- Server/ contains backend design notes, implementation plans, and handoff material.
- Clinet/ contains client feature specs, store contracts, component specs, and rollout guides.
### Source of Truth Rules
- Shared API contracts, auth policy, pagination rules, and error envelopes live under Common/.
- Server-only implementation decisions live under Server/.
- Client-only UI, state, adapter, and rollout decisions live under Clinet/.
- Application repositories should link back to these specs instead of copying them by hand.
### Auth Policy
- Web auth uses a same-site deployment model with an HttpOnly refresh cookie.
- Web refresh failures return explicit 401 responses so the client can move to a signed-out state.
- Mobile auth uses the same backend auth core, but the refresh token is transported through secure storage.
### Working Agreement
- Update shared contracts before changing transport-specific implementations.
- When real backend payloads differ from the spec, fix the spec and the client adapters in the same pass.
- Keep Markdown docs in English so cross-team review stays easy.
