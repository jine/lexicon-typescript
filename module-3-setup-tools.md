# Module 3: Setup and Tools

## Lesson 1: Installing Node.js and Package Managers

Node.js enables server-side JS/TS execution. npm (or yarn) handles dependencies and scripts.

### Setup Steps

1. Download Node.js from nodejs.org (LTS version recommended, includes npm).
2. Use nvm (Node Version Manager) for multiple Node versions: `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash`
3. Verify installations: `node --version`, `npm --version`, `nvm --version`
4. Initialize project: `npm init -y` (creates package.json)
5. Install TypeScript globally or locally: `npm install -D typescript` (dev dependency recommended)
6. Create tsconfig.json: `npx tsc --init`
7. Set up project structure: `mkdir src dist`, create `src/index.ts`

### Code Snippets

```bash
# Install TypeScript and related tools
npm install --save-dev typescript @types/node ts-node

# Compile TS to JS
npx tsc

# Watch mode for continuous compilation
npx tsc --watch

# Run TS directly without compilation
npx ts-node src/index.ts

# Check TS version
npx tsc --version
```

```json
// package.json scripts section
{
  "scripts": {
    "build": "tsc",
    "dev": "tsc --watch",
    "start": "node dist/index.js",
    "dev:start": "ts-node src/index.ts"
  }
}
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

2. **Set up package.json scripts for development**  
    Add to package.json:  
    ```json
    {
      "scripts": {
        "build": "tsc",
        "start": "node dist/index.js",
        "dev": "ts-node src/index.ts",
        "watch": "tsc --watch"
      }
    }
    ```  
    Usage: `npm run build`, `npm run dev`

3. **Install and use ts-node for direct execution**  
    - `npm install --save-dev ts-node`  
    - Create src/greeter.ts:  
    ```typescript
    export function greet(name: string): string {
      return `Hello, ${name}!`;
    }
    ```  
    - Create src/index.ts:  
    ```typescript
    import { greet } from './greeter';
    console.log(greet('TypeScript'));
    ```  
    - Run: `npx ts-node src/index.ts`

### Quiz
1. What's the difference between npm install and npm install -D? (Dev dependencies for build tools)

## Lesson 2: Configuring TypeScript

tsconfig.json controls compilation. Key options for strict typing.

### Code Snippets

```json
// tsconfig.json - Comprehensive configuration
{
  "compilerOptions": {
    "target": "ES2020",              // ECMAScript target version
    "module": "commonjs",            // Module system
    "lib": ["ES2020", "DOM"],        // Library files to include
    "outDir": "./dist",              // Output directory
    "rootDir": "./src",              // Input directory
    "strict": true,                  // Enable all strict type checking
    "esModuleInterop": true,         // Enable ES module interop
    "skipLibCheck": true,            // Skip type checking of declaration files
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,       // Include JSON modules
    "declaration": true,             // Generate .d.ts files
    "declarationMap": true,          // Source maps for declarations
    "sourceMap": true,               // Generate source maps
    "removeComments": false,         // Keep comments in output
    "noImplicitAny": true,           // Disallow implicit any
    "strictNullChecks": true,        // Strict null checks
    "strictFunctionTypes": true,     // Strict function types
    "noImplicitReturns": true,       // Report implicit returns
    "noFallthroughCasesInSwitch": true, // Fallthrough case in switch
    "moduleResolution": "node"       // Module resolution strategy
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist", "**/*.test.ts"]
}
```

### Exercises

1. **Configure tsconfig for strict mode**  
   Edit tsconfig.json:  
   ```json
    {
      "compilerOptions": {
        "strict": true,
        "noImplicitAny": true,
        "strictNullChecks": true,
        "noImplicitReturns": true
      }
    }
    ```

2. **Configure for different environments**  
    Create tsconfig.build.json for production:  
    ```json
    {
      "extends": "./tsconfig.json",
      "compilerOptions": {
        "sourceMap": false,
        "removeComments": true,
        "declaration": true
      },
      "exclude": ["**/*.test.ts", "**/*.spec.ts"]
    }
    ```  
    Use: `npx tsc --project tsconfig.build.json`

3. **Set up path mapping for clean imports**  
    Add to tsconfig.json:  
    ```json
    {
      "compilerOptions": {
        "baseUrl": "./src",
        "paths": {
          "@/*": ["*"],
          "@/utils/*": ["utils/*"],
          "@/types/*": ["types/*"]
        }
      }
    }
    ```  
    Now you can: `import { helper } from '@/utils/helper';`

### Quiz
1. What does "strict": true enable? (All strict type checking)

## Lesson 3: VS Code and Extensions

VS Code with extensions boosts TS productivity.

### Extensions

- TypeScript Importer (auto imports and organizing)
- Prettier - Code formatter
- ESLint (linting with TypeScript support)
- Jest (testing framework)
- TypeScript Toolbox (snippets and helpers)
- GitLens (git integration)
- Auto Rename Tag (for JSX/TSX if needed later)

### Setup

Install extensions, configure settings.json:

```json
// .vscode/settings.json
{
  "typescript.preferences.importModuleSpecifier": "relative",
  "typescript.suggest.autoImports": true,
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "typescript.updateImportsOnFileMove.enabled": "always"
}
```

```json
// .vscode/extensions.json - Recommended extensions
{
  "recommendations": [
    "ms-vscode.vscode-typescript-next",
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint",
    "orta.vscode-jest",
    "ms-vscode.vscode-json"
  ]
}
```

### Exercises

1. **Set up Prettier and ESLint**  
   - `npm install --save-dev prettier eslint`  
   - Create .prettierrc and .eslintrc.js  
    - Add scripts in package.json: `"lint": "eslint src/**/*.ts", "format": "prettier --write src/**/*.ts"`
    - Configure .eslintrc.js:  
    ```javascript
    module.exports = {
      parser: '@typescript-eslint/parser',
      extends: ['eslint:recommended', '@typescript-eslint/recommended'],
      plugins: ['@typescript-eslint'],
      rules: {
        '@typescript-eslint/no-unused-vars': 'error',
        '@typescript-eslint/explicit-function-return-type': 'off'
      }
    };
    ```

2. **Set up VS Code tasks for development**  
    Create .vscode/tasks.json:  
    ```json
    {
      "version": "2.0.0",
      "tasks": [
        {
          "type": "typescript",
          "tsconfig": "tsconfig.json",
          "group": "build",
          "presentation": {
            "echo": true,
            "reveal": "silent",
            "focus": false,
            "panel": "shared"
          }
        }
      ]
    }
    ```

### Quiz
1. What does Prettier do? (Auto-formats code)
2. Why use ESLint with TypeScript? (Additional linting rules specific to TS)
3. What does "strict": true enable in tsconfig? (All strict type checking options)

## Lesson 4: Build Tools and Testing

Modern development workflows with bundlers and testing frameworks.

### Build Tools

#### Webpack with TypeScript
```javascript
// webpack.config.js
const path = require('path');

module.exports = {
  entry: './src/index.ts',
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'ts-loader',
        exclude: /node_modules/,
      },
    ],
  },
  resolve: {
    extensions: ['.tsx', '.ts', '.js'],
  },
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
};
```

#### Parcel (Zero-config bundler)
```bash
# Install Parcel
npm install --save-dev parcel

# Add to package.json scripts
"scripts": {
  "dev": "parcel src/index.html",
  "build": "parcel build src/index.html"
}
```

### Testing Setup

#### Jest with TypeScript
```bash
# Install testing dependencies
npm install --save-dev jest @types/jest ts-jest
```

```javascript
// jest.config.js
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  roots: ['<rootDir>/src'],
  testMatch: ['**/__tests__/**/*.ts', '**/?(*.)+(spec|test).ts'],
};
```

```typescript
// src/utils.test.ts
import { add } from './utils';

describe('add function', () => {
  it('should add two numbers', () => {
    expect(add(2, 3)).toBe(5);
  });

  it('should handle negative numbers', () => {
    expect(add(-1, 1)).toBe(0);
  });
});
```

### Exercises

1. **Set up Jest for TypeScript testing**  
    - Install dependencies: `npm install --save-dev jest @types/jest ts-jest`  
    - Create jest.config.js as above  
    - Add test script: `"test": "jest", "test:watch": "jest --watch"`  
    - Write a simple test for a utility function

2. **Configure Webpack for TypeScript bundling**  
    - Install: `npm install --save-dev webpack webpack-cli ts-loader`  
    - Create webpack.config.js  
    - Update package.json scripts  
    - Test build: `npm run build`

3. **Set up ESLint with TypeScript rules**  
    - Install: `npm install --save-dev eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin`  
    - Create .eslintrc.js:  
    ```javascript
    module.exports = {
      parser: '@typescript-eslint/parser',
      extends: ['eslint:recommended', '@typescript-eslint/recommended'],
      rules: {
        '@typescript-eslint/no-explicit-any': 'warn'
      }
    };
    ```

### Quiz
1. Why use a bundler like Webpack? (Bundles modules, optimizes for production)
2. What does Jest preset 'ts-jest' do? (Configures Jest to work with TypeScript)
3. Why configure ESLint for TypeScript? (Type-aware linting rules)