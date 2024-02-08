# Typescript 프로젝트 시작하기

### NPM 패키지 생성하기
```Bash
npm init -y
```

### Typescript 환경 설정하기
Typescript 의존성 설치하기 
```Bash
npm i -D typescript
```

`tsconfig.json` 파일 생성하기
```Bash
tsc -init
```

`tsconfig.json` 파일에서 환경 설정하기
```Typescript
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es2017",
    "noImplicitAny": true,
    "sourceMap": false,
    "outDir": "./dist"
  },
  "exclude": [
    "node_modules",
    "**/*.(spec|test).ts"
  ],
  "esModuleInterop": true
}
```

### ESLint 환경 설정
`eslint` 자동 파일 생성하기
- eslint 체크 리스트 답변하기
- 가급적이면 `.js`파일로 생성하는 것을 추천
- airbnb 스타일을 주로 사용함
```Bash
npx eslint --init
```
`eslint airbnb` 패키지 설치하기
```Bash
npm i -D eslint-config-airbnb-base eslint-config-airbnb-typescript 
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
npm i -D jest ts-jest @types/jest @swc/core @swc/jest eslint-plugin-jest@latest
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
### vscode 환경 셋팅
`.vscode` 폴더 내에 `settings.json` 추가하기
```json
{
  // editor 80 ~ 120 사이즈
  "editor.rulers": [
    {
      "column": 80,
      "color": "#f5870a"
    },
    100, // <- a ruler in the default color or as customized (with "editorRuler.foreground") at column 100
    {
      "column": 120,
      "color": "#9f0af5"
    }
  ],
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit", // 저장 시 eslint 적용 
    "source.organizeImports": "explicit" // 저장 시 패키지 자동 정리
  },
  "editor.formatOnSave": false, // 포맷터 자동 적용 유무
  "files.trimTrailingWhitespace": true,
  "editor.insertSpaces": false,
  "editor.tabSize": 2 // 탭 사이즈 2
}
```
