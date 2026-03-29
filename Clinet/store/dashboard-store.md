## Spec: dashboardStore

**Type**: $Type  
**Location**: $Location  
**Updated**: 2026-03-29  
**Status**: $Status

---

### Purpose

> Hold dashboard-local UI state that should not live in server caches.

---

### State Model

- Store the active `period` as `daily`, `weekly`, or `monthly`.
- Store the selected date for the mini calendar as an ISO string or `null`.
- Keep server payloads out of this store so React Query remains the source of truth for fetched data.

---

### Actions

- `setPeriod(period)` switches the dashboard tab state.
- `setSelectedDate(date)` records the active calendar selection.
- `resetDashboardUi()` returns the slice to its default state.

---

### Operational Rules

- Keep the selected date format timezone-safe and consistent across widgets.
- Changing `period` should line up with the query keys used for metrics and analytics.
- Local UI state can expand later for widget collapse, filters, or pinning without storing fetched payloads.

---

### Test Notes

- Default state is `daily` with no selected date.
- Period changes are stored exactly as supplied.
- Reset returns the slice to the default values.
