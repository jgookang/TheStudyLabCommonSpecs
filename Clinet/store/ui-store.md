## Spec: uiStore

**Type**: $Type  
**Location**: $Location  
**Updated**: 2026-03-29  
**Status**: $Status

---

### Purpose

> Manage global browser-only UI state such as toasts, overlays, and global modal routing.

---

### State Model

- Store the toast queue with tone, message, optional description, and action labels.
- Store the active modal id and optional modal props.
- Optionally track the maximum number of visible toasts.

---

### Actions

- `pushToast()` adds a toast to the queue.
- `dismissToast(id)` removes a single toast.
- `clearToasts()` empties the queue.
- `openModal(id, props?)` opens a named global modal.
- `closeModal()` clears the active modal state.

---

### Operational Rules

- Keep server payloads out of this slice.
- Prefer serializable modal props so deep links and debugging stay reasonable.
- Decide how duplicate toasts should behave before connecting multiple async flows.

---

### Test Notes

- Toasts are appended and dismissed by id.
- Opening a modal stores both the id and the provided props.
- Closing a modal resets the modal state cleanly.
