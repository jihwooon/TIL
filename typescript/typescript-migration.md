# Typescript 마이그레이션하기
## 개요
JavaScript는 동적 타입 언어로 타입은 실행 중에 결정되기 때문에 타입 안정성을 보장하기가 어렵습니다. 개발 초기에 타입에 대한 명확한 정의가 부족하면 버그 발생 가능성이 높습니다.  
TypeScript는 이런 문제를 해결하기 위해 개발된 언어로, 정적 타입 언어의 특성을 가지고 있습니다. 변수, 함수 및 객체에 대한 명시적 타입을 정의 할 수 있는 장점과 컴파일러를 통해 타입 정의를 실행 하기 전 오류를 찾을 수 있는 장점이 있어 코드 안정성을 높일 수 있습니다.

이에 따라 JavaScript 문제를 해결하기 위해 타입 안정성을 확보하고 있는 TypeScript 환경에서 작업을 진행하게 되었습니다.

## 무엇을 배울 수 있나요?
- JavaScript 에서 TypeScript 환경으로 안정적으로 마이그레이션 하는 방법
- JavaScript와 TypeScript 라이브러리 차이점 알기
- 테스팅 환경와 정적 분석 ESLint을 TypeScript 맞게 설정하는 방법 

### Task 1. Typescript 환경 설정하기
Typescript 의존성 설치하기
```Bash
npm i -D typescript
```
`tsconfig.json` 파일 생성하기
```Bash
tsc --init
```
`tsconfig.json` 환경 설정은 다음과 같습니다.
```json
{
  "compilerOptions": {
    "module": "es2022",
    "target": "ES6",
    "moduleDetection": "auto",
    "noImplicitAny": true,
    "sourceMap": false,
    "outDir": "./dist",
    "allowJs": true,
    "esModuleInterop": true,
    "moduleResolution": "Node"
  },
  "exclude": [
    "node_modules",
    "**/*.(spec|test).ts"
  ],
  "include": [
    "./src/**/*"
  ]
}
```
### Task 2. JS 파일을 TS 파일로 변경하기
프로젝트 내 `.js`파일을 `.ts`로 변경해줍니다.
이후 `tsc -w` 컴파일러를 실행하기 전 `tsconfig.json`파일에 `outDir`옵션을 활성화 합니다.
```json
"outDir": "./dist", // 컴파일 된 `.js`파일을 지정 위치로 저장
```
`.ts` 파일을 컴파일 했다면, dist 폴더 내에 같은 이름의 하위폴더가 생성되고 그 안에 `.js` 파일이 생성됩니다.
> **Note**  
> dist 폴더 내에 컴파일 된 js 파일은 .gitignore 에 dist 추가해서 GitHub에 올라가지 않게 합니다.

만약 프로젝트 내에서 `.ts` 및 `.tsx` 파일 대신 `.js` 파일을 가져올 수 있도록 허용하려면 `"allowJs": true`을 활성화 합니다.

### Task 3. Npm 패키지 매니저에 @Types 설치하기
TypeScript와 JavaScript 라이브러리는 환경 자체가 다르기 때문에 Npm 패키지 매니저에서 **타입 지원 여부**를 확인해야 합니다.
> 타입 지원 여부란?  
> npm 라이브러리 상단에 보면 TS 마크 표시가 있습니다. 이 마크 표시는 TypeScript 지원을 의미함으로 라이브러리를 따로 설치 없이 설치하고 실행하면 됩니다.  
> DT(Definitely Typed) 마크로 표시 된 라이브러리는 타입 지원 없는 라이브러리로 @types 붙여서 라이브러리를 따로 설치해야 합니다.

컴파일일러가 실행이 되면 타입 지원이 필요한 라이브러리에서 에러가 발생합니다. Express 프레임워크는 타입이 정의 되지 않은 라이브러리로 `@types/express`를 따로 설치해야 합니다.
```Bash
npm i --save-dev @types/express
```
라이브러리 중 타입이 필요한 라이브러리를 추가 작업을 합니다.

### Task 4. ts 파일 실행하기
JavaScript 런타임 실행기 Node는 ts 파일을 해석하지 못하는 문제가 있습니다. TypeScript를 해석하도록 도와주는 `ts-node`를 설치합니다.
```Bash
npm install --save @types/node 
```
또는 
```Bash
npm install -D ts-node
```
설치를 합니다. 혹은 모두 설치합니다.

이 때 JavaScript 패키지 유형이 TypeScript에서 다를 수 있습니다. `tsx` 라이브러리를 추가해줍니다.
> tsx는 commonjs 및 모듈 패키지 유형 모두에서 TypeScript 및 ESM을 원활하게 실행하기 위한 CLI 명령(node 대신 사용 가능)입니다.
```Bash
npm install --save-dev tsx
```
어플레케이션이 정상 작동하는지 명령어를 실행합니다.
```Bash
npx tsx watch src/index.ts // main 파일 실행 
```
명령어가 정상적으로 실행이 된다면 `package.json` 파일에 script을 작성합니다. 이 때 npx는 사용하지 않아도 됩니다.
```Bash
"start": "tsx watch src/index.ts"
```
커맨드 라인에 명령어 `npm start`를 실행합니다. 
```Bash
> server@1.0.0 start
> tsx watch src/index.ts

info: Server is running at https://localhost:8080 {"label":"index.js","timestamp":"2024-01-02 22:04:16"}
```

### Task 5. ESLint와 Jest 환경설정을 TypeScript 환경에 맞게 설정하기

**ESlint 설정하기**  
`eslint` 자동 파일 생성하기
- eslint 체크 리스트 답변하기
- 가급적이면 `.js`파일로 생성하는 것을 추천
- airbnb 스타일을 주로 사용함
```Bash
npx eslint --init
```
TypeScript 환경 ESlint 추가하기
```Bash
npm install -D @typescript-eslint/eslint-plugin @typescript-eslint/parser eslint-config-airbnb-typescript eslint-import-resolver-typescript eslint-plugin-promise
```
`.eslintrc.js` 파일을 설정하기
```Javascript
module.exports = {
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
  },
  parser: '@typescript-eslint/parser',
  env: {
    node: true,
    'jest/globals': true,
  },
  extends: [
    'airbnb-base',
    'plugin:jest/recommended',
    'plugin:import/typescript',
    'plugin:@typescript-eslint/recommended',
  ],
  plugins: [
    'jest',
    'import',
    '@typescript-eslint',
  ],
  overrides: [
    {
      env: {
        node: true,
      },
      files: [
        '.eslintrc.{js,cjs}',
      ],
      parserOptions: {
        sourceType: 'script',
      },
    },
  ],
  settings: {
    'import/resolver': {
      node: {
        extensions: ['.js', '.jsx', '.ts', '.tsx'],
      },
    },
    'import/parsers': {
      '@typescript-eslint/parser': ['.ts'],
    },
  },
  ignorePatterns: ['dist', 'scripts'],
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
    'import/no-extraneous-dependencies': 'off',
    'jest/no-disabled-tests': 'warn',
    'jest/no-focused-tests': 'error',
    'jest/no-identical-title': 'error',
    'jest/prefer-to-have-length': 'warn',
    'jest/valid-expect': 'error',
    '@typescript-eslint/no-unused-vars': 'off',
    '@typescript-eslint/interface-name-prefix': 'off',
    '@typescript-eslint/explicit-function-return-type': 'off',
    '@typescript-eslint/explicit-module-boundary-types': 'off',
    '@typescript-eslint/no-explicit-any': 'off',
    'import/extensions': ['error', 'ignorePackages', {
      js: 'never',
      jsx: 'never',
      ts: 'never',
      tsx: 'never',
    }],
    'import/order': [
      'error',
      {
        'newlines-between': 'always-and-inside-groups',
        groups: [
          'builtin',
          'external',
          'internal',
        ],
      },
    ],
  },
};
```
**Jest 설정하기**
Jest 및 swc 컴파일 패키지 설치하기
```bash
npm i -D jest ts-jest @types/jest @swc/core @swc/jest 
```
`jest.config.js` 파일 생성하기
```Javascript
/** @type {import('ts-jest').JestConfigWithTsJest} */
module.exports = {
  testEnvironment: 'node',
  moduleFileExtensions: [
    'js',
    'json',
    'ts',
  ],
  setupFiles: [
    'jest-plugin-context/setup',
  ],
  testPathIgnorePatterns: [
    '/node_modules/',
  ],
  rootDir: '.',
  transform: {
    '^.+\\.(t|j)s?$': [
      '@swc/jest',
    ],
  },
  collectCoverageFrom: [
    '**/*.(t|j)s',
  ],
  roots: [
    '<rootDir>/',
  ],
  coverageDirectory: '../coverage',
};
```

### 참고 사항
- https://www.npmjs.com/package/ts-node
- https://www.npmjs.com/package/tsx
- https://velog.io/@jihwooon/%EB%A6%AC%EB%89%B4%EC%96%BC-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%98%AC%EC%9D%B8%EC%9B%90-Part2.-%EC%8B%A4%EC%A0%84-%EB%B6%84%EC%84%9D%ED%8E%B8-%EC%84%B8%EC%85%981





