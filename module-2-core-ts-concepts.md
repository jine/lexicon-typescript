# Module 2: Core TypeScript Concepts

## Lesson 1: Types and Interfaces

Types define data structures. Interfaces describe object contracts, bridging to PHP's interfaces for contracts.

### Code Snippets

```typescript
// Basic types
let isLoggedIn: boolean = true;
let scores: number[] = [10, 20, 30];

// Interfaces (like PHP interfaces for object shapes)
interface Product {
  id: number;
  name: string;
  price: number;
}

function displayProduct(product: Product): void {
  console.log(`${product.name}: $${product.price}`);
}
```

### Exercise
Create an interface for a user object and implement a function using it.

### Quiz
1. What's the difference between type and interface? (Interface for objects, type for unions/primitives)

## Lesson 2: Generics and Enums

Generics enable reusable, type-safe code (like PHP generics). Enums define named constants.

### Code Snippets

```typescript
// Generics (reusable with types)
function reverseArray<T>(arr: T[]): T[] {
  return arr.reverse();
}

// Enums (named constants, like PHP constants)
enum Role {
  Admin = 'admin',
  User = 'user',
  Guest = 'guest'
}

function checkAccess(role: Role): boolean {
  return role === Role.Admin;
}
```

### Exercise
Implement a generic function for filtering arrays.

### Quiz
1. When to use generics? (For type-safe reusable functions)