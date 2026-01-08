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

#### Classes
Object-oriented programming with cleaner syntax than prototype-based inheritance.

```javascript
// Old prototype way
function User(name, age) {
  this.name = name;
  this.age = age;
}
User.prototype.greet = function() {
  return 'Hello, ' + this.name;
};

// Modern class way
class User {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    return `Hello, ${this.name}`;
  }

  get isAdult() {
    return this.age >= 18;
  }

  set age(value) {
    if (value < 0) throw new Error('Age cannot be negative');
    this._age = value;
  }

  get age() {
    return this._age;
  }

  static createAdmin(name) {
    return new User(name, 30); // Simplified for example
  }
}
```

#### Promises
Handle asynchronous operations more elegantly than callbacks.

```javascript
// Creating a promise
const fetchUser = (id) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (id > 0) {
        resolve({ id, name: 'John' });
      } else {
        reject(new Error('Invalid ID'));
      }
    }, 1000);
  });
};

// Using promises
fetchUser(1)
  .then(user => console.log(user))
  .catch(error => console.error(error));

// Promise.all for multiple async operations
Promise.all([
  fetchUser(1),
  fetchUser(2)
]).then(users => console.log(users));
```

#### Map and Set
New data structures for better performance and functionality.

```javascript
// Map - key-value pairs with any type of key
const userMap = new Map();
userMap.set('john', { name: 'John', age: 30 });
userMap.set(123, 'some value'); // number key

console.log(userMap.get('john')); // { name: 'John', age: 30 }
console.log(userMap.has('john')); // true

// Set - unique values only
const uniqueTags = new Set(['js', 'web', 'js']); // duplicates removed
uniqueTags.add('typescript');
console.log(uniqueTags.has('web')); // true
```

#### Generators
Functions that can be paused and resumed, useful for iterators.

```javascript
function* fibonacci() {
  let [prev, curr] = [0, 1];
  while (true) {
    yield curr;
    [prev, curr] = [curr, prev + curr];
  }
}

const fib = fibonacci();
console.log(fib.next().value); // 1
console.log(fib.next().value); // 1
console.log(fib.next().value); // 2
console.log(fib.next().value); // 3
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

3. **Convert prototype-based code to class syntax**  
    Original:  
    ```javascript
    function Product(name, price) {
      this.name = name;
      this.price = price;
    }
    Product.prototype.getDisplayPrice = function() {
      return '$' + this.price;
    };
    ```  
    Solution:  
    ```javascript
    class Product {
      constructor(name, price) {
        this.name = name;
        this.price = price;
      }

      getDisplayPrice() {
        return `$${this.price}`;
      }
    }
    ```

4. **Use Map for caching user data**  
    Original:  
    ```javascript
    var userCache = {};
    function getUser(id) {
      if (userCache[id]) return userCache[id];
      // fetch user...
      userCache[id] = { id, name: 'User' + id };
      return userCache[id];
    }
    ```  
    Solution:  
    ```javascript
    const userCache = new Map();
    function getUser(id) {
      if (userCache.has(id)) return userCache.get(id);
      // fetch user...
      const user = { id, name: `User${id}` };
      userCache.set(id, user);
      return user;
    }
    ```

5. **Create a generator for ID sequence**  
    Original:  
    ```javascript
    var id = 0;
    function getNextId() {
      return ++id;
    }
    ```  
    Solution:  
    ```javascript
    function* idGenerator() {
      let id = 0;
      while (true) {
        yield ++id;
      }
    }
    const getNextId = idGenerator();
    console.log(getNextId.next().value); // 1
    console.log(getNextId.next().value); // 2
    ```

6. **Refactor callback-based code to use Promises**  
    Original:  
    ```javascript
    function loadData(callback) {
      setTimeout(() => {
        callback(null, { data: 'loaded' });
      }, 1000);
    }
    loadData((error, result) => {
      if (error) console.error(error);
      else console.log(result);
    });
    ```  
    Solution:  
    ```javascript
    function loadData() {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve({ data: 'loaded' });
        }, 1000);
      });
    }
    loadData()
      .then(result => console.log(result))
      .catch(error => console.error(error));
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

// Union types - multiple possible types
let id: string | number;
id = 'abc';
id = 123;

// Literal types - specific values
type Status = 'pending' | 'approved' | 'rejected';
let orderStatus: Status = 'pending';

// Type aliases
type Point = {
  x: number;
  y: number;
};

type Callback = (error: Error | null, result?: any) => void;
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

3. **Add union types for flexible function parameters**  
    Original:  
    ```javascript
    function processInput(input) {
      if (typeof input === 'string') {
        return input.toUpperCase();
      } else if (typeof input === 'number') {
        return input * 2;
      }
      return input;
    }
    ```  
    Solution:  
    ```typescript
    function processInput(input: string | number): string | number {
      if (typeof input === 'string') {
        return input.toUpperCase();
      } else if (typeof input === 'number') {
        return input * 2;
      }
      return input;
    }
    ```

4. **Use literal types for status management**  
    Original:  
    ```javascript
    function updateOrder(order, status) {
      // status could be any string
      order.status = status;
    }
    ```  
    Solution:  
    ```typescript
    type OrderStatus = 'pending' | 'processing' | 'shipped' | 'delivered';

    interface Order {
      id: number;
      status: OrderStatus;
    }

    function updateOrder(order: Order, status: OrderStatus): void {
      order.status = status;
    }
    ```

5. **Create a type alias for complex object**  
    Original:  
    ```javascript
    function createPoint(x, y) {
      return { x: x, y: y };
    }
    ```  
    Solution:  
    ```typescript
    type Point = {
      x: number;
      y: number;
    };

    function createPoint(x: number, y: number): Point {
      return { x, y };
    }

    function calculateDistance(p1: Point, p2: Point): number {
      return Math.sqrt(Math.pow(p2.x - p1.x, 2) + Math.pow(p2.y - p1.y, 2));
    }
    ```

### Quiz
1. What are the benefits of TypeScript? (Type safety, better IDE support, error catching at compile time)
2. How do interfaces help? (Define object shapes for type checking, enable code contracts)
3. What is a union type? (A type that can be one of several types)
4. How do literal types improve code? (Restrict values to specific literals for better type safety)
5. Why use type aliases? (Create reusable type definitions, improve readability)
6. What ES6+ features replace callback hell? (Promises and async/await)
7. How do Maps differ from objects? (Any type keys, better performance for frequent additions/removals)
8. What are generators useful for? (Creating iterators, handling large datasets lazily)