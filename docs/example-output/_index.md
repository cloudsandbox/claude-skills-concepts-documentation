# Task Manager — Concepts Index

> **AI Agent Instructions**: Read this file first. Use the routing tables below to find the specific file for your task. Each sub-file is <400 lines.

## Quick Navigation by Task

| If you need to...                        | Read this file                              |
| ---------------------------------------- | ------------------------------------------- |
| Understand task lifecycle and statuses    | [tasks/README.md](tasks/README.md)          |
| See Task or Project field definitions     | [tasks/data-models.md](tasks/data-models.md)|
| Understand user roles and authentication  | [users/README.md](users/README.md)          |
| Configure notification rules and delivery | [notifications/README.md](notifications/README.md) |

## Navigation by Entity

| Entity               | Definition                                  | Behavior/Usage                              |
| -------------------- | ------------------------------------------- | ------------------------------------------- |
| **Task**             | [tasks/data-models.md](tasks/data-models.md)| [tasks/README.md](tasks/README.md)          |
| **Project**          | [tasks/data-models.md](tasks/data-models.md)| [tasks/README.md](tasks/README.md)          |
| **Comment**          | [tasks/data-models.md](tasks/data-models.md)| [tasks/README.md](tasks/README.md)          |
| **User**             | [users/data-models.md](users/data-models.md)| [users/README.md](users/README.md)          |
| **Notification**     | [notifications/data-models.md](notifications/data-models.md) | [notifications/README.md](notifications/README.md) |
| **NotificationRule** | [notifications/data-models.md](notifications/data-models.md) | [notifications/README.md](notifications/README.md) |

## Concept Overviews

| Concept       | README                                      | Description                                        |
| ------------- | ------------------------------------------- | -------------------------------------------------- |
| Tasks         | [tasks/README.md](tasks/README.md)          | Task lifecycle, projects, comments, and assignments |
| Users         | [users/README.md](users/README.md)          | User accounts, roles, and authentication            |
| Notifications | [notifications/README.md](notifications/README.md) | Event-driven alerts, rules, and delivery channels |

## File Structure

```
docs/concepts/
├── _index.md
├── _ownership.md
├── tasks/
│   ├── README.md
│   └── data-models.md
├── users/
│   ├── README.md
│   └── data-models.md
└── notifications/
    ├── README.md
    └── data-models.md
```

## Design Rules

1. Each file stays under 400 lines
2. Entity schemas defined in exactly ONE `data-models.md`
3. All links are relative — never absolute
4. API routes use `{param}` syntax (NOT `:param`)
5. Every file has the standard header (Part-of, Lines, Last-updated)
6. Every file ends with a Related Files table
7. Ownership defined in `_ownership.md` is canonical

## Version History

> Keep only the last 2 entries.

| Version | Date       | Author  | Changes                        |
| ------- | ---------- | ------- | ------------------------------ |
| 1.0     | 2026-03-04 | @claude | Initial concept documentation  |
