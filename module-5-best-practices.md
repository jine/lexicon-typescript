# Module 5: Best Practices and Migration

## Lesson 1: Migration Strategies

Migrate from JS to TS incrementally to avoid overwhelming changes.

### Tips
- Rename .js to .ts
- Add types gradually
- Use `any` temporarily, then refine
- Enable strict mode step-by-step

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

### Exercises

1. **Migrate a JS file to TS**  
   Original JS:  
   ```javascript
   function sum(arr) {
     return arr.reduce((a, b) => a + b, 0);
   }
   ```  
   Solution:  
   ```typescript
   function sum(arr: number[]): number {
     return arr.reduce((a, b) => a + b, 0);
   }
   ```

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

// Custom error types
class ValidationError extends Error {
  constructor(message: string) {
    super(message);
    this.name = 'ValidationError';
  }
}

function validateUser(user: { name: string; age: number }): void {
  if (!user.name) throw new ValidationError('Name required');
  if (user.age < 0) throw new ValidationError('Invalid age');
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
   Solution:  
   ```typescript
   function divide(a: number, b: number): number {
     if (b === 0) throw new Error('Division by zero');
     return a / b;
   }
   ```

### Quiz
1. How to handle errors in async TS? (try/catch)

## Lesson 3: Performance and Best Practices

Avoid `any`, use const assertions, optimize types.

### Tips
- Use `const` for immutable types
- Avoid unnecessary generics
- Use strict mode
- Profile with tsc --noEmit

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
```

### Exercises

1. **Optimize a function with types**  
   Original:  
   ```typescript
   function find(items: any[], id: any): any {
     return items.find(item => item.id === id);
   }
   ```  
   Solution:  
   ```typescript
   interface Item { id: number; name: string; }
   function find(items: Item[], id: number): Item | undefined {
     return items.find(item => item.id === id);
   }
   ```

### Quiz
1. Why avoid `any`? (Loses type safety, hinders tooling)