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

### Github PR 포맷 생성
`.github` 폴더 생성 및 `pull_request_template.md` 만들기
```Bash
mkdir .github
cd .github
touch pull_request_template.md
```
`pull_request_template.md`에 내용 추가하기
```markdown
## 요약
<!--해당 PR에 대한 설명 혹은 이미지, 링크 등을 넣어주세요. -->

## Pull Request 체크리스트

### TODO
- [ ] PR 올리기 전 **rebase** 동기화를 하셨나요?
- [ ] 마지막 줄에 공백 처리를 하셨나요?
- [ ] 커밋 단위를 의미 단위로 나눴나요?
  - 예시
    - 코드 가독성을 위해 메서드를 추출하라
    - if-else 문을 if 문으로 분리하라
    - 불필요한 메서드를 인라인화하라
- [ ] 커밋 본문을 작성하셨나요?
  - 예시
    - 함수는 한 가지 일을 해야 한다는 원칙에 따라 메서드를 추출합니다.
    - if-else는 컴파일 시 처리가 되어 재컴파일 없이 수정 할 수 없습니다.  
      이에 따라 코드가 실행되는 순간에 실행이 결정되는 if 문으로 수정합니다.
```
