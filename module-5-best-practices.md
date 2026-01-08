# Module 5: Best Practices and Migration

## Lesson 1: Migration Strategies

Gradual TS adoption: rename .js to .ts, add types incrementally.

### Tips
- Start with `any` then refine.
- Use `--noImplicitAny` for strictness.

### Code Snippets

```typescript
// Before: loose JS
function process(data) { return data.value; }

// After: typed
function process(data: { value: string }): string {
  return data.value;
}
```

## Lesson 2: Error Handling and Performance

Use try/catch in async functions. Avoid `any` for performance.

### Code Snippets

```typescript
async function fetchData(): Promise<string> {
  try {
    const response = await fetch('/api');
    return await response.text();
  } catch (error) {
    throw new Error('Fetch failed');
  }
}
```

### Exercise
Migrate a JS file to TS.

### Quiz
1. How to handle errors in async TS? (try/catch)