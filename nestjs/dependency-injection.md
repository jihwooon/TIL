# 제어 역전 자세히 알아보기

### 의존성 주입
의존성 주입이란 무엇인가?  
- new 예약어를 사용하여 클래스 내부에 종속되는 클래스의 객체를 생성하는 대신 외부에서 종속 클래스의 객체를 생성 한 후 생성자, 함수의 매개변수로 클래스에 주입하는 것을 의미합니다.

의존성 주입을 해야 하는 이유는 무엇일까요?
- 결합도를 낮춘다: 객체 간의 직접적인 의존성을 제거하여 코드 변경 및 재사용이 용이해집니다.
- 유연성을 높인다: 컨테이너를 통해 다양한 구현체를 쉽게 교체할 수 있습니다.
- 확장성을 향상시킨다: 새로운 기능 추가 시 코드 변경 없이 컨테이너 설정만 변경하면 됩니다.
- 테스트 가능성을 높인다: 의존성을 주입하여 모킹(mocking)을 통해 쉽게 테스트할 수 있습니다.

아래 코드는 객체를 직접 의존성을 생성하여 코드의 결합도가 높습니다. 때문에 코드의 유연성도 낮고 확장성에서 불리한 코드 입니다.
```Typescript
export class MessagesService {
  messagesRepository: MessageRepository;

  constructor() {
    this.messagesRepository = new MessageRepository();
  }
 }
```
이 코드를 다음과 같이 수정합니다.  
이 코드에서는 자체 의존성을 생성하도록 하는 것이 아닌, 생성자에 인수로 의존성 주입하도록 설정합니다.
```Typescript
export class MessagesService {
  messagesRepository: MessageRepository;

  constructor(repo: MessageRepository) {
    this.messagesRepository = repo;
  }
}
```
하지만 MessageRepository는 MessagesService클래스에 의존하는 존재입니다.    
의존성을 분리하기 위해서는 인터페이스 기반 프로그래밍을 해야 합니다.  

`Repository` 인터페이스를 정의를 합니다. Repository 인터페이스를 객체로 제공하여 다른 클래스를 직접 참조하지 않고도 기능을 활용 할 수 있습니다.

```Typescript
interface Repository {
  findOne(id: string);
  findAll();
  create(content: string);
}

export class MessagesService {
  messagesRepository: Repository;

  constructor(repo: Repository) {
    this.messagesRepository = repo;
  }
}

class InMemoryMessageRepository implements MessageRepository {
  private readonly messages: Message[] = [];

  async findOne(id: string): Promise<Message> {
    return this.messages.find((message) => message.id === id);
  }

  async findAll(): Promise<Message[]> {
    return this.messages;
  }

  async create(content: string): Promise<Message> {
    const message = new Message(content);
    this.messages.push(message);
    return message;
  }
}
```
위 코드에서 MessagesService는 MessageRepository 인터페이스에 의존하지만, 구체적인 MessageRepository 구현은 InMemoryMessageRepository 클래스를 사용합니다. 이처럼 의존성 주입을 통해 코드 간의 결합도를 낮추고 유연성을 높일 수 있습니다.
