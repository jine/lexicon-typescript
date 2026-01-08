# Module 5: Best Practices and Migration

## Lesson 1: Migration Strategies

Migrate from JS to TS incrementally to avoid overwhelming changes.

### Migration Approaches

#### Big Bang Migration
- Convert entire codebase at once
- Requires comprehensive test coverage
- Best for small projects
- Risky for large applications

#### Incremental Migration
- Convert files gradually
- Start with leaf modules (utilities, helpers)
- Allow mixed JS/TS during transition
- Safer for large codebases

#### File-by-File Migration Steps
1. Rename `.js` to `.ts` (no code changes needed)
2. Install `@types/*` packages for dependencies
3. Add `// @ts-ignore` comments for problematic lines
4. Gradually replace `any` with proper types
5. Enable strict mode incrementally
6. Add tests to prevent regressions

### Tips
- Start with utility functions and data models
- Use TypeScript's `--noEmit` flag to check for errors without generating JS
- Leverage existing JSDoc comments as type hints
- Use `tsc --build --dry` to preview compilation results
- Consider using `allowJs: true` in tsconfig during migration

### Code Snippets

```typescript
// Gradual migration
// Step 1: Rename to .ts (implicit any)
// function process(data) { return data.value; }

// Step 2: Add types
function process(data: { value: string }): string {
  return data.value;
}

// Step 3: Use interfaces
interface Data {
  value: string;
  optional?: number;
}

function process(data: Data): string {
  return data.value;
}
```

#### Handling Third-Party Libraries
```typescript
// Before migration - implicit any
const express = require('express');
const app = express();
app.get('/', (req, res) => {
  res.send('Hello');
});

// After migration - with types
import express, { Request, Response } from 'express';
const app = express();

app.get('/', (req: Request, res: Response) => {
  res.send('Hello');
});
```

#### Dealing with Legacy Code
```typescript
// Legacy code with mixed types
function legacyFunction(param) {
  if (typeof param === 'string') {
    return param.toUpperCase();
  }
  return param;
}

// Type-safe wrapper
function typeSafeLegacyFunction(param: string | number): string | number {
  return legacyFunction(param);
}

// Gradual migration
type LegacyParam = string | number | boolean;
function partiallyTypedFunction(param: LegacyParam): any {
  // TODO: Add proper typing
  return legacyFunction(param);
}
```

### Exercises

1. **Migrate a JS file to TS**  
    Original JS:  
    ```javascript
    function sum(arr) {
      return arr.reduce((a, b) => a + b, 0);
    }
    ```  
    [View Solution](./solutions/module-5-solutions.md#exercise-1-migrate-a-js-file-to-ts)

2. **Migrate Express.js route handler**  
     Original JS:  
     ```javascript
     const express = require('express');
     const app = express();

     app.get('/users/:id', function(req, res) {
       const userId = req.params.id;
       res.json({ id: userId, name: 'User ' + userId });
     });
     ```  
    [View Solution](./solutions/module-5-solutions.md#exercise-2-migrate-expressjs-route-handler)

3. **Add types to a jQuery function**  
     Original:  
     ```javascript
     function setupEventHandlers() {
       $('#button').click(function() {
         var value = $('#input').val();
         console.log(value);
       });
     }
     ```  
    [View Solution](./solutions/module-5-solutions.md#exercise-3-add-types-to-a-jquery-function)

## Lesson 2: Error Handling

Proper error handling in TS, especially with async code.

### Code Snippets

```typescript
// Async error handling
async function fetchData(url: string): Promise<string> {
  try {
    const response = await fetch(url);
    if (!response.ok) throw new Error('HTTP error');
    return await response.text();
  } catch (error) {
    console.error('Fetch failed:', error);
    throw error; // Re-throw or handle
  }
}

// Custom error types with additional context
class ValidationError extends Error {
  constructor(message: string, public field?: string, public value?: any) {
    super(message);
    this.name = 'ValidationError';
  }
}

class NetworkError extends Error {
  constructor(message: string, public statusCode?: number, public url?: string) {
    super(message);
    this.name = 'NetworkError';
  }
}

class DatabaseError extends Error {
  constructor(message: string, public code?: string, public query?: string) {
    super(message);
    this.name = 'DatabaseError';
  }
}

// Error handling patterns
function validateUser(user: { name: string; age: number }): void {
  if (!user.name) {
    throw new ValidationError('Name is required', 'name', user.name);
  }
  if (user.age < 0) {
    throw new ValidationError('Age must be positive', 'age', user.age);
  }
  if (user.age > 150) {
    throw new ValidationError('Age seems unrealistic', 'age', user.age);
  }
}

// Result type pattern (similar to Rust's Result)
type Result<T, E = Error> = { success: true; data: T } | { success: false; error: E };

function safeDivide(a: number, b: number): Result<number, Error> {
  if (b === 0) {
    return { success: false, error: new Error('Division by zero') };
  }
  return { success: true, data: a / b };
}

// Usage
const result = safeDivide(10, 0);
if (result.success) {
  console.log('Result:', result.data);
} else {
  console.error('Error:', result.error.message);
}
```

### Exercises

1. **Add error handling to a function**  
    Original:  
    ```typescript
    function divide(a: number, b: number): number {
      return a / b;
    }
    ```  
    [View Solution](./solutions/module-5-solutions.md#exercise-1-add-error-handling-to-a-function)

2. **Implement Result type for API calls**  
     Original:  
     ```typescript
     async function fetchUser(id: number) {
       const response = await fetch(`/api/users/${id}`);
       return response.json();
     }
     ```  
    [View Solution](./solutions/module-5-solutions.md#exercise-2-implement-result-type-for-api-calls)

3. **Create error boundary pattern**  
     Original:  
     ```typescript
     function processItems(items: any[]) {
       return items.map(item => item.value.toUpperCase());
     }
     ```  
    [View Solution](./solutions/module-5-solutions.md#exercise-3-create-error-boundary-pattern)

### Quiz
1. How to handle errors in async TS? (try/catch)

## Lesson 3: Performance and Best Practices

Avoid `any`, use const assertions, optimize types.

### Performance Optimization Techniques

#### Type-Level Performance
- Use `const` assertions for literal types
- Avoid excessive generic instantiation
- Use `keyof` and mapped types efficiently
- Prefer interfaces over type aliases for objects

#### Runtime Performance
- Avoid `any` types which bypass optimization
- Use discriminated unions for better pattern matching
- Minimize object creation in hot paths
- Use `readonly` for immutable data structures

#### Compilation Performance
- Use `skipLibCheck: true` for faster compilation
- Structure imports to avoid deep dependency trees
- Use project references for monorepos
- Configure `incremental: true` for faster rebuilds

### Tips
- Profile compilation with `tsc --diagnostics`
- Use `tsc --noEmit --incremental false` to measure type checking speed
- Monitor bundle size with source maps
- Use TypeScript's performance tracing: `tsc --generateTrace traceDir`

### Code Snippets

```typescript
// Const assertions for literals
const colors = ['red', 'green', 'blue'] as const;
type Color = typeof colors[number]; // 'red' | 'green' | 'blue'

// Performance: avoid any
// Bad
function process(data: any): any { return data; }

// Good
function process<T>(data: T): T { return data; }

// Memory: use readonly for large arrays
const readonlyArray: readonly number[] = [1, 2, 3];

// Discriminated unions for performance
type Shape =
  | { type: 'circle'; radius: number }
  | { type: 'rectangle'; width: number; height: number };

function calculateArea(shape: Shape): number {
  switch (shape.type) {
    case 'circle':
      return Math.PI * shape.radius ** 2;
    case 'rectangle':
      return shape.width * shape.height;
  }
}

// Avoid unnecessary type assertions
// Bad: Type assertion loses type safety
const value = someApi() as string;

// Good: Proper type guards
function isString(value: unknown): value is string {
  return typeof value === 'string';
}

if (isString(someApi())) {
  // value is string here
}

// Optimize generic functions
function identity<T>(value: T): T {
  return value; // Simple generics are zero-cost at runtime
}

// Use const contexts for better inference
function createConfig() {
  return {
    apiUrl: 'https://api.example.com',
    timeout: 5000,
  } as const; // Preserves literal types
}
```

### Exercises

1. **Optimize a function with types**  
    Original:  
    ```typescript
    function find(items: any[], id: any): any {
      return items.find(item => item.id === id);
    }
    ```  
    [View Solution](./solutions/module-5-solutions.md#exercise-1-optimize-a-function-with-types)

2. **Optimize discriminated union for better performance**  
     Original:  
     ```typescript
     interface ApiResponse {
       status: 'success' | 'error';
       data?: any;
       error?: string;
     }

     function handleResponse(response: ApiResponse) {
       if (response.status === 'success') {
         console.log(response.data);
       } else {
         console.error(response.error);
       }
     }
     ```  
    [View Solution](./solutions/module-5-solutions.md#exercise-2-optimize-discriminated-union-for-better-performance)

3. **Use const assertions for configuration objects**  
     Original:  
     ```typescript
     const config = {
       theme: 'dark',
       language: 'en',
       features: ['feature1', 'feature2']
     };

     type Theme = string;
     type Language = string;
     ```  
    [View Solution](./solutions/module-5-solutions.md#exercise-3-use-const-assertions-for-configuration-objects)

### Quiz
1. Why avoid `any`? (Loses type safety, hinders tooling)
2. What does `as const` do? (Creates readonly literal types)
3. Why use discriminated unions? (Better type safety and performance)

## Lesson 4: Debugging TypeScript and Common Pitfalls

Effective debugging techniques and avoiding common TypeScript mistakes.

### Debugging Techniques

#### Type-Level Debugging
```typescript
// Use never type to catch exhaustive checks
type Shape = 'circle' | 'square' | 'triangle';

function getArea(shape: Shape, size: number): number {
  switch (shape) {
    case 'circle': return Math.PI * size * size;
    case 'square': return size * size;
    case 'triangle': return (size * size) / 2;
    default:
      // TypeScript will catch if we miss a case
      const _exhaustiveCheck: never = shape;
      throw new Error(`Unhandled shape: ${shape}`);
  }
}

// Use typeof for debugging complex types
type ComplexType = {
  user: {
    profile: {
      settings: {
        theme: 'light' | 'dark';
      };
    };
  };
};

function debugType<T>(value: T): T {
  console.log('Type info:', typeof value);
  return value;
}

// IntelliSense debugging with hover
const complexValue = {
  nested: {
    deeply: {
      value: 42
    }
  }
};
// Hover over complexValue to see inferred type
```

#### Runtime Debugging
```typescript
// Type guards for debugging
function isObject(value: unknown): value is Record<string, unknown> {
  return value !== null && typeof value === 'object';
}

function debugObject(obj: unknown): void {
  if (isObject(obj)) {
    console.log('Object keys:', Object.keys(obj));
    console.log('Object values:', Object.values(obj));
  } else {
    console.log('Not an object:', typeof obj);
  }
}

// Assertion functions
function assert(condition: boolean, message: string): asserts condition {
  if (!condition) {
    throw new Error(`Assertion failed: ${message}`);
  }
}

function processUser(user: unknown) {
  assert(isObject(user), 'User must be an object');
  assert(typeof user.name === 'string', 'User name must be string');
  // TypeScript now knows user is { name: string; ... }
}
```

### Common Pitfalls and Solutions

#### Pitfall 1: Type Assertion Abuse
```typescript
// Bad: Overusing type assertions
const data = JSON.parse(input) as User; // Unsafe!

// Good: Validate first
function isUser(obj: any): obj is User {
  return obj && typeof obj.name === 'string' && typeof obj.age === 'number';
}

const data = JSON.parse(input);
if (isUser(data)) {
  // Safe to use as User
}
```

#### Pitfall 2: Optional Properties Confusion
```typescript
interface Config {
  timeout?: number;
}

// Bad: Doesn't handle undefined
function createClient(config: Config) {
  const timeout = config.timeout * 1000; // Error if undefined
}

// Good: Proper defaults
function createClient(config: Config) {
  const timeout = (config.timeout ?? 30) * 1000;
}
```

#### Pitfall 3: Generic Constraints Issues
```typescript
// Bad: Too restrictive
function merge<T extends { id: string }>(a: T, b: T): T {
  return { ...a, ...b }; // Type error
}

// Good: Proper constraints
function merge<T extends Record<string, unknown>>(a: T, b: Partial<T>): T {
  return { ...a, ...b };
}
```

#### Pitfall 4: Module Declaration Conflicts
```typescript
// In one file
declare module 'some-library' {
  export function helper(): void;
}

// In another file - conflict!
declare module 'some-library' {
  export const version: string;
}

// Solution: Use namespace merging
declare global {
  namespace SomeLibrary {
    function helper(): void;
    const version: string;
  }
}
```

### Exercises

1. **Create a type-safe assertion function**  
     Original:  
     ```typescript
     function processData(data: any) {
       if (data.type === 'user') {
         console.log(data.name);
       }
     }
     ```  
    [View Solution](./solutions/module-5-solutions.md#exercise-1-create-a-type-safe-assertion-function)

2. **Fix optional property handling**  
     Original:  
     ```typescript
     interface Options {
       retries?: number;
       timeout?: number;
     }

     function fetchWithRetry(url: string, options: Options) {
       const retries = options.retries + 1; // Error if undefined
       // ...
     }
     ```  
    [View Solution](./solutions/module-5-solutions.md#exercise-2-fix-optional-property-handling)

3. **Debug complex union types**  
     Original:  
     ```typescript
     type Event = { type: 'click'; x: number; y: number } | { type: 'key'; key: string };
     function handleEvent(event: Event) {
       // How to access properties safely?
     }
     ```  
    [View Solution](./solutions/module-5-solutions.md#exercise-3-debug-complex-union-types)

### Quiz
1. What is a type guard? (Function that narrows types at runtime)
2. Why avoid type assertions? (Can hide type errors, less safe)
3. How to handle optional properties safely? (Use nullish coalescing ??)
4. What does `never` type help with? (Exhaustive type checking)