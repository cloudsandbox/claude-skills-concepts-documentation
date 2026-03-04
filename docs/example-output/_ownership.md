# Entity Ownership Registry

> Canonical source for each entity. When files disagree, the owner wins.
> **Last updated**: 2026-03-04

## Ownership Table

| Entity               | Canonical Owner      | Referenced By              |
| -------------------- | -------------------- | -------------------------- |
| **Task**             | `tasks/`             | `notifications/`, `users/` |
| **Project**          | `tasks/`             | `users/`                   |
| **Comment**          | `tasks/`             | `users/`                   |
| **User**             | `users/`             | `tasks/`, `notifications/` |
| **Notification**     | `notifications/`     | `tasks/`, `users/`         |
| **NotificationRule** | `notifications/`     | —                          |

## Ownership Rules

1. **Canonical owner defines**: entity name, fields, types, relationships, constraints
2. **Referencing files must**: use exact entity names, link to owner, not redefine behavior
3. **New entities**: add to this table, declare in owner README, update referencing concepts

## Conflict Resolution

1. Identify the canonical owner from the table above
2. Owner's definition wins — no exceptions
3. Update the referencing file to match the owner
4. If the owner is wrong, fix the owner first, then propagate

## Cross-Concept Entities

| Scenario                                          | Resolution                                                    |
| ------------------------------------------------- | ------------------------------------------------------------- |
| Task references User (assignee)                   | `tasks/` owns Task; links to `users/` for User definition     |
| Notification triggered by Task status change       | `notifications/` owns Notification; references `tasks/` for trigger |
| Comment authored by User on a Task                 | `tasks/` owns Comment; references `users/` for author          |

## Adding New Concepts

1. Identify all entities the new concept will own
2. Add entries to the Ownership Table
3. Identify which existing concepts it references
4. Update those concepts' "Referenced By" columns
5. Declare ownership in the new README.md

## Related Files

| Topic          | File                                 |
| -------------- | ------------------------------------ |
| Navigation hub | [_index.md](./_index.md)             |
