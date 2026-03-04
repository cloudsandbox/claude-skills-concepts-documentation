# Tasks

> **Part of**: [concepts/](../_index.md) · **Lines**: ~70 · **Last updated**: 2026-03-04

## Summary

The tasks concept covers the core work-tracking domain: creating tasks within projects, assigning them to users, tracking status through a defined lifecycle, and attaching comments for collaboration. Tasks are the central entity in the system — most other concepts reference them.

## Owned Entities

- **Task** — A unit of work with status, priority, and assignee
- **Project** — A container that groups related tasks
- **Comment** — A text note attached to a task by a user

## Referenced By

- `users/` — User is referenced as task assignee and comment author
- `notifications/` — Task status changes trigger notifications

## Key Concepts

| Concept          | Description                                                        |
| ---------------- | ------------------------------------------------------------------ |
| Task Lifecycle   | Tasks move through OPEN -> IN_PROGRESS -> DONE -> CLOSED           |
| Project Scoping  | Every task belongs to exactly one project                          |
| Assignment       | Tasks may be assigned to a single user (nullable)                  |
| Comments         | Chronological discussion thread attached to a task                 |

## When to Read Which File

| If you need to...                         | Read                                    |
| ----------------------------------------- | --------------------------------------- |
| See Task, Project, or Comment schemas     | [data-models.md](./data-models.md)      |
| Understand task statuses and transitions  | This file (Key Concepts above)          |
| Know how tasks trigger notifications      | [../notifications/README.md](../notifications/README.md) |
| Understand who can be assigned to tasks   | [../users/README.md](../users/README.md)|

## Related Files

| Topic                | File                                                       |
| -------------------- | ---------------------------------------------------------- |
| Entity schemas       | [data-models.md](./data-models.md)                         |
| Navigation hub       | [_index.md](../_index.md)                                  |
| Entity ownership     | [_ownership.md](../_ownership.md)                          |
| Users concept        | [users/README.md](../users/README.md)                      |
| Notifications concept| [notifications/README.md](../notifications/README.md)      |

## Code References

| Component            | Location                                  |
| -------------------- | ----------------------------------------- |
| Task entity          | `src/tasks/entities/task.entity.ts`       |
| Project entity       | `src/tasks/entities/project.entity.ts`    |
| Comment entity       | `src/tasks/entities/comment.entity.ts`    |
| Tasks service        | `src/tasks/tasks.service.ts`              |
| Tasks controller     | `src/tasks/tasks.controller.ts`           |
