# Tasks Data Models

> **Part of**: [tasks/](./README.md) · **Lines**: ~80 · **Last updated**: 2026-03-04

## Task

```
Task
├── id: UUID (PK)
├── title: string
├── description: text (nullable)
├── status: 'OPEN' | 'IN_PROGRESS' | 'DONE' | 'CLOSED'
├── priority: 'LOW' | 'MEDIUM' | 'HIGH' | 'URGENT'
├── assigneeId: UUID (FK -> User, nullable)
├── projectId: UUID (FK -> Project)
├── createdAt: timestamp
└── updatedAt: timestamp
```

### Constraints

- `title` is required and must be 1-255 characters
- `status` defaults to `'OPEN'` on creation
- `priority` defaults to `'MEDIUM'` on creation
- `assigneeId` is nullable — unassigned tasks are allowed
- `projectId` is required — every task belongs to a project

---

## Project

```
Project
├── id: UUID (PK)
├── name: string
├── description: text (nullable)
├── ownerId: UUID (FK -> User)
├── createdAt: timestamp
└── updatedAt: timestamp
```

### Constraints

- `name` is required, unique per owner, 1-100 characters
- `ownerId` is required — every project has an owner

---

## Comment

```
Comment
├── id: UUID (PK)
├── body: text
├── taskId: UUID (FK -> Task)
├── authorId: UUID (FK -> User)
├── createdAt: timestamp
└── updatedAt: timestamp
```

### Constraints

- `body` is required and must be 1-5000 characters
- `taskId` is required — every comment belongs to a task
- `authorId` is required — every comment has an author

---

## Entity Relationships

```
Project 1──* Task *──1 User (assignee)
                |
                └──* Comment *──1 User (author)
```

- A **Project** has many **Tasks**; a Task belongs to one Project
- A **User** can be assigned to many Tasks; a Task has at most one assignee
- A **Task** has many **Comments**; a Comment belongs to one Task
- A **User** authors many Comments; a Comment has one author

## Code References

| Component       | Location                               |
| --------------- | -------------------------------------- |
| Task entity     | `src/tasks/entities/task.entity.ts`    |
| Project entity  | `src/tasks/entities/project.entity.ts` |
| Comment entity  | `src/tasks/entities/comment.entity.ts` |

## Related Files

| Topic            | File                                          |
| ---------------- | --------------------------------------------- |
| Tasks overview   | [README.md](./README.md)                      |
| User schema      | [../users/data-models.md](../users/data-models.md) |
| Navigation hub   | [../\_index.md](../_index.md)                 |
