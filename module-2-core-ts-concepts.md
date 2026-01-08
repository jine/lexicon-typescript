# Module 2: Core TypeScript Concepts

## Lesson 1: Types and Interfaces

Types define data structures, from primitives to complex objects. Interfaces describe contracts for objects, similar to PHP interfaces, ensuring consistency.

### Code Snippets

```typescript
// Basic types
let isLoggedIn: boolean = true;
let scores: number[] = [10, 20, 30];
let user: { name: string; age: number } = { name: 'John', age: 30 };

// Interfaces (extensible object contracts)
interface Product {
  id: number;
  name: string;
  price: number;
  category?: string; // Optional property
}

function displayProduct(product: Product): void {
  console.log(`${product.name}: $${product.price}`);
}

// Extending interfaces
interface PremiumProduct extends Product {
  discount: number;
}

const premium: PremiumProduct = { id: 1, name: 'Laptop', price: 1000, discount: 10 };
```

### Exercises

1. **Create an interface for a user object and implement a function**  
   Original:  
   ```typescript
   function greet(user) {
     console.log('Hi ' + user.name);
   }
   ```  
   Solution:  
   ```typescript
   interface User {
     name: string;
     age?: number;
   }

   function greet(user: User): void {
     console.log(`Hi ${user.name}`);
   }
   ```

### Quiz
1. What's the difference between type and interface? (Interface for objects, type for unions/primitives)
2. Can interfaces be extended? (Yes, like classes)

## Lesson 2: Generics and Enums

Generics provide reusable, type-safe code, akin to PHP generics. Enums define named constants for better readability.

### Code Snippets

```typescript
// Generics (type parameters)
function reverseArray<T>(arr: T[]): T[] {
  return arr.reverse();
}

const nums = reverseArray([1, 2, 3]); // T = number
const strs = reverseArray(['a', 'b']); // T = string

// Generic interfaces
interface Container<T> {
  value: T;
  getValue(): T;
}

const stringContainer: Container<string> = {
  value: 'hello',
  getValue: () => 'hello'
};

// Enums (string or numeric)
enum Role {
  Admin = 'admin',
  User = 'user',
  Guest = 'guest'
}

enum Status {
  Pending, // 0
  Approved, // 1
  Rejected // 2
}

function checkAccess(role: Role): boolean {
  return role === Role.Admin;
}
```

### Exercises

1. **Implement a generic function for filtering arrays**  
   Original:  
   ```typescript
   function filterNumbers(arr, predicate) {
     return arr.filter(predicate);
   }
   ```  
   Solution:  
   ```typescript
   function filterArray<T>(arr: T[], predicate: (item: T) => boolean): T[] {
     return arr.filter(predicate);
   }

   const nums = filterArray([1, 2, 3, 4], n => n > 2); // [3, 4]
   ```

### Quiz
1. When to use generics? (For type-safe reusable functions)
2. What's the default value for numeric enums? (Starts at 0)

## Lesson 3: Advanced Types

Unions, intersections, and utility types for flexible typing.

### Code Snippets

```typescript
// Union types (or)
type Status = 'pending' | 'approved' | 'rejected';

function updateStatus(status: Status): void {
  // ...
}

// Intersection types (and)
type Name = { first: string };
type Age = { age: number };
type Person = Name & Age; // { first: string; age: number }

// Utility types
type PartialProduct = Partial<Product>; // All properties optional
type ReadonlyProduct = Readonly<Product>; // All properties readonly
```

### Exercises

1. **Use union types for a function parameter**  
   Original:  
   ```typescript
   function logValue(value) {
     console.log(value);
   }
   ```  
   Solution:  
   ```typescript
   function logValue(value: string | number): void {
     console.log(value);
   }
   ```

### Quiz
1. What does `Partial<T>` do? (Makes all properties optional)