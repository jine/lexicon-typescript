# Module 5 Solutions: Best Practices and Migration

## Migration Strategies Exercises

### Exercise 1: Migrate a JS file to TS

```typescript
function sum(arr: number[]): number {
  return arr.reduce((a, b) => a + b, 0);
}
```

### Exercise 2: Migrate Express.js route handler

```typescript
import express, { Request, Response } from 'express';
const app = express();

interface UserParams {
  id: string;
}

interface UserResponse {
  id: string;
  name: string;
}

app.get('/users/:id', (req: Request<UserParams>, res: Response<UserResponse>) => {
  const userId = req.params.id;
  res.json({ id: userId, name: `User ${userId}` });
});
```

### Exercise 3: Add types to a jQuery function

```typescript
interface JqueryEventHandler {
  (event: JQuery.Event): void;
}

function setupEventHandlers(): void {
  $('#button').click(function(this: HTMLElement, event: JQuery.Event) {
    const value = $('#input').val() as string;
    console.log(value);
  } as JqueryEventHandler);
}
```

## Error Handling Exercises

### Exercise 1: Add error handling to a function

```typescript
function divide(a: number, b: number): number {
  if (b === 0) throw new Error('Division by zero');
  return a / b;
}
```

### Exercise 2: Implement Result type for API calls

```typescript
type ApiResult<T> = { success: true; data: T } | { success: false; error: string };

async function fetchUser(id: number): Promise<ApiResult<User>> {
  try {
    const response = await fetch(`/api/users/${id}`);
    if (!response.ok) {
      return { success: false, error: `HTTP ${response.status}` };
    }
    const data = await response.json();
    return { success: true, data };
  } catch (error) {
    return { success: false, error: error instanceof Error ? error.message : 'Unknown error' };
  }
}
```

### Exercise 3: Create error boundary pattern

```typescript
class ProcessingError extends Error {
  constructor(message: string, public itemIndex: number, public originalError: Error) {
    super(message);
    this.name = 'ProcessingError';
  }
}

function processItems(items: Array<{ value?: string }>): Array<{ result: string } | { error: ProcessingError }> {
  return items.map((item, index) => {
    try {
      if (!item.value) {
        throw new Error('Missing value property');
      }
      return { result: item.value.toUpperCase() };
    } catch (error) {
      return {
        error: new ProcessingError(
          `Failed to process item ${index}`,
          index,
          error instanceof Error ? error : new Error(String(error))
        )
      };
    }
  });
}
```

## Performance and Best Practices Exercises

### Exercise 1: Optimize a function with types

```typescript
interface Item { id: number; name: string; }
function find(items: Item[], id: number): Item | undefined {
  return items.find(item => item.id === id);
}
```

### Exercise 2: Optimize discriminated union for better performance

```typescript
type ApiResponse =
  | { status: 'success'; data: unknown }
  | { status: 'error'; error: string };

function handleResponse(response: ApiResponse) {
  if (response.status === 'success') {
    console.log(response.data); // TypeScript knows data exists
  } else {
    console.error(response.error); // TypeScript knows error exists
  }
}
```

### Exercise 3: Use const assertions for configuration objects

```typescript
const config = {
  theme: 'dark',
  language: 'en',
  features: ['feature1', 'feature2']
} as const;

type Theme = typeof config.theme; // 'dark'
type Language = typeof config.language; // 'en'
type Feature = typeof config.features[number]; // 'feature1' | 'feature2'
```

## Debugging TypeScript and Common Pitfalls Exercises

### Exercise 1: Create a type-safe assertion function

```typescript
interface UserData {
  type: 'user';
  name: string;
  age: number;
}

function isUserData(data: any): data is UserData {
  return data &&
         data.type === 'user' &&
         typeof data.name === 'string' &&
         typeof data.age === 'number';
}

function processData(data: unknown) {
  if (isUserData(data)) {
    console.log(data.name); // TypeScript knows data is UserData
  }
}
```

### Exercise 2: Fix optional property handling

```typescript
interface Options {
  retries?: number;
  timeout?: number;
}

function fetchWithRetry(url: string, options: Options = {}) {
  const retries = (options.retries ?? 3) + 1;
  const timeout = options.timeout ?? 5000;
  // Safe to use
}
```

### Exercise 3: Debug complex union types

```typescript
type Event = { type: 'click'; x: number; y: number } | { type: 'key'; key: string };

function handleEvent(event: Event) {
  switch (event.type) {
    case 'click':
      console.log(`Click at ${event.x}, ${event.y}`);
      break;
    case 'key':
      console.log(`Key pressed: ${event.key}`);
      break;
    default:
      // TypeScript will catch missing cases
      const _exhaustive: never = event;
  }
}
```