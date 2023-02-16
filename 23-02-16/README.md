# 리액트 최신 환경 설정

패키지 설치

```shell
npm init -y
```

## 타입스크립트 세팅

Typescript 의존성 설치

```shell
npm i -D typescript
```

tsconfig.json 파일 생성

```shell
npx tsc --init
```

tsconfig.json 파일에서 jsx 관련 항목 주석을 제거하고 수정.

```shell
"jsx": "react-jsx",
```

## ESLint 세팅

ESLint 설치

```shell
npm i -D eslint
```

ESLint 설정

```shell
npx eslint --init
```

xo 관련 의존성 제거, airbnb 의존성 설치

```shell
npm uninstall eslint-config-xo \
    eslint-config-xo-typescript

npm i -D eslint-config-airbnb \
    eslint-plugin-import \
    eslint-plugin-react \
    eslint-plugin-react-hooks \
    eslint-plugin-jsx-a11y
```

.eslintrc.js 파일 설정

```shell
module.exports = {
  env: {
    browser: true,
    es2021: true,
    jest: true,
  },
  extends: [
    'airbnb',
    'plugin:@typescript-eslint/recommended',
    'plugin:react/recommended',
    'plugin:react/jsx-runtime',
  ],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
  },
  plugins: [
    'react',
    '@typescript-eslint',
  ],
  settings: {
    'import/resolver': {
      node: {
        extensions: ['.js', '.jsx', '.ts', '.tsx'],
      },
    },
  },
  rules: {
    indent: ['error', 2],
    'no-trailing-spaces': 'error',
    curly: 'error',
    'brace-style': 'error',
    'no-multi-spaces': 'error',
    'space-infix-ops': 'error',
    'space-unary-ops': 'error',
    'no-whitespace-before-property': 'error',
    'func-call-spacing': 'error',
    'space-before-blocks': 'error',
    'keyword-spacing': ['error', { before: true, after: true }],
    'comma-spacing': ['error', { before: false, after: true }],
    'comma-style': ['error', 'last'],
    'comma-dangle': ['error', 'always-multiline'],
    'space-in-parens': ['error', 'never'],
    'block-spacing': 'error',
    'array-bracket-spacing': ['error', 'never'],
    'object-curly-spacing': ['error', 'always'],
    'key-spacing': ['error', { mode: 'strict' }],
    'arrow-spacing': ['error', { before: true, after: true }],
    'import/no-extraneous-dependencies': ['error', {
      devDependencies: [
        '**/*.test.js',
        '**/*.test.jsx',
        '**/*.test.ts',
        '**/*.test.tsx',
      ],
    }],
    'import/extensions': ['error', 'ignorePackages', {
      js: 'never',
      jsx: 'never',
      ts: 'never',
      tsx: 'never',
    }],
    'react/jsx-filename-extension': [2, {
      extensions: ['.js', '.jsx', '.ts', '.tsx'],
    }],
    'jsx-a11y/label-has-associated-control': ['error', { assert: 'either' }],
  },
};
```

## VS Code 설정 파일 생성

.vscode 디렉터리 및 settings.json 파일 생성.

```shell
mkdir .vscode

touch .vscode/settings.json
```

`settings.json` 파일은 다음과 같이 작성한다.

```json
{
    "editor.rulers": [
        80
    ],
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    },
    "trailing-spaces.trimOnSave": true
}
```

## Jest 세팅

Jest 및 SWC 지원 패키지 설치.

```shell
npm i -D jest @types/jest @swc/core @swc/jest
```

jest.config.js 파일 생성.

```shell
touch jest.config.js
```

jest.config.js 파일 설정
```js
module.exports = {
  // testEnvironment: 'jsdom',
  // setupFilesAfterEnv: [
  //   '<rootDir>/src/setupTests.ts',
  // ],
  transform: {
    '^.+\\.(t|j)sx?$': ['@swc/jest', {
      jsc: {
        parser: {
          syntax: 'typescript',
          // jsx: true,
          // decorators: true,
        },
        transform: {
          // react: {
          //   runtime: 'automatic',
          // },
          // legacyDecorator: true,
          // decoratorMetadata: true,
        },
      },
    }],
  },
};
```

## React, Parcel 세팅

react 의존성 설치

```shell
npm i react react-dom

npm i -D @types/react @types/react-dom
```

parcel 설치

```shell
npm i -D parcel
```

package.json에 다음 추가

```json
"source": "index.html",
"scripts": {
    "start": "parcel --port 8080",
    "build": "parcel build",
    "check": "tsc --noEmit",
    "lint": "eslint --fix --ext .js,.jsx,.ts,.tsx .",
    "test": "jest",
    "coverage": "jest --coverage --coverage-reporters html",
    "watch:test": "jest --watchAll"
},
```

index.html 생성 후 작성

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>React Demo App</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="./src/main.tsx"></script>
  </body>
</html>
```

src/main.tsx 파일 작성.

```jsx
import ReactDOM from 'react-dom/client';

import App from './App';

function main() {
  const container = document.getElementById('root');
  if (!container) {
    return;
  }

  const root = ReactDOM.createRoot(container);
  root.render(<App />);
}

main();
```

src/App.tsx 파일 작성.

```jsx
export default function App() {
  return (
    <p>Hello, world!</p>
  );
}
```

## React Testing Library 세팅

관련 의존성 설치

```shell
npm i -D @testing-library/react jest-environment-jsdom
```

jest.config.js 파일에서 관련 항목의 주석을 풀어준다.

```js
module.exports = {
  testEnvironment: 'jsdom',
  // setupFilesAfterEnv: [
  //   '<rootDir>/src/setupTests.ts',
  // ],
  transform: {
    '^.+\\.(t|j)sx?$': ['@swc/jest', {
      jsc: {
        parser: {
          syntax: 'typescript',
          jsx: true,
          // decorators: true,
        },
        transform: {
          react: {
            runtime: 'automatic',
          },
          // legacyDecorator: true,
          // decoratorMetadata: true,
        },
      },
    }],
  },
};
```

src/App.test.tsx 파일 작성.

```shell
import { render, screen } from '@testing-library/react';

import App from './App';

test('App', () => {
  render(<App />);

  screen.getByText(/Hello, world/);
});
```

## 잊지말고 ignore도 작성하자

깜박하고 그냥 git add 했다가 한참걸려서 놀랬네

.gitignore, .eslintignore 파일 만들고 작성

```
# Created by https://www.toptal.com/developers/gitignore/api/node
# Edit at https://www.toptal.com/developers/gitignore?templates=node

### Node ###
# Logs
logs
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*
lerna-debug.log*
.pnpm-debug.log*

# Diagnostic reports (https://nodejs.org/api/report.html)
report.[0-9]*.[0-9]*.[0-9]*.[0-9]*.json

# Runtime data
pids
*.pid
*.seed
*.pid.lock

# Directory for instrumented libs generated by jscoverage/JSCover
lib-cov

# Coverage directory used by tools like istanbul
coverage
*.lcov

# nyc test coverage
.nyc_output

# Grunt intermediate storage (https://gruntjs.com/creating-plugins#storing-task-files)
.grunt

# Bower dependency directory (https://bower.io/)
bower_components

# node-waf configuration
.lock-wscript

# Compiled binary addons (https://nodejs.org/api/addons.html)
build/Release

# Dependency directories
node_modules/
jspm_packages/

# Snowpack dependency directory (https://snowpack.dev/)
web_modules/

# TypeScript cache
*.tsbuildinfo

# Optional npm cache directory
.npm

# Optional eslint cache
.eslintcache

# Optional stylelint cache
.stylelintcache

# Microbundle cache
.rpt2_cache/
.rts2_cache_cjs/
.rts2_cache_es/
.rts2_cache_umd/

# Optional REPL history
.node_repl_history

# Output of 'npm pack'
*.tgz

# Yarn Integrity file
.yarn-integrity

# dotenv environment variable files
.env
.env.development.local
.env.test.local
.env.production.local
.env.local

# parcel-bundler cache (https://parceljs.org/)
.cache
.parcel-cache

# Next.js build output
.next
out

# Nuxt.js build / generate output
.nuxt
dist

# Gatsby files
.cache/
# Comment in the public line in if your project uses Gatsby and not Next.js
# https://nextjs.org/blog/next-9-1#public-directory-support
# public

# vuepress build output
.vuepress/dist

# vuepress v2.x temp and cache directory
.temp

# Docusaurus cache and generated files
.docusaurus

# Serverless directories
.serverless/

# FuseBox cache
.fusebox/

# DynamoDB Local files
.dynamodb/

# TernJS port file
.tern-port

# Stores VSCode versions used for testing VSCode extensions
.vscode-test

# yarn v2
.yarn/cache
.yarn/unplugged
.yarn/build-state.yml
.yarn/install-state.gz
.pnp.*

### Node Patch ###
# Serverless Webpack directories
.webpack/

# Optional stylelint cache

# SvelteKit build / generate output
.svelte-kit

# End of https://www.toptal.com/developers/gitignore/api/node
```