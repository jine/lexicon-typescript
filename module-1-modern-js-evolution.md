# Module 1: Modern JS Evolution and TS Introduction

## Lesson 1: ES6+ Features Bridging to Your JS Knowledge

Modern JavaScript (ES6+) evolved significantly. Key features include arrow functions for concise syntax, modules for better organization, and async/await for cleaner asynchronous code—bridging from your jQuery/PHP background where callbacks were common.

### Code Snippets

```javascript
// Old JS (what you might remember)
function calculateTotal(items) {
  return items.reduce(function(sum, item) { return sum + item.price; }, 0);
}

// Modern arrow functions
const calculateTotal = (items) => items.reduce((sum, item) => sum + item.price, 0);

// Modules (vs. global scripts)
import { calculateTotal } from './utils.js';
export const calculateTotal = (items) => { /* ... */ };

// Async/await (vs. jQuery.ajax callbacks)
async function loadUserData() {
  try {
    const response = await fetch('/api/user');
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Failed to load data:', error);
  }
}
```

### Exercise
Refactor a simple jQuery AJAX call to use fetch with async/await.

### Quiz
1. What is the main benefit of arrow functions over function expressions? (Conciseness and lexical this)
2. How do ES6 modules improve over global variables? (Encapsulation and reusability)

## Lesson 2: Introduction to TypeScript

TypeScript adds static typing to JavaScript, catching errors early—similar to PHP's type hints. It compiles to JS, so it runs everywhere.

### Code Snippets

```typescript
// Basic types
let name: string = 'John';
let age: number = 30;

// Functions with types
function greet(user: { name: string }): string {
  return `Hello, ${user.name}`;
}
```

### Exercise
Add types to a plain JS function.

### Quiz
1. Why use TypeScript over plain JS? (Type safety and better tooling)