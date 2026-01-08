# Module 4: Hands-on Projects

## Project 1: Typed Functions - Calculator

Build a command-line calculator with typed functions.

### Steps

1. Create calculator.ts
2. Define operations with types
3. Handle user input

### Code Snippets

```typescript
// calculator.ts
type Operation = 'add' | 'subtract' | 'multiply' | 'divide';

function calculate(op: Operation, a: number, b: number): number {
  switch (op) {
    case 'add': return a + b;
    case 'subtract': return a - b;
    case 'multiply': return a * b;
    case 'divide': return b !== 0 ? a / b : NaN;
    default: throw new Error('Invalid operation');
  }
}

interface Calculator {
  calculate: (op: Operation, a: number, b: number) => number;
}

const calc: Calculator = { calculate };
```

### Exercise
Implement full calculator with input parsing.

Example:
```typescript
// Use readline for input
import * as readline from 'readline';

const rl = readline.createInterface({ input: process.stdin, output: process.stdout });

rl.question('Enter op a b: ', (input) => {
  const [op, a, b] = input.split(' ');
  const result = calculate(op as Operation, +a, +b);
  console.log(result);
  rl.close();
});
```

## Project 2: CLI Tool - File Processor

A CLI tool to read, process, and write files.

### Steps

1. Use fs and path modules
2. Add type-safe file operations

### Code Snippets

```typescript
import * as fs from 'fs';
import * as path from 'path';

interface FileData {
  path: string;
  content: string;
}

function readFile(filePath: string): FileData {
  const content = fs.readFileSync(filePath, 'utf8');
  return { path: filePath, content };
}

function writeFile(filePath: string, content: string): void {
  fs.writeFileSync(filePath, content);
}

function processFile(inputPath: string, outputPath: string, transform: (content: string) => string): void {
  const data = readFile(inputPath);
  const newContent = transform(data.content);
  writeFile(outputPath, newContent);
}
```

### Exercise
Build a tool to uppercase text files.

Example:
```typescript
processFile('input.txt', 'output.txt', content => content.toUpperCase());
```

## Project 3: Web App with DOM Manipulation - Todo List

A type-safe todo app using DOM APIs.

### Steps

1. Create index.html and app.ts
2. Use TS for DOM elements
3. Handle events

### Code Snippets

```typescript
// app.ts
interface Todo {
  id: number;
  text: string;
  completed: boolean;
}

class TodoApp {
  private todos: Todo[] = [];
  private nextId = 1;

  addTodo(text: string): void {
    this.todos.push({ id: this.nextId++, text, completed: false });
    this.render();
  }

  toggleTodo(id: number): void {
    const todo = this.todos.find(t => t.id === id);
    if (todo) {
      todo.completed = !todo.completed;
      this.render();
    }
  }

  render(): void {
    const list = document.getElementById('todo-list') as HTMLUListElement;
    list.innerHTML = '';
    this.todos.forEach(todo => {
      const li = document.createElement('li');
      li.textContent = todo.text;
      if (todo.completed) li.classList.add('completed');
      li.addEventListener('click', () => this.toggleTodo(todo.id));
      list.appendChild(li);
    });
  }
}

const app = new TodoApp();
const input = document.getElementById('todo-input') as HTMLInputElement;
const button = document.getElementById('add-btn') as HTMLButtonElement;

button.addEventListener('click', () => {
  if (input.value) {
    app.addTodo(input.value);
    input.value = '';
  }
});
```

### Exercise
Add delete functionality.

Example:
```typescript
// Add delete button to each li
const deleteBtn = document.createElement('button');
deleteBtn.textContent = 'Delete';
deleteBtn.addEventListener('click', (e) => {
  e.stopPropagation();
  this.todos = this.todos.filter(t => t.id !== id);
  this.render();
});
li.appendChild(deleteBtn);
```

## Project 4: React Integration Teaser - Counter Component

Basic React component with TS, preparing for React modules.

### Steps

1. Set up React with TS
2. Create functional component

### Code Snippets

```tsx
// Counter.tsx
import React, { useState } from 'react';

interface CounterProps {
  initialCount?: number;
}

const Counter: React.FC<CounterProps> = ({ initialCount = 0 }) => {
  const [count, setCount] = useState<number>(initialCount);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setCount(count - 1)}>Decrement</button>
    </div>
  );
};

export default Counter;
```

### Exercise
Add reset button.

Example:
```tsx
<button onClick={() => setCount(initialCount)}>Reset</button>
```