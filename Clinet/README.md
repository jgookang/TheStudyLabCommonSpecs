# Clinet Specs
This directory contains client-facing design notes for StudyPath, including product features, UI components, state slices, and remote rollout material.
### Scope
- Feature flow and screen responsibilities
- Shared component contracts
- Zustand store responsibilities
- Client adapter, rollout, and smoke-test guides
### Structure
- pi/ contains integration and rollout docs for moving from mock data to the real backend.
- eatures/ contains product-level feature specifications.
- store/ contains local and global state contracts.
- components/ contains reusable UI component specs.
### Working Rules
- Check ../Common/api before changing client payload handling.
- Keep UI models separate from raw DTOs when the payload shape is not ideal for rendering.
- Use remote smoke tests and captured fixtures to verify rollout changes.
