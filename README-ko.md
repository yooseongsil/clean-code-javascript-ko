# clean-code-javascript

## 목차
  1. [소개](#소개)
  2. [변수](#변수)
  3. [함수](#함수)
  4. [객체와 자료구조](#객체와-자료구조)
  5. [클래스](#클래스)
  6. [테스트](#테스트)
  7. [동시성](#동시성)
  8. [에러 처리](#에러-처리)
  9. [포멧팅](#포멧팅)
  10. [주석](#주석)
  
## 소개
![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](http://www.osnews.com/images/comics/wtfm.jpg)

이 글은 소프트웨어 방법론에 관한 책중 Robert C. Martin's의 책인 [*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)에 있는 내용을 Javascript language에 적용시켜 적은 글 입니다.
이 글은 단순히 Style Guide가 아니라 Javascript로 코드를 작성할때 읽기 쉽고, 재사용 가능하며 리팩토링 가능하게끔 작성하도록 도와줍니다.

여기있는 모든 원칙이 엄격히 지켜져야하는 것은 아니며, 보편적으로 통용되는 원칙은 아닙니다. 이것들은 지침일 뿐이며 `Clean Code`의 저자가 수년간 경험한 내용을 바탕으로 정리한 것입니다.

소프트웨어 엔지니어링 역사는 50년을 조금 넘겼지만 우리는 아직도 많은 것들을 배우고 있습니다.
그리고 소프트웨어 아키텍쳐가 건축설계 만큼이나 오래되었을때 우리는 아래 규칙들보다 더 엄격한 규칙들을 따라야 할지도 모릅니다.
하지만 지금 당장은 이 가이드 라인을 당신과 당신 팀이 작성하는 JavaScript 코드의 품질을 평가하는 기준으로 삼으세요.

한가지 더 덧붙이자면, 이 원칙들을 알게된다해서 당장 더 나은 개발자가 되는 것은 아니며 코드를 작성할 때 
실수를 하지 않게 해주는 것은 아닙니다. 훌륭한 도자기들이 처음엔 말랑한 점토부터 시작하듯이 모든 코드들은 처음부터 완벽할 수 없습니다.
하지만 당신은 팀원들과 같이 코드를 리뷰하며 점점 완벽하게 만들어가야 합니다. 당신이 처음 작성한 코드를 고칠때 절대로 자신을 질타하지마세요
대신 코드를 부수고 더 나은 코드를 만드세요!

## **변수**
### 의미있고 발음하기 쉬운 변수 이름을 사용하세요

**안좋은 예:**
```javascript
const yyyymmdstr = moment().format('YYYY/MM/DD');
```

**좋은 예:**
```javascript
const yearMonthDay = moment().format('YYYY/MM/DD');
```
**[⬆ 상단으로](#목차)**

### 동일한 유형의 변수에 동일한 어휘를 사용하세요

**안좋은 예:**
```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**좋은 예:**
```javascript
getUser();
```
**[⬆ 상단으로](#목차)**

### 검색가능한 이름을 사용하세요
우리는 코드를 작성하는 것보다 더 많이 코드를 읽습니다. 그렇기 때문에 코드를 읽기쉽고 검색가능하게 작성해야합니다 
그렇지 않으면 여러분의 코드를 이해하려고 하는 사람들에게 큰 어려움을 줍니다.

**안좋은 예:**
```javascript
// 대체 525600이 무엇을 의미하는 걸까요?
for (let i = 0; i < 525600; i++) {
  runCronJob();
}
```

**좋은 예**
```javascript
// 대문자로 `const` 전역 변수를 선언하세요
const MINUTES_IN_A_YEAR = 525600;
for (let i = 0; i < MINUTES_IN_A_YEAR; i++) {
  runCronJob();
}
```
**[⬆ 상단으로](#목차)**

### 독립 변수를 사용하세요
**안좋은 예:**
```javascript
const cityStateRegex = /^(.+)[,\\s]+(.+?)\s*(\d{5})?$/;
saveCityState(cityStateRegex.match(cityStateRegex)[1], cityStateRegex.match(cityStateRegex)[2]);
```

**좋은 예:**
```javascript
const cityStateRegex = /^(.+)[,\\s]+(.+?)\s*(\d{5})?$/;
const match = cityStateRegex.match(cityStateRegex)
const city = match[1];
const state = match[2];
saveCityState(city, state);
```
**[⬆ 상단으로](#목차)**

### 자신만 알아볼 수 있는 작명을 피하세요
명확한 것이 암시적인 것보다 좋습니다.

**안좋은 예:**
```javascript
const locations = ['서울', '인천', '수원'];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  ...
  ...
  ...
  // 잠깐, `l`은 또 뭘까요?
  dispatch(l);
});
```

**좋은 예:**
```javascript
const locations = ['서울', '인천', '수원'];
locations.forEach(location => {
  doStuff();
  doSomeOtherStuff();
  ...
  ...
  ...
  dispatch(location);
});
```
**[⬆ 상단으로](#목차)**

### 문맥상 필요없는 것들을 쓰지 마세요

**안좋은 예:**
```javascript
const Car = {
  carMake: 'BMW',
  carModel: 'M3',
  carColor: '파란색'
};

function paintCar(car) {
  car.carColor = '빨간색';
}
```

**좋은 예:**
```javascript
const Car = {
  make: 'BMW',
  model: 'M3',
  color: '파란색'
};

function paintCar(car) {
  car.color = '빨간색';
}
```
**[⬆ 상단으로](#목차)**

### short-circuit 트릭이 조건문 보다 깔끔합니다

**안좋은 예:**
```javascript
function createMicrobrewery(name) {
  let breweryName;
  if (name) {
    breweryName = name;
  } else {
    breweryName = 'Hipster Brew Co.';
  }
}
```

**좋은 예:**
```javascript
function createMicrobrewery(name) {
  const breweryName = name || 'Hipster Brew Co.'
}
```
**[⬆ 상단으로](#목차)**

## **함수**
### 함수 인자는 2개 이하가 이상적입니다
매개변수의 개수를 제한 하는 것은 함수 테스팅을 쉽게 만들어 주기 때문에 중요합니다. 만약 매개변수가 3개 이상일 경우엔
테스트 해야하는 경우의 수가 많아지고 각기 다른 인수들로 여러 사례들을 테스트 해야합니다.

인자가 한 개도 없는 것이 이상적인 케이스이고 1개나 2개 정도의 인자는 괜찮습니다. 
하지만 3개는 피해야하고 그 이상이라면 줄이는 것이 좋습니다. 
만약 당신이 2개 이상의 인자를 가진 함수를 사용한다면 그 함수에게 너무 많은 역할을 하게 만든 것입니다.
그렇지 않은 경우라면 대부분의 경우 상위 객체는 1개의 인자만으로 충분합니다.

Javascript를 사용할 때 많은 보일러플레이트 없이 바로 객체를 만들 수 있습니다 
그러므로 당신이 만약 많은 인자들을 사용해야 한다면 객체를 이용할 수 있습니다.

**안좋은 예:**
```javascript
function createMenu(title, body, buttonText, cancellable) {
  ...
}
```

**좋은 예:**
```javascript
const menuConfig = {
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
}

function createMenu(menuConfig) {
  ...
}

```
**[⬆ 상단으로](#목차)**

### 함수는 하나의 행동만 해야합니다
이것은 소프트웨어 엔지니어링에서 가장 중요한 규칙입니다. 함수가 1개 이상의 행동을 한다면 작성하는 것도, 테스트하는 것도, 이해하는 것도 어려워집니다.
당신이 하나의 함수에 하나의 행동을 정의하는 것이 가능해진다면 함수는 좀 더 고치기 쉬워지고 코드들은 읽기 쉬워질 것입니다.
많은 원칙들 중 이것만 알아간다 하더라도 당신은 많은 개발자들을 앞설 수 있습니다.

**안좋은 예:**
```javascript
function emailClients(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**좋은 예:**
```javascript
function emailClients(clients) {
  clients
    .filter(isClientActive)
    .forEach(email);
}

function isClientActive(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```
**[⬆ 상단으로](#목차)**

### 함수명은 함수가 무엇을 하는지 알 수 있어야 합니다

**안좋은 예:**
```javascript
function dateAdd(date, month) {
  // ...
}

const date = new Date();

// 뭘 추가하는 건지 이름만 보고 알아내기 힘듭니다.
dateAdd(date, 1);
```

**좋은 예:**
```javascript
function dateAddMonth(date, month) {
  // ...
}

const date = new Date();
dateAddMonth(date, 1);
```
**[⬆ 상단으로](#목차)**

### 함수는 단일 행동을 추상화 해야합니다
추상화된 이름이 여러 의미를 내포하고 있다면 그 함수는 너무 많은 일을 하게끔 설계된 것입니다.
함수들을 나누어서 재사용가능하고 테스트하기 쉽게 만드세요.

**안좋은 예:**
```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(' ');
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      // ...
    })
  });

  const ast = [];
  tokens.forEach(token => {
    // lex...
  });

  ast.forEach(node => {
    // parse...
  })
}
```

**좋은 예:**
```javascript
function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(' ');
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      tokens.push( // ... );
    })
  });

  return tokens;
}

function lexer(tokens) {
  const ast = [];
  tokens.forEach(token => {
    ast.push( // ... );
  });

  return ast;
}

function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const ast = lexer(tokens);
  ast.forEach(node => {
    // parse...
  })
}
```
**[⬆ 상단으로](#목차)**

### 중복된 코드를 작성하지 마세요
무조건 절대 어떤 상황에서도 중복된 코드를 작성하지 마세요. 당신이 프로페셔널한 개발자로서 커밋할때 지을 수 있는 가장
큰 죄입니다. 중복된 코드가 있다는 것은 어떤 로직을 수정해야 할 일이 생겼을 때 수정 해야할 코드가 한 곳 이상이라는 것을 뜻합니다.
Javascript는 타입이 없는 언어이기 때문에 일반적인 함수를 만드는 것이 쉽습니다. [jsinpect](https://github.com/danielstjules/jsinspect)
같은 도구를 이용해 리팩토링 가능한 중복된 코드들을 찾으세요.

**안좋은 예:**
```javascript
function showDeveloperList(developers) {
  developers.forEach(developers => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary: expectedSalary,
      experience: experience,
      githubLink: githubLink
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach(manager => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary: expectedSalary,
      experience: experience,
      portfolio: portfolio
    };

    render(data);
  });
}
```

**좋은 예:**
```javascript
function showList(employees) {
  employees.forEach(employee => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    let portfolio = employee.getGithubLink();

    if (employee.type === 'manager') {
      portfolio = employee.getMBAProjects();
    }

    const data = {
      expectedSalary: expectedSalary,
      experience: experience,
      portfolio: portfolio
    };

    render(data);
  });
}
```
**[⬆ 상단으로](#목차)**

### short circuiting 트릭 대신에 기본 매개변수를 사용하세요

**안좋은 예:**
```javascript
function writeForumComment(subject, body) {
  subject = subject || 'No Subject';
  body = body || 'No text';
}

```

**좋은 예:**
```javascript
function writeForumComment(subject = 'No subject', body = 'No text') {
  ...
}

```
**[⬆ 상단으로](#목차)**

### Object.assign을 사용해 기본 객체를 만드세요

**안좋은 예:**
```javascript
const menuConfig = {
  title: null,
  body: 'Bar',
  buttonText: null,
  cancellable: true
}

function createMenu(config) {
  config.title = config.title || 'Foo'
  config.body = config.body || 'Bar'
  config.buttonText = config.buttonText || 'Baz'
  config.cancellable = config.cancellable === undefined ? config.cancellable : true;

}

createMenu(menuConfig);
```

**좋은 예:**
```javascript
const menuConfig = {
  title: 'Order',
  // 유저가 'body' key의 value를 정하지 않았다.
  buttonText: 'Send',
  cancellable: true
}

function createMenu(config) {
  config = Object.assign({
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true
  }, config);

  // config 이제 다음과 동일합니다.: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```
**[⬆ 상단으로](#목차)**

### 매개변수로 플래그를 사용하지 마세요
플래그를 사용하는 것 자체가 그 함수가 한가지 이상의 역할을 하고 있다는 것을 뜻합니다.
boolean 기반으로 함수가 실행되는 코드가 나뉜다면 함수를 분리하세요.

**안좋은 예:**
```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create('./temp/' + name);
  } else {
    fs.create(name);
  }
}
```

**좋은 예:**
```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile('./temp/' + name);
}
```
**[⬆ 상단으로](#목차)**

### 사이드 이펙트를 피하세요
함수는 값을 받아서 어떤 일을 하거나 값을 리턴할때 사이드 이팩트를 만들어냅니다.
사이드 이팩트는 파일에 쓰여질 수도 있고, 전역 변수를 수정할 수 있으며, 실수로 모든 돈을 다른 사람에게 보낼 수도 있습니다.

당신은 때때로 프로그램에서 사이드 이팩트를 만들어야 할 때가 있습니다. 아까 들었던 예들중 하나인 파일작성을 할때와 같이 말이죠.
이 때 여러분이 해야할 일은 파일 작성을 하는 한 개의 함수를 만드는 일 입니다. 파일을 작성하는 함수나 클래스가
여러개 존재하면 안됩니다. 반드시 하나만 있어야 합니다.

즉, 어떠한 구조체도 없이 객체 사이의 상태를 공유하거나, 무엇이든 쓸 수 있는 변경 가능한 데이터 유형을 사용하거나,
같은 사이드 이펙트를 만들어내는 것을 여러개 만들거나하면 안됩니다. 여러분들이 이러한 것들을 지키며 코드를 작성한다면
대부분의 다른 개발자들보다 행복할 수 있습니다.

**안좋은 예:**
```javascript
// 아래 함수에 의해 참조되는 전역 변수입니다.
// 다음과 같은 전역 변수명을 참조하는 함수가 또 있다면 이 변수는 이제 배열이 될 수도 있고, 우리가 예상한 것이 아닐 수 있습니다.
let name = 'Ryan McDermott';

function splitIntoFirstAndLastName() {
  name = name.split(' ');
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**좋은 예:**
```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(' ');
}

const name = 'Ryan McDermott'
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```
**[⬆ 상단으로](#목차)**

### 전역 함수를 사용하지 마세요
전역 환경을 사용하는 것은 Javascript에서 나쁜 관행입니다. 왜냐하면 다른 라이브러리들과의 충돌이 일어날 수 있고, 
당신의 API를 쓰는 유저들은 예외를 발견할 때까지 그것을 이해하지 못 할 것입니다. 예제를 하나 생각해봅시다.
JavaScript의 네이티브 Array 메서드를 확장하여 두 배열 간의 차이를 보여줄 수있는 `diff` 메서드를 사용하려면 어떻게해야할까요? 
새로운 함수를`Array.prototype`에 쓸 수도 있지만, 똑같은 일을 시도한 다른 라이브러리와 충돌 할 수 있습니다.
다른 라이브러리가 `diff` 메소드를 사용하여 첫번째 요소와 마지막 요소의 차이점을 찾으면 어떻게 될까요?
이것이 단순히 ES2015/ES6의 classes를 사용하여 전역 `Array`를 확장하는 것이 좋은 이유입니다.

**안좋은 예:**
```javascript
Array.prototype.diff = function(comparisonArray) {
  const values = [];
  const hash = {};

  for (let i of comparisonArray) {
    hash[i] = true;
  }

  for (let i of this) {
    if (!hash[i]) {
      values.push(i);
    }
  }

  return values;
}
```

**좋은 예:**
```javascript
class SuperArray extends Array {
  constructor(...args) {
    super(...args);
  }

  diff(comparisonArray) {
    const values = [];
    const hash = {};

    for (let i of comparisonArray) {
      hash[i] = true;
    }

    for (let i of this) {
      if (!hash[i]) {
        values.push(i);
      }
    }

    return values;
  }
}
```
**[⬆ 상단으로](#목차)**

### 명령형 프로그래밍보다 함수형 프로그래밍을 지향하세요
Javascript는 Haskell처럼 함수형 프로그래밍 언어는 아니지만 함수형 프로그래밍 처럼 작성할 수는 있습니다.
함수형 프로그래밍은 코드를 깔끔하고 테스트하기 쉽도록 작성할 수 있습니다. 이러한 프로그래밍 스타일이 익숙해지게끔 해보세요.

**안좋은 예:**
```javascript
const programmerOutput = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```

**좋은 예:**
```javascript
const programmerOutput = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

const totalOutput = programmerOutput
  .map(programmer => programmer.linesOfCode)
  .reduce((acc, linesOfCode) => acc + linesOfCode, 0);
```
**[⬆ 상단으로](#목차)**

### 조건문을 캡슐화 하세요

**Bad:**
```javascript
if (fsm.state === 'fetching' && isEmpty(listNode)) {
  /// ...
}
```

**Good**:
```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === 'fetching' && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```
**[⬆ 상단으로](#목차)**

### 부정적인 조건문을 피하세요

**Bad:**
```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Good**:
```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```
**[⬆ 상단으로](#목차)**

### 조건문 작성을 피하세요
조건문 작성을 피하라는 것은 매우 불가능한 일로 보인다. 이 얘기를 처음 듣는 사람들은 대부분 "`If문` 없이 어떻게 코드를 짜나요?"라고 말합니다. 
하지만 다형성을 이용한다면 동일한 작업을 수행할 수 있습니다. 두번째 질문은 대부분 "물론 그렇게 짤 수 있겠지만 왜 그렇게 해야하나요?"라고 묻습니다. 
그에대한 대답은 앞서 우리가 공부했던 clean code 컨셉에 있습니다. 함수는 단 하나의 일만 수행하여야 합니다. 
당신이 함수나 클래스에 `if문`을 쓴다면 그것은 그 함수나 클래스가 한가지 이상의 일을 수행하고 있다고 말하는 것과 같습니다. 
기억하세요, 하나의 함수는 하나의 일을 해야합니다.

**안좋은 예:**
```javascript
class Airplane {
  //...
  getCruisingAltitude() {
    switch (this.type) {
      case '777':
        return getMaxAltitude() - getPassengerCount();
      case 'Air Force One':
        return getMaxAltitude();
      case 'Cessna':
        return getMaxAltitude() - getFuelExpenditure();
    }
  }
}
```

**좋은 예:**
```javascript
class Airplane {
  //...
}

class Boeing777 extends Airplane {
  //...
  getCruisingAltitude() {
    return getMaxAltitude() - getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  //...
  getCruisingAltitude() {
    return getMaxAltitude();
  }
}

class Cessna extends Airplane {
  //...
  getCruisingAltitude() {
    return getMaxAltitude() - getFuelExpenditure();
  }
}
```
**[⬆ 상단으로](#목차)**

### 타입-체킹을 피하세요 (part 1)
Javascript는 타입이 정해져있지 않습니다. 이는 당신의 함수가 어떤 타입의 인자든 받을 수 있다는 것을 의미합니다.
언젠가 당신은 이 자유로움에 화를 입었었고, 이 때문에 당신의 함수에 타입-체킹을 시도 할 수도 있습니다.
하지만 타입-체킹 말고도 이러한 화를 피할 많은 방법들이 존재합니다. 첫번째 방법은 일관성 있는 API를 사용하는 것입니다.

**안좋은 예:**
```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.peddle(this.currentLocation, new Location('texas'));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location('texas'));
  }
}
```

**좋은 예:**
```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location('texas'));
}
```
**[⬆ 상단으로](#목차)**

### 타입-체킹을 피하세요 (part 2)
당신이 문자열, 정수, 배열등 기본 자료형을 사용하고 다형성을 사용할 수 없을 때 여전히 타입-체킹이 필요하다고 느껴진다면
TypeScript를 도입하는 것을 고려해보는 것이 좋습니다. TypeScript는 표준 자바 스크립트 구문에 정적 타입을 제공하므로 
일반 자바 스크립트의 대안으로 사용하기에 좋습니다. Javascript에서 타입-체킹을 할 때 문제점은 거짓된 `type-safety`
를 얻기위해 작성된 코드를 설명하기 위해서 많은 주석을 달아야한다는 점입니다. Javascript로 코드를 작성할땐 깔끔하게 코드를 작성하고,
좋은 테스트 코드를 짜야하며 좋은 코드 리뷰를 해야합니다. 그러기시르면 그냥 TypeScript를 쓰세요!

**안좋은 예:**
```javascript
function combine(val1, val2) {
  if (typeof val1 == "number" && typeof val2 == "number" ||
      typeof val1 == "string" && typeof val2 == "string") {
    return val1 + val2;
  } else {
    throw new Error('Must be of type String or Number');
  }
}
```

**좋은 예:**
```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```
**[⬆ 상단으로](#목차)**

### 과도하게 최적화 하지 마세요
최신 브라우저들은 런타임에 많은 최적화 작업을 수행합니다. 대부분 당신이 코드를 최적화 하는 것은 시간낭비일 가능성이 많습니다.
최적화가 부족한 곳이 어딘지를 알려주는 [좋은 자료](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)가 여기 있습니다.
이것을 참조하여 최신 브라우저들이 최적화 해주지 않는 부분만 최적화를 해주는 것이 좋습니다.

**Bad:**
```javascript

// 오래된 브라우저의 경우 캐시되지 않은 `list.length`를 통한 반복문은 높은 코스트를 가졌습니다.
// 그 이유는 `list.length`를 매번 계산해야만 했기 때문인데, 최신 브라우저에서는 이것이 최적화 되었습니다.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Good**:
```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```
**[⬆ 상단으로](#목차)**

### 죽은 코드를 지우세요

죽은 코드는 중복된 코드 만큼이나 좋지 않습니다. 죽은 코드는 당신의 코드에 남아있을 어떠한 이유도 없습니다.
호출되지 않는 코드가 있다면 그 코드는 지우세요! 그 코드가 여전히 필요하다면 그것은 버전 히스토리에 안전하게
남아있을 것입니다.

**안좋은 예:**
```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');

```

**좋은 예:**
```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```
**[⬆ 상단으로](#목차)**
