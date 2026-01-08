# Module 1 Solutions: Modern JS Evolution and TS Introduction

## ES6+ Features Exercises

## Exercise 1: Refactor jQuery AJAX to fetch with async/await

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

## Exercise 2: Use destructuring and template literals

```javascript
function displayUser({ name, age }) {
  console.log(`Name: ${name}, Age: ${age}`);
}
```

## Exercise 3: Convert prototype-based code to class syntax

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

## Exercise 4: Use Map for caching user data

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

## Exercise 5: Create a generator for ID sequence

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

## Exercise 6: Refactor callback-based code to use Promises

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

## TypeScript Introduction Exercises

## Exercise 1: Add types to a plain JS function

```typescript
function add(a: number, b: number): number {
  return a + b;
}
```

## Exercise 2: Define an interface and use it

```typescript
interface Product {
  name: string;
  price: number;
}

function displayProduct(product: Product): void {
  console.log(`${product.name}: $${product.price}`);
}
```

## Exercise 3: Add union types for flexible function parameters

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

## Exercise 4: Use literal types for status management

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

## Exercise 5: Create a type alias for complex object

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