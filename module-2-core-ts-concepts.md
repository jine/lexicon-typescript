# Module 2: Core TypeScript Concepts

<div align="center">
  <strong>Previous Module:</strong> [← Modern JS Evolution and TS Introduction](./module-1-modern-js-evolution.md) |
  <strong>Next Module:</strong> [Setup and Tools →](./module-3-setup-tools.md)
</div>

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
    [View Solution](./solutions/module-2-solutions.md#exercise-1-create-an-interface-for-a-user-object-and-implement-a-function)

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

// Generic constraints - restrict type parameters
interface HasId {
  id: number;
}

function findById<T extends HasId>(items: T[], id: number): T | undefined {
  return items.find(item => item.id === id);
}

// Multiple type parameters
function mergeObjects<T, U>(obj1: T, obj2: U): T & U {
  return { ...obj1, ...obj2 };
}

const merged = mergeObjects({ name: 'John' }, { age: 30 }); // { name: string; age: number }

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
    [View Solution](./solutions/module-2-solutions.md#exercise-1-implement-a-generic-function-for-filtering-arrays)

2. **Create a generic class for a data store**  
     Original:  
     ```typescript
     class DataStore {
       constructor() {
         this.items = [];
       }
       add(item) {
         this.items.push(item);
       }
     }
     ```  
    [View Solution](./solutions/module-2-solutions.md#exercise-2-create-a-generic-class-for-a-data-store)

3. **Implement generic constraints for API responses**  
     Original:  
     ```typescript
     function handleResponse(response) {
       return response.data;
     }
     ```  
    [View Solution](./solutions/module-2-solutions.md#exercise-3-implement-generic-constraints-for-api-responses)

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
type RequiredProduct = Required<Product>; // All properties required
type PickProduct = Pick<Product, 'name' | 'price'>; // Select specific properties
type OmitProduct = Omit<Product, 'category'>; // Exclude specific properties

// Mapped types - transform existing types
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};

type Optional<T> = {
  [P in keyof T]?: T[P];
};

// Conditional types
type IsString<T> = T extends string ? 'yes' : 'no';
type A = IsString<string>; // 'yes'
type B = IsString<number>; // 'no'

// Template literal types
type EventName = 'click' | 'hover' | 'focus';
type EventHandler = `on${Capitalize<EventName>}`; // 'onClick' | 'onHover' | 'onFocus'
```

### Exercises

1. **Use union types for a function parameter**  
    Original:  
    ```typescript
    function logValue(value) {
      console.log(value);
    }
    ```  
    [View Solution](./solutions/module-2-solutions.md#exercise-1-use-union-types-for-a-function-parameter)

2. **Create a mapped type for configuration objects**  
     Original:  
     ```typescript
     interface Config {
       apiUrl: string;
       timeout: number;
       retries: number;
     }
     ```  
    [View Solution](./solutions/module-2-solutions.md#exercise-2-create-a-mapped-type-for-configuration-objects)

3. **Use conditional types for type guards**  
     Original:  
     ```typescript
     function processValue(value) {
       if (typeof value === 'string') {
         return value.toUpperCase();
       } else {
         return value * 2;
       }
     }
     ```  
    [View Solution](./solutions/module-2-solutions.md#exercise-3-use-conditional-types-for-type-guards)

### Quiz
1. What does `Partial<T>` do? (Makes all properties optional)
2. How do generic constraints work? (extends keyword to limit type parameters)
3. What are mapped types? (Types that transform properties of existing types)
4. When to use conditional types? (For type logic based on conditions)

## Lesson 4: Advanced TypeScript Features

Type guards, decorators, and declaration merging for robust type systems.

### Code Snippets

```typescript
// Type guards - narrow types at runtime
function isString(value: unknown): value is string {
  return typeof value === 'string';
}

function processInput(input: string | number): string {
  if (isString(input)) {
    return input.toUpperCase(); // input is string here
  }
  return input.toString();
}

// Using instanceof for class type guards
class Dog {
  bark() { console.log('woof'); }
}

function isDog(pet: Dog | Cat): pet is Dog {
  return pet instanceof Dog;
}

// Decorators - modify classes/methods at design time
function logged(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  const original = descriptor.value;
  descriptor.value = function(...args: any[]) {
    console.log(`Calling ${propertyKey} with`, args);
    return original.apply(this, args);
  };
}

class Calculator {
  @logged
  add(a: number, b: number): number {
    return a + b;
  }
}

// Declaration merging - extend existing types
interface Array<T> {
  first(): T | undefined;
  last(): T | undefined;
}

Array.prototype.first = function() {
  return this[0];
};

Array.prototype.last = function() {
  return this[this.length - 1];
};

const arr = [1, 2, 3];
console.log(arr.first()); // 1
console.log(arr.last()); // 3
```

### Exercises

1. **Create a type guard for union types**  
     Original:  
     ```typescript
     type Shape = Circle | Rectangle;
     function getArea(shape) {
       // Need to check type
     }
     ```  
     [View Solution](./solutions/module-2-solutions.md#exercise-1-create-a-type-guard-for-union-types)

2. **Implement a simple decorator for timing**  
     Original:  
     ```typescript
     class Service {
       slowMethod() {
         // some slow operation
       }
     }
     ```  
    [View Solution](./solutions/module-2-solutions.md#exercise-2-implement-a-simple-decorator-for-timing)

### Quiz
1. What are type guards used for? (Narrow union types at runtime)
2. How do decorators work? (Modify classes/methods at design time)
3. What is declaration merging? (Extending existing type declarations)
4. Why use type guards instead of type assertions? (Runtime safety vs compile-time assumptions)

---

<div align="center">
  <strong>Previous Module:</strong> [← Modern JS Evolution and TS Introduction](./module-1-modern-js-evolution.md) |
  <strong>Next Module:</strong> [Setup and Tools →](./module-3-setup-tools.md)
</div>