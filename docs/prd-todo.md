# Product Requirements Document (PRD) - TODO App Upgrade

## 1. Overview

We are upgrading the basic TODO app (currently storing only `title` and `completed`) to support due dates, priority levels, and date-based filters. The goal is to make the app more practical for users who need to track urgency and organize tasks by deadline — without introducing backend complexity or over-engineering the solution. All data will remain in local storage.

---

## 2. MVP Scope

- **Due date field** — add an optional `dueDate` field (ISO `YYYY-MM-DD`) to each task; invalid values are treated as absent
- **Priority field** — add a `priority` field with enum values `P1 | P2 | P3`, defaulting to `P3`; displayed as color-coded badges (P1 = red, P2 = orange, P3 = gray)
- **Filter tabs** — three views: **All**, **Today**, **Overdue**
  - **All**: shows all tasks, including completed ones
  - **Today**: shows only incomplete tasks due today
  - **Overdue**: shows only incomplete tasks with a past due date
- **Data model validation**
  - `title`: required
  - `priority`: must be `"P1"`, `"P2"`, or `"P3"`; defaults to `"P3"` if absent or invalid
  - `dueDate`: optional ISO `YYYY-MM-DD`; invalid values ignored (treated as absent)
- **Local storage only** — no backend or external storage changes

---

## 3. Post-MVP Scope

- **Overdue highlighting** — overdue tasks are visually highlighted in red so they stand out at a glance
- **Sorting rules** — tasks are sorted in the following order:
  1. Overdue tasks first
  2. Then by priority (P1 → P2 → P3)
  3. Then by due date ascending
  4. Tasks without a due date listed last

---

## 4. Out of Scope

- Notifications / reminders
- Recurring tasks
- Multi-user support
- Keyboard navigation and accessibility features
- External storage or backend integration
