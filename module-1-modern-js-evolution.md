# Module 1: Modern JS Evolution and TS Introduction

## Lesson 1: ES6+ Features Bridging to Your JS Knowledge

Modern JavaScript (ES6+) introduced features that make code more readable, maintainable, and powerful. Drawing from your jQuery and PHP background, these changes reduce boilerplate and improve error handling.

### Key Features and Code Snippets

#### Arrow Functions
Concise syntax and lexical `this` binding.

```javascript
// Old JS
function add(a, b) {
  return a + b;
}

// Modern
const add = (a, b) => a + b;

// With lexical this (useful in event handlers, like jQuery)
const obj = {
  value: 10,
  getValue: function() {
    return () => this.value; // this refers to obj, not the function
  }
};
```

#### Template Literals
String interpolation, like PHP's double quotes with variables.

```javascript
// Old JS
var name = 'John';
console.log('Hello, ' + name + '!');

// Modern
const name = 'John';
console.log(`Hello, ${name}!`);
```

#### Destructuring
Unpack arrays/objects easily.

```javascript
// Arrays
const [a, b] = [1, 2];

// Objects
const { name, age } = { name: 'John', age: 30 };

// Function params
function greet({ name }) {
  console.log(`Hi ${name}`);
}
```

#### Spread/Rest Operators
Spread for expansion, rest for gathering.

```javascript
// Spread
const arr = [1, 2, 3];
const newArr = [...arr, 4, 5];

// Rest
function sum(...numbers) {
  return numbers.reduce((a, b) => a + b, 0);
}
```

#### Modules
Organize code into reusable files.

```javascript
// utils.js
export const calculateTotal = (items) => items.reduce((sum, item) => sum + item.price, 0);

// main.js
import { calculateTotal } from './utils.js';
```

#### Async/Await
Simplify asynchronous code, replacing callback hell.

```javascript
// Old jQuery AJAX
$.ajax({
  url: '/api/user',
  success: function(data) {
    console.log(data);
  },
  error: function(err) {
    console.error(err);
  }
});

// Modern fetch with async/await
async function loadUserData() {
  try {
    const response = await fetch('/api/user');
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error('Failed to load data:', error);
  }
}
```

### Exercises

1. **Refactor jQuery AJAX to fetch with async/await**  
   Original:  
   ```javascript
   $.ajax({
     url: '/api/data',
     method: 'GET',
     success: function(data) {
       console.log(data);
     },
     error: function(err) {
       console.error(err);
     }
   });
   ```  
   Solution:  
   ```javascript
   async function loadData() {
     try {
       const response = await fetch('/api/data');
       const data = await response.json();
       console.log(data);
     } catch (error) {
       console.error(error);
     }
   }
   ```

2. **Use destructuring and template literals**  
   Original:  
   ```javascript
   function displayUser(user) {
     console.log('Name: ' + user.name + ', Age: ' + user.age);
   }
   ```  
   Solution:  
   ```javascript
   function displayUser({ name, age }) {
     console.log(`Name: ${name}, Age: ${age}`);
   }
   ```

### Quiz
1. How do arrow functions differ from regular functions? (Lexical this, concise syntax)
2. What does the spread operator do? (Expands iterables)
3. Why are modules better than global variables? (Avoid naming conflicts, better organization)

## Lesson 2: Introduction to TypeScript

TypeScript extends JavaScript with types, similar to PHP's type declarations, helping catch errors at compile time.

### Code Snippets

```typescript
// Basic types
let name: string = 'John';
let age: number = 30;
let isActive: boolean = true;

// Arrays and objects
let scores: number[] = [10, 20];
let user: { name: string; age: number } = { name: 'John', age: 30 };

// Functions with types
function greet(user: { name: string }): string {
  return `Hello, ${user.name}`;
}

// Optional properties
interface User {
  name: string;
  age?: number;
}

function createUser(name: string, age?: number): User {
  return { name, age };
}
```

### Exercises

1. **Add types to a plain JS function**  
   Original:  
   ```javascript
   function add(a, b) {
     return a + b;
   }
   ```  
   Solution:  
   ```typescript
   function add(a: number, b: number): number {
     return a + b;
   }
   ```

2. **Define an interface and use it**  
   Original:  
   ```javascript
   function displayProduct(product) {
     console.log(product.name + ': $' + product.price);
   }
   ```  
   Solution:  
   ```typescript
   interface Product {
     name: string;
     price: number;
   }

   function displayProduct(product: Product): void {
     console.log(`${product.name}: $${product.price}`);
   }
   ```

### Quiz
1. What are the benefits of TypeScript? (Type safety, better IDE support, error catching)
2. How do interfaces help? (Define object shapes for type checking)