# Clean_Code

[참고 링크](https://github.com/738/clean-code-typescript#%EC%86%8C%EA%B0%9C) 를 통해 꼭 알고 있었으면 하는 내용을 바탕으로 작성 및 인용하였습니다.

## ⭕️ Variable

협업하는 과정에 따라 다르겠지만 camelCase, snake_case, PaskalCase 등등 하나의 방법으로 통일하여 개발하는 것을 추천한다.

**변수명은 의미 있게, 검색할 수 있게 사용하자**

```typescript
recordId: number; // Id의 종류가 무엇인지 알 수 있도록

Bad: getTime(start, 86400000); // 86400000가 무엇을 의미하는 숫자일까?
Good: const MILLISECENDS_IN_A_DAY = 24 * 60 * 60 * 1000;
getTime(start, MILLISECENDS_IN_A_DAY);
```

## ⭕️ Enum

Typescript enum은 그 자체로 객체이지만, enum의 속성을 변경할 수는 없습니다.
이름을 붙여서 코드의 가독성을 높일 때 enum을 사용하면 좋을 것 같습니다.

```typescript
export enum Genre {
  DRAMA = "drama",
  MOVIE = "movie",
  NEWS = "news",
}

projector.configureFilm(Genre.DRAMA);

class Projector {
  configureFilm(genre) {
    switch (genre) {
      case GENRE.DRAMA:
      // 실행되어야 하는 로직
    }
  }
}
```

## ⭕️ Function

함수의 매개변수는 2개 혹은 그 이하가 이상적이라고 알려져 있습니다. 함수 매개변수의 개수를 제한하는 것은 함수를 테스트하기 쉽게 만들어주기 때문에 정말 중요합니다. 함수 매개변수가 3개 이상인 경우, 각기 다른 케이스를 테스트해야 해서 경우의 수가 많아진다.

### **함수는 한 가지만 해야합니다**

소프트웨어 공학에서 가장 중요한 규칙이다. 함수가 한 가지 이상의 역할을 수행하게 되면 테스트하고 추론하기가 어려워진다. 함수를 하나의 행동으로 정의해야 쉽게 리팩토링할 수 있으며 코드를 명료하게 읽을 수 있다. 따라서 꼭 자기의 것으로 만드는 것이 좋다.

```typescript
Bad: function addMenu(title: string, body: string, writer: string, date: Date) {
  // ...
}
addMenu("김치볶음밥", "김치와 스팸의 조합", "김태용", "2022-10-10");

Good: function addMenu(options: {
  title: string;
  body: string;
  writer: string;
  date: Date;
}) {
  // ...
}
addMenu({
  title: "김치볶음밥",
  body: "김치와 스팸의 조합",
  writer: "김태용",
  date: "2022-10-10",
});

Better: type MenuOptions = {
  title: string;
  body: string;
  writer: string;
  date: Date;
};

function addMenu(options: MenuOptions) {
  // ...
}

addMenu({
  title: "김치볶음밥",
  body: "김치와 스팸의 조합",
  writer: "김태용",
  date: "2022-10-10",
});
```

```typescript
조건문 대신 기본 매개변수를 사용하는 것에 익숙해지자
Bad:
function makeLogic(count?: number){
    const loadCount = count !== undefined ? count : 10;

}

Good:
function makeLogic(count: number = 10) {

}
```

### **중복된 코드는 제거하자!**

코드가 중복되지 않게 최선을 다하여야 한다.
중복된 코드는 어떤 로직을 변경할 때 한 곳 이상을 변경해야 하기 때문에 좋지 않습니다.

## ⭕️ Class

**_클래스는 작아야 합니다!_**
SOLID 원칙 중, 단일 책임 원칙에 따르면, 한 클래스는 하나의 책임만 가져야 합니다.

```Typescript
Bad:
class DashBoard {
    getLanguage(): string { /* ... */ }
    setLanguage(language: string): void { /* ... */ }
    showProgress(): void { /* ... */ }
    hideProgress(): void { /* ... */ }
    isDirty(): boolean { /* ... */ }
    disable(): void { /* ... */ }
    enable(): void { /* ... */ }
    addSubscription(subscription: Subscription): void { /* ... */ }
    removeSubscription(subscription: Subscription): void { /* ... */ }
    addUser(user: User): void { /* ... */ }
    removeUser(user: User): void { /* ... */ }
    goToHomePage(): void { /* ... */ }
    updateProfile(details: UserDetails): void { /* ... */ }
    getVersion(): string { /* ... */ }
    // ...
    }


Good:
class Dashboard {
    disable(): void { /* ... */ }
    enable(): void { /* ... */ }
    getVersion(): string { /* ... */ }
    // 다른 클래스에 남은 메소드를 이동시킴으로써 책임을 분산시키세요
    // ...
}
```

**_높은 응집도와 낮은 결합도를 가져야 한다라는 말, 많이 들어보시지 않으셨나요?_**<br>

응집도는 클래스 멤버가 서로에게 연관되어 있는 정도를 정의하는데, 이상적으로 클래스 안의 모든 필드는 각 메소드에 의해 사용되어야 한다. 그 때, 클래스가 최대한으로 응집되어 있다고 말합니다. 이것이 항상 가능한 것은 아니지만 응집도를 높이는 것을 선호해야 합니다.

```Typescript
Bad:
class DatabaseService {
  constructor(
    private readonly db: Database,
    private readonly emailSender: EmailSender
  ) {}

  async findUser(id: number): Promise<User> {
    return await db.users.findOne({ id });
  }

  async sendMessage(): Promise<void> {
    return await emailSender.send("Hello");
  }

  async sendNotification(text: string): Promise<void> {
    return await emailSender.send(text);
  }
}

Good:
class DatabaseService {
  constructor(private readonly db: Database) {}
  async findUser(id: number): Promise<User> {
    return await db.users.findOne({ id });
  }
}

class UserNotifier {
    constructor(private readonly emailSender: EmailSender) {
  }

  async sendMessage(): Promise<void> {
    return await emailSender.send("Hello");
  }

  async sendNotification(text: string): Promise<void> {
    return await emailSender.send(text);
  }
}
```
