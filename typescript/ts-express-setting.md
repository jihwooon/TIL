# Typescript-Express 프로젝트 시작하기

### NPM 패키지 설치
```Bash
npm init -y
```

### Typescript 설치
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

### Node 설치

`package.json` node 패키지 설치 
```Bash
npm i node # node 최신 버전
npm install --save @types/node # Typescript 버전 node 설치
```

### TypeScript REPL 설치
`ts-node` Node.js용 REPL 설치하기
```Bash
npm i ts-node
```
