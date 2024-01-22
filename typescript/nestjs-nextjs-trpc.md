# Fullstack TypeScript Monorepo: NestJS, NextJS 13 & tRPC

### pnpm 패키지 생성하기
pnpm 패키지 설치하기
```Bash
pnpm init 
```
pnpm-workspace.yaml 설정하기  
- apps 폴더 내에 workspace 환경 구성하기
```yaml
packages:
  - "apps/*"
```

### NestJS 환경 설정하기
Nest CLI 의존성 설치하기
```Bash
npm i -g @nestjs/cli
```

Nest 프로젝트 생성하기
```Bash
nest new server --strict --skip-git --package-manager=pnpm
```

Nestjs config 설정하기
```Bash
pnpm add @nestjs/config
```

### NextJs 환경 설정하기
Nextjs 의존성 설치하기
```Bash
pnpm create-next-app@latest
```

### MonoRepo 구성하기
전체 `tsconfig.json` 환경 설정은 다음과 같습니다.
```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,
    "incremental": true,
    "skipLibCheck": true,
    "strictNullChecks": true,
    "noImplicitAny": true,
    "strictBindCallApply": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "paths": {
      "@server/*": ["./apps/server/src/*"],
      "@web/*": ["./apps/web/*"]
    }
  }
}
```

server `tsconfig.json` 설정하기
```Bash
{
  "extends": "../../tsconfig.json", // Extend the config options from the root
  "compilerOptions": {
    "module": "commonjs",
    "declaration": true,
    "removeComments": true,
    "allowSyntheticDefaultImports": true,
    "target": "es2017",
    "sourceMap": true,
    "outDir": "./dist"
  }
}
```
web `tsconfig.json` 설정하기
```Bash
{
  "extends": "../../tsconfig.json", // Extend the config options from the root,
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "plugins": [
      {
        "name": "next"
      }
    ]
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx", ".next/types/**/*.ts"],
  "exclude": ["node_modules"]
}
```

script은 다음과 같습니다.
```Bash
"scripts": {
    "dev": "pnpm run --parallel dev",
    "build:server": "pnpm --filter server build",
    "build:web": "pnpm --filter web build",
    "start:server": "pnpm --filter server start:prod",
    "start:web": "pnpm --filter web start"
},
```
`apps/server` script 명령어를 변경합니다.
```Bash
"scripts": {
   // ...
   "start:dev": "nest start --watch",
   // ...
},
```
`start:dev` script를 `dev`로 변경합니다. 
```Bash
"scripts": {
   // ...
   "dev": "nest start --watch",
   // ...
},
```

### trpc 환경 설정하기

### ESLint 환경 설정
`eslint` 자동 파일 생성하기
- eslint 체크 리스트 답변하기
- 가급적이면 `.js`파일로 생성하는 것을 추천
- airbnb 스타일을 주로 사용함
```Bash
npx eslint --init
```

`.eslintrc.js` 파일을 셋팅하기
```Javascript
module.exports = {
  env: {
    node: true,
    jest: true,
  },
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
  },
  plugins: [
    '@typescript-eslint',
    'jest',
  ],
  extends: [
    'airbnb',
    'plugin:@typescript-eslint/recommended',
    'plugin:jest/recommended',
  ],
  root: true,

  settings: {
    'import/resolver': {
      node: {
        extensions: ['.js', '.ts'],
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
    'class-methods-use-this': 'off',
    'no-shadow': 'off',
    'no-plusplus': 'off',
    'no-useless-constructor': 'off',
    'import/prefer-default-export': 'off',
    'consistent-return': 'off',
    'no-unreachable-loop': 'off',
    'no-empty-function': 'off',
    'max-classes-per-file': 'off',
    'no-cond-assign': 'off',
    'import/extensions': ['error', 'ignorePackages', {
      js: 'never',
      ts: 'never',
    }],
    '@typescript-eslint/no-unused-vars': 'off',
    '@typescript-eslint/interface-name-prefix': 'off',
    '@typescript-eslint/explicit-function-return-type': 'off',
    '@typescript-eslint/explicit-module-boundary-types': 'off',
    '@typescript-eslint/no-explicit-any': 'off',
  },
};
```
### Jest 환경 설정하기
Jest 및 swc 컴파일 패키지 설치하기
```bash
npm i -D jest ts-jest @types/jest @swc/core @swc/jest
```
`jest.config.js` 파일 생성하기
```Javascript
/** @type {import('ts-jest').JestConfigWithTsJest} */
module.exports = {
  testEnvironment: 'node',
  testPathIgnorePatterns: [
    '/node_modules',
  ],
  setupFiles: [
    'jest-plugin-context/setup',
  ],
  moduleFileExtensions: [
    'js',
    'json',
    'ts',
  ],
  transform: {
    '^.+\\.ts?$': [
      '@swc/jest',
    ],
  },
  roots: [
    '<rootDir>/',
  ],
  testTimeout: 10000,
};
```
`jest-context` 추가하기
```Bash
npm i -D jest-plugin-context @types/jest-plugin-context
```
`jest.config.js`에 context 추가하기
```Javascript
/** @type {import('ts-jest').JestConfigWithTsJest} */
module.exports = {
  // ...
  setupFiles: [
    'jest-plugin-context/setup',
  ],
  // ...
}
```
