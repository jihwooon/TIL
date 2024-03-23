# NestJS Validation

### Nest 데코레이터로 요청 데이터 액세스
``` Typescript
@Controller('messages')
export class MessagesController {
  @Get()
  listMessage() {}

  @Post()
  createMessage() {}

  @Get('/:id')
  getMessage() {}
}

```
URL 요청을 통해 데이터 정보는 `@Post()`와 `@Get()` 요청 핸들러에 유입합니다.
Nest 사용할 때 요청에서 데이터를 추출하기 위해 데코레이터를 사용합니다.

**HTTP 요청**은 다음과 같이 작동합니다.
![HTTP Request](https://github.com/jihwooon/TIL/assets/68071599/25258cb4-84ff-4813-931d-239160f5f110)

위에서 아래 순으로 요청 메서드와 전체 경로, 프로토콜 포함하고 있습니다.
헤더 정보에는 Host 정보와 Content 타입을 가지고 있고 Body는 데이터 정보를 가지고 있습니다.

Nest는 HTTP 요청에 대한 데코레이터를 제공합니다.
![nest-http](https://github.com/jihwooon/TIL/assets/68071599/3c0a5828-f5a4-4939-b66b-ac50370b1b5a)

`Post /messages/5?validate=true HTTP/1.1` URL의 파라미터 값은 `@Param()`데코레이터를 사용합니다.  
`@Param()`데코레이터는 URL 파라미터 값을 추출하여 문자열로 사용합니다.  
URL 중 `validate=true` 해당 부분은 `@Query()`데코레이터를 사용합니다.  
**헤더의 정보**를 액세스를 하고 싶으면 `@Headers()` 데코레이터를 사용 할 수 있습니다.  
**요청의 본문**은 `@Body()` 데코레이터를 사용합니다.  

Nest 코드에서는 다음과 같이 사용합니다.
```Typescript
import { Controller, Get, Post, Body, Param } from '@nestjs/common';

@Controller('messages')
export class MessagesController {
  @Get()
  listMessage() {
    return 'test';
  }

  @Post()
  createMessage(@Body() body: any) {
    console.log(body);
  }

  @Get('/:id')
  getMessage(@Param('id') id: string) {
    console.log(id);
  }
}
```
`@nestjs/common` 패키지 내에 `Param`, `Body`를 호출합니다.  
이때 Controller는 **클래스 데코레이터** 라고 부릅니다.  :wq
Get, Post는 **메서드 데코레이터** 라고 부릅니다.  
Body, Param은 **인수 데코레이터** 라고 부릅니다.

Param 데코레이터와 Body 데코레이터를 사용해 핸들러 요청에 인수로 사용합니다.

### Nest 검증 규칙 적용하기

POST 요청 시 Body 본문에 데이터를 검증하기 위해 파이프라인이 필요합니다.
데이터가 값이 숫자 같은 것이라면 파이프를 사용하면 유입되는 그 요청을 거부하고, 요청이 컨트롤러에 도착하기 전에 그걸 요청자에게 반환해야 합니다.
![Request](https://github.com/jihwooon/TIL/assets/68071599/130edd81-244c-4da7-9512-4c188192fc37)

파이프은 앱에 유입되는 데이터가 라우터 핸들러에 도달하기 전에 검증하는 것이 파이프입니다.

Nest에서는 `ValidationPipe` 내장 파이프라인을 제공해줍니다.
![validation](https://github.com/jihwooon/TIL/assets/68071599/6ad83f9e-5dfd-4784-b774-072a39467284)

**ValidationPipe 추가하기**
애플리케이션 내에서 파이프를 연결하기 위해서는 `@nestjs/common` 패키지에서 `ValidationPipe`를 호출합니다.    
`app.useGlobalPipes()` 내에 `new ValidationPipe()` 추가해줍니다.  
파이프를 전역으로 적용할 수 있습니다.
```Typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { ValidationPipe } from '@nestjs/common';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new ValidationPipe());
  await app.listen(3000);
}
bootstrap();
```

**검증 규칙 추가하기**
클래스에 유입되는 요청이 갖는 속을 가진 클래스를 생성합니다. 이 클래스는 검증 규칙을 적용 할 수 있습니다. 흔히 DTO 클래스 (=데이터 전송 객체)라고 부릅니다.

- `dtos` 폴더를 생성합니다.
- `create-message.dto.ts` 클래스 파일을 생성합니다.
- `POST` 요청 핸들러 받을 모든 속성 클래스 파일을 만듭니다.
```Typescript
 export class CreateMessageDto {
  content: string;
}
```
- `class-validator` 라이브러리를 사용하여 클래스 자체의 검증 규칙을 적용합니다.
```Typescript
import { IsString } from 'class-validator';

export class CreateMessageDto {
  @IsString()
  content: string;
}
```
만약 class-validator가 설치 되지 않아 오류가 발생한다면 패키지를 설치합니다.
```Typescript
  @Post('/interface')
  createMessageInterface(@Body() body: CreateMessageInfo) {
    console.log(typeof body);
    console.log(body);
  }
```
Body에 `CreateMessageInfo`class를 주입합니다.
