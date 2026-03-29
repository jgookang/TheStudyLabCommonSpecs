## Spec: API Client Wrapper

**Type**: `Client API Guide`  
**Location**: `Clinet/api/client-wrapper.md`  
**Updated**: 2026-03-29  
**Status**: `Implemented`

---

### Purpose

> Shared client wrapper for fetch setup, query building, error handling, and adapter boundaries.

---

### Core Responsibilities

- Keep the client rollout path clear and repeatable.
- Align client behavior with the shared contracts under `Common/api`.
- Document what needs to happen before, during, and after remote integration changes.

---

### Operating Rules

- Prefer stable adapters between remote DTOs and UI-facing models.
- Capture real payload differences in both fixtures and shared specs.
- Record blockers instead of hiding them in ad hoc implementation notes.

---

### Verification Notes

- Run smoke or targeted verification whenever a feature changes data source.
- Keep examples and results documents current enough for the next engineer to continue.
