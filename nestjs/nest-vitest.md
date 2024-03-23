# Nest Vitest 테스팅 환경 시작하기

패키지 의존성에 설치하기
```Bash
npm i --save-dev vitest unplugin-swc @swc/core @vitest/coverage-v8
```

### Config 파일 설정하기
`vitest.config.ts` 파일을 생성하여 다음과 같은 내용을 추가합니다.
```Typescript
import swc from 'unplugin-swc';
import { defineConfig } from 'vitest/config';

export default defineConfig({
  plugins: [
    swc.vite({ // SWC로 테스트 파일을 빌드 시 추가해야 합니다.
      module: { type: 'es6' }, // 모듈 유형을 명시적으로 설정하여 `.swcrc` 구성 파일에서 이 값을 상속하지 않도록 합니다.
      jsc: {
        target: 'esnext',
        parser: {
          syntax: 'typescript',
          decorators: true,
        },
        transform: {
          legacyDecorator: true,
          decoratorMetadata: true,
        },
      },
    }),
  ],
  test: {
    globals: true,
    alias: {
      '@src': '/src',
      '@test': '/test',
    },
    root: './',
    coverage: {
      enabled: false, // Enable coverage
      provider: 'v8',
      thresholds: {
        branches: 100,
        functions: 57.14,
        lines: 81.08,
        statements: 81.08,
      },
    },
  },
  resolve: {
    alias: {
      '@src': '/src',
      '@test': '/test',
    },
  },
});
```
E2E 테스트는 별도로 config 파일을 생성해야 합니다. 이때 `include`에 e2e 테스트 파일 경로를 따로 추가해줍니다.
```Typescript
import swc from 'unplugin-swc';
import { defineConfig } from 'vitest/config';

export default defineConfig({
  test: {
    include: ['**/*.e2e-spec.ts'], // 별도로 추가
    globals: true,
    root: './',
  },
  plugins: [swc.vite()],
});
```
E2E 테스트는 supertest 라이브러리를 변경해야 합니다. 네임스페이스 가져올 시 문제가 발생 할 수 있기 때문에 `import * as request from 'supertest'`를 
`import request from 'supertest'`로 변경합니다.

package.json 파일의 테스트 스크립트를 다음과 같이 업데이트합니다
```Bash
{
  "scripts": {
    "test": "vitest run",
    "test:watch": "vitest",
    "test:cov": "vitest run --coverage",
    "test:debug": "vitest --inspect-brk --inspect --logHeapUsage --threads=false",
    "test:e2e": "vitest run --config ./vitest.config.e2e.ts"
  }
}
```
이 설정을 통해 이제 테스트 실행 속도 향상 및 최신 테스트 환경 등 NestJS 프로젝트에서 Vitest 사용의 이점을 누릴 수 있습니다.

### 참고 사항
- [Nest-Vitest](https://docs.nestjs.com/recipes/swc#update-imports-in-e2e-tests)
- [Vitest](https://vitest.dev/)
- [Repository](https://github.com/jihwooon/nest-js-example)