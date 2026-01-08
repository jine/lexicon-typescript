# Module 2 Solutions: Core TypeScript Concepts

## Types and Interfaces Exercises

### Exercise 1: Create an interface for a user object and implement a function

```typescript
interface User {
  name: string;
  age?: number;
}

function greet(user: User): void {
  console.log(`Hi ${user.name}`);
}
```

## Generics and Enums Exercises

### Exercise 1: Implement a generic function for filtering arrays

```typescript
function filterArray<T>(arr: T[], predicate: (item: T) => boolean): T[] {
  return arr.filter(predicate);
}

const nums = filterArray([1, 2, 3, 4], n => n > 2); // [3, 4]
```

### Exercise 2: Create a generic class for a data store

```typescript
class DataStore<T> {
  private items: T[] = [];

  add(item: T): void {
    this.items.push(item);
  }

  getAll(): T[] {
    return [...this.items];
  }

  find(predicate: (item: T) => boolean): T | undefined {
    return this.items.find(predicate);
  }
}

const userStore = new DataStore<{ id: number; name: string }>();
userStore.add({ id: 1, name: 'John' });
```

### Exercise 3: Implement generic constraints for API responses

```typescript
interface ApiResponse<T> {
  data: T;
  status: number;
  message?: string;
}

function handleResponse<T>(response: ApiResponse<T>): T {
  if (response.status !== 200) {
    throw new Error(response.message || 'API Error');
  }
  return response.data;
}

const userResponse: ApiResponse<{ id: number; name: string }> = {
  data: { id: 1, name: 'John' },
  status: 200
};
```

## Advanced Types Exercises

### Exercise 1: Use union types for a function parameter

```typescript
function logValue(value: string | number): void {
  console.log(value);
}
```

### Exercise 2: Create a mapped type for configuration objects

```typescript
interface Config {
  apiUrl: string;
  timeout: number;
  retries: number;
}

type ConfigKeys = keyof Config; // 'apiUrl' | 'timeout' | 'retries'
type OptionalConfig = Partial<Config>;
type RequiredConfig = Required<OptionalConfig>;

// Custom mapped type for environment variables
type EnvConfig = {
  [K in keyof Config as `APP_${Uppercase<string & K>}`]: string;
};
// Results in: { APP_APIURL: string; APP_TIMEOUT: string; APP_RETRIES: string }
```

### Exercise 3: Use conditional types for type guards

```typescript
type ProcessResult<T> = T extends string ? string : number;

function processValue<T extends string | number>(value: T): ProcessResult<T> {
  if (typeof value === 'string') {
    return value.toUpperCase() as ProcessResult<T>;
  } else {
    return (value * 2) as ProcessResult<T>;
  }
}

const result1 = processValue('hello'); // string
const result2 = processValue(5); // number
```

## Advanced TypeScript Features Exercises

### Exercise 1: Create a type guard for union types

```typescript
interface Circle {
  type: 'circle';
  radius: number;
}

interface Rectangle {
  type: 'rectangle';
  width: number;
  height: number;
}

type Shape = Circle | Rectangle;

function isCircle(shape: Shape): shape is Circle {
  return shape.type === 'circle';
}

function getArea(shape: Shape): number {
  if (isCircle(shape)) {
    return Math.PI * shape.radius ** 2;
  } else {
    return shape.width * shape.height;
  }
}
```

### Exercise 2: Implement a simple decorator for timing

```typescript
function timing(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  const original = descriptor.value;
  descriptor.value = function(...args: any[]) {
    const start = Date.now();
    const result = original.apply(this, args);
    const end = Date.now();
    console.log(`${propertyKey} took ${end - start}ms`);
    return result;
  };
}

class Service {
  @timing
  slowMethod(): void {
    // simulate slow operation
    for (let i = 0; i < 1000000; i++) {}
  }
}
```