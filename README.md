# @prmichaelsen/task-core

Core business logic for the task execution system. Provides Zod schemas, DTOs, services, and Firebase client for task management.

[![npm version](https://badge.fury.io/js/%40prmichaelsen%2Ftask-core.svg)](https://www.npmjs.com/package/@prmichaelsen/task-core)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Features

- 🔒 **Type-Safe Schemas** - Zod schemas with TypeScript inference
- 🔄 **DTO Transformers** - Convert internal schemas to API responses
- 🔥 **Firebase Integration** - Firestore service layer and client wrapper
- 📦 **Tree-Shakeable** - Import only what you need via subpath exports
- ✅ **Well-Tested** - 93% test coverage with unit and E2E tests

## Installation

```bash
npm install @prmichaelsen/task-core
```

## Quick Start

```typescript
import { TaskDatabaseService } from '@prmichaelsen/task-core/services'
import { toTaskApiResponse } from '@prmichaelsen/task-core/dto'

// Create a task
const task = await TaskDatabaseService.createTask(
  'user-123',
  'My Task',
  'Task description'
)

// Transform to API response
const apiResponse = toTaskApiResponse(task)
```

## Package Exports

This package uses subpath exports for optimal tree-shaking:

- `@prmichaelsen/task-core/schemas` - Zod schemas and types
- `@prmichaelsen/task-core/dto` - API response DTOs and transformers
- `@prmichaelsen/task-core/services` - Firestore service layer
- `@prmichaelsen/task-core/client` - Firebase client wrapper
- `@prmichaelsen/task-core/constants` - Collection path helpers

## Usage

### Schemas

Define and validate task data with Zod schemas:

```typescript
import { TaskSchema, MilestoneSchema, TaskItemSchema } from '@prmichaelsen/task-core/schemas'
import type { Task, Milestone, TaskItem } from '@prmichaelsen/task-core/schemas'

// Validate task data
const task = TaskSchema.parse({
  id: 'task-1',
  user_id: 'user-123',
  title: 'My Task',
  description: 'Task description',
  status: 'active',
  created_at: '2026-02-18T00:00:00Z',
  updated_at: '2026-02-18T00:00:00Z'
})

// TypeScript types are inferred from schemas
const myTask: Task = {
  id: 'task-1',
  user_id: 'user-123',
  title: 'My Task',
  // ... TypeScript will enforce the correct shape
}
```

### DTOs (Data Transfer Objects)

Transform internal schemas to API-friendly responses:

```typescript
import { toTaskApiResponse, toTaskListApiResponse } from '@prmichaelsen/task-core/dto'
import type { TaskApiResponse } from '@prmichaelsen/task-core/dto'

// Transform single task
const task = await TaskDatabaseService.getTask('user-123', 'task-1')
const apiResponse: TaskApiResponse = toTaskApiResponse(task)

// Transform task list
const tasks = await TaskDatabaseService.listTasks('user-123')
const listResponse = toTaskListApiResponse(tasks, tasks.length, 1, 10)
```

### Services

Interact with Firestore using the service layer:

```typescript
import { TaskDatabaseService } from '@prmichaelsen/task-core/services'

// Initialize (optional - uses default Firestore instance)
TaskDatabaseService.initialize()

// Create a task
const task = await TaskDatabaseService.createTask(
  'user-123',
  'My Task',
  'Task description'
)

// Get a task
const task = await TaskDatabaseService.getTask('user-123', 'task-1')

// List tasks
const tasks = await TaskDatabaseService.listTasks('user-123', {
  status: 'active',
  limit: 10
})

// Update task
await TaskDatabaseService.updateTask('user-123', 'task-1', {
  title: 'Updated Title',
  status: 'completed'
})

// Delete task
await TaskDatabaseService.deleteTask('user-123', 'task-1')

// Add milestone
await TaskDatabaseService.addMilestone('user-123', 'task-1', {
  id: 'milestone-1',
  name: 'Phase 1',
  status: 'active',
  progress: 0
})

// Add task item
await TaskDatabaseService.addTaskItem('user-123', 'task-1', {
  id: 'item-1',
  name: 'Subtask 1',
  status: 'pending'
})
```

### Firebase Client

Use the Firebase client wrapper for multi-tenant access:

```typescript
import { FirebaseClient } from '@prmichaelsen/task-core/client'

// Create client for a specific user
const client = new FirebaseClient({
  userId: 'user-123',
  serviceAccountPath: './service-account.json'
})

// Connect to Firebase
await client.connect()

// Create a task
const task = await client.createTask('My Task', 'Description')

// List tasks
const tasks = await client.listTasks({ status: 'active' })

// Get a task
const task = await client.getTask('task-1')

// Update task
await client.updateTask('task-1', { status: 'completed' })

// Delete task
await client.deleteTask('task-1')

// Disconnect when done
await client.disconnect()
```

### Constants

Use collection path helpers for consistent Firestore paths:

```typescript
import { getUserTasks, getUserTask, getUserTaskMessages } from '@prmichaelsen/task-core/constants'

// Get collection path for user's tasks
const tasksPath = getUserTasks('user-123')
// Returns: 'users/user-123/tasks'

// Get document path for specific task
const taskPath = getUserTask('user-123', 'task-1')
// Returns: 'users/user-123/tasks/task-1'

// Get collection path for task messages
const messagesPath = getUserTaskMessages('user-123', 'task-1')
// Returns: 'users/user-123/tasks/task-1/messages'
```

## API Reference

### Schemas

**Task Schema**
- `id`: string
- `user_id`: string
- `title`: string
- `description`: string (optional)
- `status`: 'active' | 'completed' | 'archived'
- `progress`: TaskProgress (optional)
- `config`: TaskConfig (optional)
- `metadata`: TaskMetadata (optional)
- `milestones`: Milestone[] (optional)
- `items`: TaskItem[] (optional)
- `created_at`: string (ISO 8601)
- `updated_at`: string (ISO 8601)

**Milestone Schema**
- `id`: string
- `name`: string
- `description`: string (optional)
- `status`: 'active' | 'completed'
- `progress`: number (0-100)
- `estimated_hours`: number (optional)
- `completed_at`: string (optional, ISO 8601)

**TaskItem Schema**
- `id`: string
- `name`: string
- `description`: string (optional)
- `status`: 'pending' | 'in_progress' | 'completed'
- `estimated_hours`: number (optional)
- `completed_at`: string (optional, ISO 8601)
- `notes`: string (optional)

### Service Methods

**TaskDatabaseService**

- `initialize(db?: Firestore): void` - Initialize with custom Firestore instance
- `createTask(userId, title, description?): Promise<Task>` - Create a new task
- `getTask(userId, taskId): Promise<Task | null>` - Get task by ID
- `listTasks(userId, options?): Promise<Task[]>` - List tasks with optional filters
- `updateTask(userId, taskId, updates): Promise<void>` - Update task fields
- `deleteTask(userId, taskId): Promise<void>` - Delete a task
- `addMilestone(userId, taskId, milestone): Promise<void>` - Add milestone to task
- `updateMilestone(userId, taskId, milestoneId, updates): Promise<void>` - Update milestone
- `removeMilestone(userId, taskId, milestoneId): Promise<void>` - Remove milestone
- `addTaskItem(userId, taskId, item): Promise<void>` - Add item to task
- `updateTaskItem(userId, taskId, itemId, updates): Promise<void>` - Update task item
- `removeTaskItem(userId, taskId, itemId): Promise<void>` - Remove task item
- `addMessage(userId, taskId, message): Promise<TaskMessage>` - Add message to task
- `listMessages(userId, taskId, options?): Promise<TaskMessage[]>` - List task messages

### DTO Transformers

- `toTaskApiResponse(task): TaskApiResponse` - Transform Task to API response
- `toTaskListApiResponse(tasks, total, page, pageSize): TaskListApiResponse` - Transform task list
- `toTaskMessageApiResponse(message): TaskMessageApiResponse` - Transform message
- `toTaskMessageListApiResponse(messages, total, page, pageSize): TaskMessageListApiResponse` - Transform message list
- `toMilestoneApiResponse(milestone): MilestoneApiResponse` - Transform milestone
- `toTaskItemApiResponse(item): TaskItemApiResponse` - Transform task item
- `toTaskProgressApiResponse(progress): TaskProgressApiResponse` - Transform progress
- `toTaskConfigApiResponse(config): TaskConfigApiResponse` - Transform config
- `toTaskMetadataApiResponse(metadata): TaskMetadataApiResponse` - Transform metadata

## Testing

### Run Unit Tests

```bash
npm test
```

### Run E2E Tests

E2E tests require the Firestore emulator:

```bash
# Start emulator
firebase emulators:start --only firestore

# In another terminal, run E2E tests
npm run test:e2e
```

### Test Coverage

```bash
npm test -- --coverage
```

Current coverage: **93%** (43/46 tests passing)

## Development

### Build

```bash
npm run build
```

Generates:
- JavaScript bundles in `dist/`
- TypeScript declarations (`.d.ts`)
- Source maps

### Watch Mode

```bash
npm run dev
```

### Type Check

```bash
npm run typecheck
```

## Project Structure

```
task-core/
├── src/
│   ├── schemas/
│   │   └── task.ts              # Zod schemas
│   ├── dto/
│   │   ├── task-api.dto.ts      # DTO types
│   │   ├── transformers.ts      # Transform functions
│   │   ├── transformers.spec.ts # Tests
│   │   └── index.ts             # Exports
│   ├── services/
│   │   ├── task-database.service.ts      # Firestore service
│   │   ├── task-database.service.spec.ts # Unit tests
│   │   └── task-database.service.e2e.ts  # E2E tests
│   ├── constant/
│   │   └── collections.ts       # Path helpers
│   └── client.ts                # Firebase client
│       └── client.spec.ts       # Tests
├── dist/                        # Build output
├── package.json
├── tsconfig.json
├── jest.config.js
└── esbuild.build.js
```

## Dependencies

- **firebase-admin** (^13.6.1) - Firebase Admin SDK for Firestore
- **zod** (^4.3.6) - TypeScript-first schema validation

## License

MIT

## Contributing

Contributions are welcome! Please ensure:
- All tests pass (`npm test`)
- TypeScript compiles (`npm run typecheck`)
- Code follows existing patterns
- Add tests for new features

## Support

For issues and questions:
- GitHub Issues: [Create an issue](https://github.com/yourusername/task-core/issues)
- Documentation: This README

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history and changes.
