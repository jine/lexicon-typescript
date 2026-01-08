# Module 4: Hands-on Projects

## Project 1: Typed Functions

Build a calculator with typed functions.

### Code Snippets

```typescript
function add(a: number, b: number): number {
  return a + b;
}

interface Calculator {
  add: (a: number, b: number) => number;
  subtract: (a: number, b: number) => number;
}
```

### Exercise
Implement full calculator.

## Project 2: CLI Tool

A simple CLI for processing data.

### Code Snippets

```typescript
import * as fs from 'fs';

function readFile(path: string): string {
  return fs.readFileSync(path, 'utf8');
}
```

### Exercise
Build a file reader CLI.

## Project 3: Web App with DOM Manipulation

Type-safe DOM ops, bridging to jQuery.

### Code Snippets

```typescript
const button = document.querySelector('#btn') as HTMLButtonElement;
button.addEventListener('click', () => {
  const input = document.querySelector('#input') as HTMLInputElement;
  console.log(input.value);
});
```

### Exercise
Create a todo app with TS DOM.

## Project 4: React Integration Teaser

Basic component with TS.

### Code Snippets

```tsx
interface Props { name: string; }
const Greeting: React.FC<Props> = ({ name }) => <h1>Hello, {name}!</h1>;
```

### Exercise
Build a simple React component.