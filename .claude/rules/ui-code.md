---
paths:
  - "src/ui/**"
---

# UI Code Rules

- UI must NEVER own or directly mutate application state — display only, use commands/events to request changes
- All user-facing strings must go through the i18n/localization system — no hardcoded display text
- All interactive elements must be keyboard-accessible
- All animations must respect user motion preferences (`prefers-reduced-motion`)
- UI must never block the main thread — use async operations for data fetching
- Scalable text and sufficient color contrast (WCAG 2.1 AA minimum) are required, not optional
- Test all screens at minimum and maximum supported viewport sizes
- Loading, empty, and error states must be explicitly handled for every data-dependent view
