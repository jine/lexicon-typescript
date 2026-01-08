# Module 4: Hands-on Projects

<div align="center">
  <strong>Previous Module:</strong> [← Setup and Tools](./module-3-setup-tools.md) |
  <strong>Next Module:</strong> [Best Practices and Migration →](./module-5-best-practices.md)
</div>

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

Solution:
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

Solution:
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

Solution:
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

## Project 4: API Client Library

Build a type-safe HTTP client for REST APIs.

### Steps

1. Define API types and interfaces
2. Implement HTTP methods with generics
3. Add error handling and response types
4. Create client class with configuration

### Code Snippets

```typescript
// types/api.ts
export interface ApiResponse<T> {
  data: T;
  status: number;
  message?: string;
}

export interface ApiConfig {
  baseURL: string;
  timeout?: number;
  headers?: Record<string, string>;
}

export class ApiError extends Error {
  constructor(public status: number, message: string) {
    super(message);
    this.name = 'ApiError';
  }
}

// client.ts
export class ApiClient {
  constructor(private config: ApiConfig) {}

  private async request<T>(
    method: 'GET' | 'POST' | 'PUT' | 'DELETE',
    endpoint: string,
    data?: any
  ): Promise<ApiResponse<T>> {
    const url = `${this.config.baseURL}${endpoint}`;
    const headers = {
      'Content-Type': 'application/json',
      ...this.config.headers
    };

    const response = await fetch(url, {
      method,
      headers,
      body: data ? JSON.stringify(data) : undefined,
      signal: this.config.timeout ? AbortSignal.timeout(this.config.timeout) : undefined
    });

    if (!response.ok) {
      throw new ApiError(response.status, `HTTP ${response.status}`);
    }

    const result = await response.json();
    return {
      data: result,
      status: response.status
    };
  }

  async get<T>(endpoint: string): Promise<ApiResponse<T>> {
    return this.request<T>('GET', endpoint);
  }

  async post<T>(endpoint: string, data: any): Promise<ApiResponse<T>> {
    return this.request<T>('POST', endpoint, data);
  }

  async put<T>(endpoint: string, data: any): Promise<ApiResponse<T>> {
    return this.request<T>('PUT', endpoint, data);
  }

  async delete<T>(endpoint: string): Promise<ApiResponse<T>> {
    return this.request<T>('DELETE', endpoint);
  }
}

// Usage example
interface User {
  id: number;
  name: string;
  email: string;
}

const client = new ApiClient({
  baseURL: 'https://jsonplaceholder.typicode.com',
  timeout: 5000
});

// Type-safe API calls
async function fetchUsers() {
  try {
    const response = await client.get<User[]>('/users');
    console.log(response.data);
  } catch (error) {
    if (error instanceof ApiError) {
      console.error(`API Error ${error.status}: ${error.message}`);
    }
  }
}
```

### Exercise
Implement a generic CRUD interface and extend the client.

Solution:
```typescript
interface CrudApi<T> {
  getAll(): Promise<ApiResponse<T[]>>;
  getById(id: number): Promise<ApiResponse<T>>;
  create(data: Omit<T, 'id'>): Promise<ApiResponse<T>>;
  update(id: number, data: Partial<T>): Promise<ApiResponse<T>>;
  delete(id: number): Promise<ApiResponse<void>>;
}

class UserApi implements CrudApi<User> {
  constructor(private client: ApiClient) {}

  getAll() { return this.client.get<User[]>('/users'); }
  getById(id: number) { return this.client.get<User>(`/users/${id}`); }
  create(data: Omit<User, 'id'>) { return this.client.post<User>('/users', data); }
  update(id: number, data: Partial<User>) { return this.client.put<User>(`/users/${id}`, data); }
  delete(id: number) { return this.client.delete(`/users/${id}`); }
}
```

## Project 5: File System Utilities

Create a robust file system utility library with type safety.

### Steps

1. Define file and directory types
2. Implement async file operations
3. Add path utilities and validation
4. Create batch operations

### Code Snippets

```typescript
import * as fs from 'fs/promises';
import * as path from 'path';

export interface FileInfo {
  name: string;
  path: string;
  size: number;
  isDirectory: boolean;
  modified: Date;
}

export class FileSystemUtils {
  static async readFile(filePath: string): Promise<string> {
    try {
      return await fs.readFile(filePath, 'utf-8');
    } catch (error) {
      throw new Error(`Failed to read file ${filePath}: ${error.message}`);
    }
  }

  static async writeFile(filePath: string, content: string): Promise<void> {
    try {
      await fs.mkdir(path.dirname(filePath), { recursive: true });
      await fs.writeFile(filePath, content, 'utf-8');
    } catch (error) {
      throw new Error(`Failed to write file ${filePath}: ${error.message}`);
    }
  }

  static async readJson<T>(filePath: string): Promise<T> {
    const content = await this.readFile(filePath);
    try {
      return JSON.parse(content);
    } catch (error) {
      throw new Error(`Invalid JSON in ${filePath}: ${error.message}`);
    }
  }

  static async writeJson<T>(filePath: string, data: T): Promise<void> {
    const content = JSON.stringify(data, null, 2);
    await this.writeFile(filePath, content);
  }

  static async getFileInfo(filePath: string): Promise<FileInfo> {
    const stats = await fs.stat(filePath);
    return {
      name: path.basename(filePath),
      path: filePath,
      size: stats.size,
      isDirectory: stats.isDirectory(),
      modified: stats.mtime
    };
  }

  static async listDirectory(dirPath: string): Promise<FileInfo[]> {
    const entries = await fs.readdir(dirPath);
    const fileInfos: FileInfo[] = [];

    for (const entry of entries) {
      const fullPath = path.join(dirPath, entry);
      try {
        const info = await this.getFileInfo(fullPath);
        fileInfos.push(info);
      } catch (error) {
        // Skip inaccessible files
        console.warn(`Skipping ${fullPath}: ${error.message}`);
      }
    }

    return fileInfos;
  }

  static async copyFile(source: string, destination: string): Promise<void> {
    await fs.mkdir(path.dirname(destination), { recursive: true });
    await fs.copyFile(source, destination);
  }

  static async deleteFile(filePath: string): Promise<void> {
    await fs.unlink(filePath);
  }

  static async findFiles(
    dirPath: string,
    predicate: (fileInfo: FileInfo) => boolean
  ): Promise<FileInfo[]> {
    const allFiles = await this.listDirectoryRecursive(dirPath);
    return allFiles.filter(predicate);
  }

  private static async listDirectoryRecursive(dirPath: string): Promise<FileInfo[]> {
    const files: FileInfo[] = [];
    const entries = await this.listDirectory(dirPath);

    for (const entry of entries) {
      files.push(entry);
      if (entry.isDirectory) {
        const subFiles = await this.listDirectoryRecursive(entry.path);
        files.push(...subFiles);
      }
    }

    return files;
  }
}
```

### Exercise
Create a file backup utility using the FileSystemUtils.

Solution:
```typescript
export class BackupUtils {
  static async createBackup(sourceDir: string, backupDir: string): Promise<void> {
    const timestamp = new Date().toISOString().replace(/[:.]/g, '-');
    const backupPath = path.join(backupDir, `backup-${timestamp}`);

    const files = await FileSystemUtils.listDirectoryRecursive(sourceDir);

    for (const file of files) {
      if (!file.isDirectory) {
        const relativePath = path.relative(sourceDir, file.path);
        const backupFilePath = path.join(backupPath, relativePath);
        await FileSystemUtils.copyFile(file.path, backupFilePath);
      }
    }

    console.log(`Backup created at ${backupPath}`);
  }
}
```

## Project 6: Configuration Manager

Build a type-safe configuration system with validation and environment support.

### Steps

1. Define configuration schema with types
2. Implement validation and parsing
3. Add environment variable support
4. Create configuration loader

### Code Snippets

```typescript
export interface DatabaseConfig {
  host: string;
  port: number;
  username: string;
  password: string;
  database: string;
}

export interface ServerConfig {
  port: number;
  host: string;
  cors: {
    enabled: boolean;
    origins: string[];
  };
}

export interface AppConfig {
  name: string;
  version: string;
  environment: 'development' | 'production' | 'test';
  database: DatabaseConfig;
  server: ServerConfig;
  logging: {
    level: 'debug' | 'info' | 'warn' | 'error';
    file: string;
  };
}

export class ConfigValidationError extends Error {
  constructor(public field: string, message: string) {
    super(`Configuration error in ${field}: ${message}`);
    this.name = 'ConfigValidationError';
  }
}

export class ConfigManager {
  private static instance: ConfigManager;
  private config: AppConfig | null = null;

  static getInstance(): ConfigManager {
    if (!ConfigManager.instance) {
      ConfigManager.instance = new ConfigManager();
    }
    return ConfigManager.instance;
  }

  async load(configPath?: string): Promise<AppConfig> {
    if (this.config) return this.config;

    const configFile = configPath || process.env.CONFIG_FILE || './config.json';

    try {
      const configData = await import('fs/promises').then(fs => fs.readFile(configFile, 'utf-8'));
      const parsedConfig = JSON.parse(configData);
      this.config = this.mergeWithEnvironment(parsedConfig);
      this.validateConfig(this.config);
      return this.config;
    } catch (error) {
      throw new Error(`Failed to load configuration: ${error.message}`);
    }
  }

  private mergeWithEnvironment(config: Partial<AppConfig>): AppConfig {
    return {
      name: process.env.APP_NAME || config.name || 'MyApp',
      version: process.env.APP_VERSION || config.version || '1.0.0',
      environment: (process.env.NODE_ENV as AppConfig['environment']) || config.environment || 'development',
      database: {
        host: process.env.DB_HOST || config.database?.host || 'localhost',
        port: parseInt(process.env.DB_PORT || '') || config.database?.port || 5432,
        username: process.env.DB_USERNAME || config.database?.username || 'user',
        password: process.env.DB_PASSWORD || config.database?.password || 'password',
        database: process.env.DB_NAME || config.database?.database || 'mydb'
      },
      server: {
        port: parseInt(process.env.PORT || '') || config.server?.port || 3000,
        host: process.env.HOST || config.server?.host || 'localhost',
        cors: {
          enabled: process.env.CORS_ENABLED === 'true' || config.server?.cors?.enabled || false,
          origins: (process.env.CORS_ORIGINS?.split(',') || config.server?.cors?.origins || ['*'])
        }
      },
      logging: {
        level: (process.env.LOG_LEVEL as AppConfig['logging']['level']) || config.logging?.level || 'info',
        file: process.env.LOG_FILE || config.logging?.file || './logs/app.log'
      }
    };
  }

  private validateConfig(config: AppConfig): void {
    if (!config.name) throw new ConfigValidationError('name', 'App name is required');
    if (config.server.port < 1 || config.server.port > 65535) {
      throw new ConfigValidationError('server.port', 'Port must be between 1 and 65535');
    }
    if (!['development', 'production', 'test'].includes(config.environment)) {
      throw new ConfigValidationError('environment', 'Invalid environment value');
    }
  }

  getConfig(): AppConfig {
    if (!this.config) {
      throw new Error('Configuration not loaded. Call load() first.');
    }
    return this.config;
  }
}

// Usage
async function initializeApp() {
  const configManager = ConfigManager.getInstance();
  const config = await configManager.load();

  console.log(`Starting ${config.name} v${config.version} in ${config.environment} mode`);
  console.log(`Server will listen on ${config.server.host}:${config.server.port}`);
}
```

### Exercise
Add configuration hot-reloading capability.

Solution:
```typescript
import * as fs from 'fs';

export class HotReloadConfigManager extends ConfigManager {
  private watcher: fs.FSWatcher | null = null;

  async load(configPath?: string): Promise<AppConfig> {
    const config = await super.load(configPath);

    if (this.watcher) {
      this.watcher.close();
    }

    const configFile = configPath || process.env.CONFIG_FILE || './config.json';
    this.watcher = fs.watch(configFile, async () => {
      console.log('Configuration file changed, reloading...');
      this.config = null; // Force reload on next getConfig call
    });

    return config;
  }

  destroy(): void {
    if (this.watcher) {
      this.watcher.close();
      this.watcher = null;
    }
  }
}
```

## Project 7: Event System

Create a type-safe event system for decoupling application components.

### Steps

1. Define event types and payloads
2. Implement event emitter with generics
3. Add async event handling
4. Create event middleware

### Code Snippets

```typescript
export interface EventPayloadMap {
  'user:created': { userId: number; email: string };
  'user:deleted': { userId: number };
  'order:placed': { orderId: string; amount: number; userId: number };
  'notification:sent': { type: string; recipient: string; message: string };
}

export type EventName = keyof EventPayloadMap;
export type EventPayload<T extends EventName> = EventPayloadMap[T];

export interface EventHandler<T extends EventName> {
  (payload: EventPayload<T>): void | Promise<void>;
}

export class EventEmitter {
  private handlers = new Map<EventName, Set<EventHandler<any>>>();

  on<T extends EventName>(event: T, handler: EventHandler<T>): void {
    if (!this.handlers.has(event)) {
      this.handlers.set(event, new Set());
    }
    this.handlers.get(event)!.add(handler);
  }

  off<T extends EventName>(event: T, handler: EventHandler<T>): void {
    const eventHandlers = this.handlers.get(event);
    if (eventHandlers) {
      eventHandlers.delete(handler);
    }
  }

  async emit<T extends EventName>(event: T, payload: EventPayload<T>): Promise<void> {
    const eventHandlers = this.handlers.get(event);
    if (!eventHandlers) return;

    const promises: Promise<void>[] = [];

    for (const handler of eventHandlers) {
      try {
        const result = handler(payload);
        if (result instanceof Promise) {
          promises.push(result);
        }
      } catch (error) {
        console.error(`Error in event handler for ${event}:`, error);
      }
    }

    await Promise.all(promises);
  }

  removeAllListeners(event?: EventName): void {
    if (event) {
      this.handlers.delete(event);
    } else {
      this.handlers.clear();
    }
  }

  listenerCount(event: EventName): number {
    return this.handlers.get(event)?.size || 0;
  }
}

// Singleton instance
export const eventEmitter = new EventEmitter();

// Usage example
// Register event listeners
eventEmitter.on('user:created', async (payload) => {
  console.log(`New user created: ${payload.email}`);
  // Send welcome email
});

eventEmitter.on('order:placed', (payload) => {
  console.log(`Order ${payload.orderId} placed for $${payload.amount}`);
  // Update inventory, send notifications, etc.
});

// Emit events
async function createUser(email: string) {
  // Create user in database
  const userId = Math.random();

  // Emit event
  await eventEmitter.emit('user:created', { userId, email });

  return userId;
}
```

### Exercise
Add event middleware for logging and error handling.

Solution:
```typescript
export interface EventMiddleware {
  <T extends EventName>(event: T, payload: EventPayload<T>, next: () => Promise<void>): Promise<void>;
}

export class MiddlewareEventEmitter extends EventEmitter {
  private middlewares: EventMiddleware[] = [];

  use(middleware: EventMiddleware): void {
    this.middlewares.push(middleware);
  }

  async emit<T extends EventName>(event: T, payload: EventPayload<T>): Promise<void> {
    let index = -1;

    const next = async (): Promise<void> => {
      index++;
      if (index < this.middlewares.length) {
        await this.middlewares[index](event, payload, next);
      } else {
        await super.emit(event, payload);
      }
    };

    await next();
  }
}

// Usage
const emitter = new MiddlewareEventEmitter();

// Logging middleware
emitter.use(async (event, payload, next) => {
  console.log(`[EVENT] ${event}`, payload);
  const start = Date.now();
  await next();
  console.log(`[EVENT] ${event} completed in ${Date.now() - start}ms`);
});

// Error handling middleware
emitter.use(async (event, payload, next) => {
  try {
    await next();
  } catch (error) {
    console.error(`[EVENT ERROR] ${event}:`, error);
    // Could send to error reporting service
  }
});
```

---

<div align="center">
  <strong>Previous Module:</strong> [← Setup and Tools](./module-3-setup-tools.md) |
  <strong>Next Module:</strong> [Best Practices and Migration →](./module-5-best-practices.md)
</div>

