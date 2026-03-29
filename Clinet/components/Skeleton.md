## Spec: Skeleton

**Type**: `Component`  
**Location**: `Clinet/components/Skeleton.md`  
**Updated**: 2026-03-29  
**Status**: `Draft`

---

### Purpose

> Loading placeholder primitive that prevents layout shift.

---

### Responsibilities

- Serve as a reusable UI building block for multiple features.
- Keep accessibility, responsiveness, and visual consistency aligned with the shared design system.
- Stay composable so feature screens do not duplicate the same view logic.

---

### API Surface

- Expose the content, styling, and callback props required by the surrounding feature.
- Prefer a stable prop contract so adapters and feature screens stay simple.

---

### Behavior Notes

- Keep interactive, disabled, loading, and empty-adjacent behavior predictable where applicable.
- Avoid unnecessary layout shift when content or state changes.

---

### Test Notes

- Render correctly in default and variant states.
- Support keyboard and pointer interaction.
- Remain readable on both mobile and desktop layouts.
