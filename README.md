# @prmichaelsen/task-core

Core business logic for the task execution system. Provides schemas, DTOs, services, and Firebase client for task management.

## Installation

```bash
npm install @prmichaelsen/task-core
```

## Usage

### Schemas

```typescript
import { TaskSchema, MilestoneSchema } from '@prmichaelsen/task-core/schemas'

const task = TaskSchema.parse(data)
```

### DTOs

```typescript
import { toTaskApiResponse } from '@prmichaelsen/task-core/dto'

const apiResponse = toTaskApiResponse(task)
```

### Services

```typescript
import { TaskDatabaseService } from '@prmichaelsen/task-core/services'

const task = await TaskDatabaseService.createTask(userId, title, description)
```

### Firebase Client

```typescript
import { FirebaseClient } from '@prmichaelsen/task-core/client'

const client = new FirebaseClient({ userId })
await client.connect()
```

## License

MIT
