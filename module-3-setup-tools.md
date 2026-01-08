# Module 3: Setup and Tools

## Lesson 1: Installing Node.js and Package Managers

Node.js enables server-side JS/TS execution. npm (or yarn) handles dependencies and scripts.

### Setup Steps

1. Download Node.js from nodejs.org (includes npm).
2. Verify: `node --version` and `npm --version`.
3. Initialize project: `npm init -y` (creates package.json).
4. Install TypeScript: `npm install -D typescript`.
5. Create tsconfig.json: `npx tsc --init`.

### Code Snippets

```bash
# Install TS as dev dependency
npm install --save-dev typescript

# Compile TS to JS
npx tsc

# Watch mode
npx tsc --watch
```

### Exercises

1. **Set up a new TS project**  
   Steps:  
   - `mkdir my-ts-project && cd my-ts-project`  
   - `npm init -y`  
   - `npm install --save-dev typescript`  
   - `npx tsc --init`  
   - Create src/index.ts with `console.log('Hello TS!');`  
   - `npx tsc` (compiles to dist/index.js)

### Quiz
1. What's the difference between npm install and npm install -D? (Dev dependencies for build tools)

## Lesson 2: Configuring TypeScript

tsconfig.json controls compilation. Key options for strict typing.

### Code Snippets

```json
// tsconfig.json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "strict": true,
    "outDir": "./dist",
    "rootDir": "./src"
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

### Exercises

1. **Configure tsconfig for strict mode**  
   Edit tsconfig.json:  
   ```json
   {
     "compilerOptions": {
       "strict": true,
       "noImplicitAny": true
     }
   }
   ```

### Quiz
1. What does "strict": true enable? (All strict type checking)

## Lesson 3: VS Code and Extensions

VS Code with extensions boosts TS productivity.

### Extensions

- TypeScript Hero (auto imports)
- Prettier (code formatting)
- ESLint (linting)
- Jest (testing)

### Setup

Install extensions, configure settings.json:

```json
{
  "typescript.preferences.importModuleSpecifier": "relative",
  "editor.formatOnSave": true
}
```

### Exercises

1. **Set up Prettier and ESLint**  
   - `npm install --save-dev prettier eslint`  
   - Create .prettierrc and .eslintrc.js  
   - Add scripts in package.json: `"lint": "eslint src/**/*.ts", "format": "prettier --write src/**/*.ts"`

### Quiz
1. What does Prettier do? (Auto-formats code)