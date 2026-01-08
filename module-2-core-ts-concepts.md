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
   Solution:  
   ```typescript
   function filterArray<T>(arr: T[], predicate: (item: T) => boolean): T[] {
     return arr.filter(predicate);
   }

    const nums = filterArray([1, 2, 3, 4], n => n > 2); // [3, 4]
    ```

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
    Solution:  
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

3. **Implement generic constraints for API responses**  
    Original:  
    ```typescript
    function handleResponse(response) {
      return response.data;
    }
    ```  
    Solution:  
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
   Solution:  
   ```typescript
    function logValue(value: string | number): void {
      console.log(value);
    }
    ```

2. **Create a mapped type for configuration objects**  
    Original:  
    ```typescript
    interface Config {
      apiUrl: string;
      timeout: number;
      retries: number;
    }
    ```  
    Solution:  
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
    Solution:  
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
    Solution:  
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

2. **Implement a simple decorator for timing**  
    Original:  
    ```typescript
    class Service {
      slowMethod() {
        // some slow operation
      }
    }
    ```  
    Solution:  
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

### Quiz
1. What are type guards used for? (Narrow union types at runtime)
2. How do decorators work? (Modify classes/methods at design time)
3. What is declaration merging? (Extending existing type declarations)
4. Why use type guards instead of type assertions? (Runtime safety vs compile-time assumptions)