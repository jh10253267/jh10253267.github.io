---
layout: post
title:  "Typescript"
author: 악어새62
categories: [TIL, WEB, Frontend ]
tags: [ "Typescript"]
image: assets/images/5.jpg
---
## 개요

세상에는 여러가지 프로그래밍 언어가 있다. 그 언어마다의 용도와 특성이 모두 다른데 여러가지 언어를 경험해 봤다면 자유로운 형식의 언어와 엄격한 형식의 언어로 나눠볼 수 있을 것이다.

예를 들어 c언어를 사용할 땐 변수의 타입을 지정해주고 간단한 문자열을 출력하기 위해선 6줄의 코드를 적어야했고 파이썬을 사용할 땐 변수의 타입을 지정하지 않고 문자열 출력도 한줄의 코드만 작성하면 된다.

자바스크립트는 비교적 자유로운 형식의 언어에 속한다.

장점으로는 배우기 쉽고 사용할 때도 자유롭게 코드를 작성할 수 있다는 것이 있다. 그러나 이런 자유로움이 반대로 단점으로 작용하기도 한다.

## 필요성

예를 들어 다음과 같은 덧셈 함수가 있다고 하자.
```javascript
function sum(num1, num2) {
  return num1 + num2;
}
```
```javascript
sum(10, 20); // 30
sum('10', '20'); //1020
```
우리가 원하는 결과는 첫 번째 결과처럼 숫자 두 개를 더하는 것이였는데 문자열 두개를 이어버리는 연산이 수행되어버렸다.  
이렇게 타입이 정수형이 아닌 문자형을 입력해도 오류를 발생시키지 않고 작동한다.

의도치 않은 입력값이 들어갔고 그로인해 의도치 않은 동작이 수행된다.  
타입스크립트를 사용하면 이러한 오류를 사전에 방지할 수 있다.

## 정적 타입의 컴파일 언어

* 자바스크립트(동적 타입) - 런타임에서 동작할 때 타입오류를 확인한다.
* 타입스크립트(정적 타입) - 코드 작성단계에서 타입오류 확인
  자바스크립트로 변환(컴파일)후 브라우저나 Node.js환경에서 동작한다.

타입스크립트는 코드 작성 단계에서 타입오류를 확인하고 자바스크립트로 변환되어 동작하는 정적타입의 컴파일 언어라고 할 수 있겠다.

타입스크립트는 자바스크립트의 슈퍼셋으로 완벽하게 호환된다.
슈퍼셋은 기본적으로 자바스크립트의 기능을 모두 가지고있고 추가적인 기능을 포함하고있다는 의미이다.

오늘날 우리가 사용하는 대부분의 라이브러리나 프레임워크는 타입스크립트를 지원한다. 범용적으로 사용할 수 있다는 의미이다.

## Let's get started!

### 개발 환경 구성

우선 Node.js가 설치되어있어야하니 먼저 설치하기.  
[Node.js](https://nodejs.org/en)

1. vs코드 상에서 비어있는 폴더를 하나 생성한다.
2. 터미널을 열어서 `npm init -y`명령어를 입력한다.
3. `npm i -D parcel typescript`명령어를 입력한다.
4. 여기까지 잘 진행이 되었다면 pakage.json가 생성되고 의존성들이 들어가있을 것이다.
5.  pakage.json의 스크립트 부분을 다음과 같이 변경한다.
  ```
  "scripts": {
      "dev": "parcel ./index.html",
      "build": "parcel build ./index.html"
    },
  ```
6. tsconfig.json파일을 만들고 내용을 채워넣는다.
  ```
  {
    "compilerOptions": {
      "target": "ES2015",//변환할 자바스크립트의 버전
      "module": "ESNext",//자바스크립트(ESNext)로 변환
      "moduleResolution": "Node",
      "esModuleInterop": true,//자바스크립트는 ESM방식을 사용하지만 nodejs에선 common방식을 사용한다. 호환여부 true
      "lib": ["ESNext", "DOM"],//타입스크립트에서 자바스크립트로 컴파일될 때 내부적으로 사용되는 라이브러리 지정
      "strict": true//타입스크립트 문법 엄격히 적용
    },
    "include": [
      "src/**/*.ts"//어느 부분에서 타입스크립트 파일을 찾을 수 있는지
    ],
    "exclude": [
      "node_modules"//컴파일시 제외할 경로를 명시
      //해당 경로는 패키지를 보관하기 위한 용도로 컴파일을 위한 용도는 아니기에 제외
    ]
  }
  ```
7. 소스파일 폴더(src)를 만들고 그 안에 main.ts파일을 만든다.  
8. index.html 파일을 생성하고 밑의 스크립트를 추가한다.
  ```
  <script type="module" defer src="./src/main.ts"></script>
  ```


이제 타입스크립트 파일에서 타입스크립트를 작성해볼 수 있다.

간단한 예제를 살펴보자면
```javascript
let hello = 123 // 기존의 자바스크립트 방식 아무런 오류가 나지 않는다.
```
```javascript
//콜론과 자료형을 명시해서 타입을 보장할 수 있다.

let hello: String = 123; 

//오류 발생
//'number' 형식은 'String' 형식에 할당할 수 없습니다.
```

이렇게 작성된 타입 스크립트가 브라우저에서 동작하려면 자바스크립트로 변환되어야한다.

앞에서 의존성으로 추가한 파슬 번들러가 변환역할까지 해줄 수 있다.

이부분도 실습을 해보자면

다음의 명령어로 개발 서버를 오픈할 수 있다.

```
npm run dev
```
실행시키면
```
Server running at http://localhost:1234
```
이런 문구가 나오며 서버가 실행된다.

그리고 파슬 번들러는 결과를 만들 때 dist라는 폴더가 생성되는데 열어보면 사용할 수 있는 Index.html파일이 있고 내부 자바스크립트 부분을 보면 자동으로 자바스크립트 파일로 변환되어 적용되고 있는 것을 볼 수 있다.

```html
<script defer="" src="/index.b7a05eb9.js"></script>
```

그리고 이렇게 변환된 js파일로 한 번 이동해보면 다양한 내용들이 보이는데 여기서 우리가 작성한 코드를 검색해보면 
```javascript
let hello = "hello world!";
```
위와 같이 타입스크립트의 문법이 사라지고 자바스크립트 문법으로 변환된 것을 확인할 수 있다.

타입을 체크하긴 하지만 그 사용법이 다른 언어들과 다르게 자유롭다

숫자
----

```javascript
let num: number
let integer: number = 10
let float: number = 3.14
let infinity: number = Infinity
let nan: number = NaN
```

불리언
----

```javascript
let isBoolean: boolean
let isDone: boolean = false
```

널/언디파인드
----

```javascript
let nul: null
let und: undefined
```

배열
----

```javascript
const fruits: string[] = ['Type', 'script]
const numbers: number[] = [1,2,3,4,5]
const union: (String|number)[] = ['Type', 1, 2, 'script']
```

객체
----

타입스크립트로 객체 데이터를 사용할 경우 object라는 키워드를 명시해서 사용하기 보단 밑의 userA처럼 객체의 필드값에 대한 일종의 정의를 해서 쓴다.


```javascript
const obj: object = {}
const arr: object = []
const func: object = function() {}

const userA: {
  name: string
  age: number
  isValid: boolean
  = {
    name: 'user',
    age: 20,
    isValid: true
  }
}

const userB: {
  name: string
  age: number
  isValid: boolean
  = {
    name: 'user',
    age: 20,
    isValid: true
  }
}
```
uesrA와 userB는 형식은 같고 내용이 다르다

이렇게 형식은 같고 내용이 다른 객체들을 여러개 만들어야한다면 중복되는 코드가 생긴다. 개발자들은 중복되는 코드를 매우 싫어하기 때문에 당연히 중복되는 코드들을 일반화하는 방법이 있을거라고 유추할 수 있다.

바로 인터페이스라는 개념인데 다음과 같이 사용할 수 있다.

```javascript
interface User{ 
  name : string,
  age : number,
  isValid : boolean
}

const userA: User={
  name: 'userA',
  age: 20,
  isValid: true
}

const userB: User={
  name: 'userB',
  age: 22,
  isValid: true  
}
```

인터페이스에 선언되지 않은 속성을 새롭게 추가하면 오류가난다.

함수
----

일반적인 화살표 함수의 사용법에서 크게 벗어나지 않는다. 바뀐 부분은 각 파라미터에 대한 타입 정의와 리턴값의 타입을 지정해주는 부분이다.

마치 컴파일 언어들처럼 작성할 수 있다.

```javascript
const add: (x: number, y: number) => number = function(x, y) {
  return x + y
}
//이렇게 작성할 수도 있다.
const add1: function(x: number, y: number): number{
  return x + y
}
// add함수의 리턴형은 number이기 때문에 받는 변수의 타입도 마찬가지로 number여야한다
const a: number = add(1, 2)
```

리턴해주는 값이 없는 경우는 다음과 같이 작성할 수 있다.

```javascript
const sayHello: () => void = function() {
  console.log("Hello~~")
}

const sayHello1: = function(): void {
  console.log("Hello~~")
}

const h: void = sayHello()
```

Any
-----

any 키워드는 모든 타입이 올 수 있다.

```javascript
let hello: any = 'Hello world'
hello = 1234
hello = null
```

그러나 타입을 체크하지 않느다면 타입스크립트를 사용하는 의미가 없다.

따라서 any타입의 사용을 권장하지 않는다.


Unknown
-------

정확하게 어떤 타입이 올지 알 수 없어서 unknown으로 표현하겠다 라는 의미이다.

any와 비슷한 개념같지만 조금 차이가 있다.

우선 any를 살펴보면 다음과 같다.
```javascript
let hello: any = 'Hello world'

const boo: boolean = hello // 문제없음
const num: number = hello // 문제 없음
```

그러나 unknown은 any처럼 다른 타입의 값에 할당할 수 없다.

```javascript
let hello: unknown = 'Hello world'

const boo: boolean = hello // 할당 불가
const num: number = hello // 할당 불가
```

그래서 타입을 특정짓기 힘들 경우에 any를 사용하는 것보다 타입스크립트의 사용이 의미를 가지려면 unknown을 사용하는 것이 좋다. 


Tuple
-----

```javascript
const tuple: [String, number, boolean] = ['a', 1. true]
```

리스트와 비슷한 형태이고 자료형을 지정할 수 있다.


Void
-------

함수에서 리턴값이 없을 경우 반환 타입을 void로 작성할 수 있다.

```javascript
const sayHello1: = function(): void {
  console.log("Hello~~")
}
```

Never
-----
```js
const nev: [] = []
//const nev: [Never] = []
```

이렇게 아무 타입을 넣어주지 않으면 Never라는 타입이 들어간다. Never타입은 아무값이 들어갈 수 없는 타입이다.

Union
-------
유니온은 타입이 두가지 이상 올 때 사용할 수 있다.
```js
let union: string | number
```
Interscetion
-----

위에서 봤던 인터페이스의 내용을 보면

```javascript
const test: User & Validation = {
  name: 'jun',
  age: 25,
  isValid: true
}
```


이렇게 사용할 수 있다. 


## 타입 추론

 위에서 타입을 매번 지정해줬는데 귀찮다고 느낄수도 있다. 이를 위해 타입스크립트는 타입 추론을 제공한다. 그래서 매번 타입을 지정할 필요 없이 필요한 곳에서만(컴퓨터가 추론하기 힘든부분) 타입을 지정해서 쓸 수 있다.

```javascript
let num = 12
num = 'Hello world' // 할당 불가
```

위의 코드는 number라는 타입을 명시하지 않았다. 그러나 문자열을 재할당 하려고 할 때 에러가난다. 타입스크립트가 num에 초기 할당된 타입이 number라는 것을 추론해서 자동으로 타입을 설정하고있기 때문이다.

조금 더 자세히 알아보면 타입 추론을 하는 일종의 룰이있다.

* 초기화된 변수
* 기본값이 설정된 매개변수
* 반환값이 있는 함수

## Assertion

> **단언** - 추론하지 않고 딱 잘라 말함.

타입스크립트에서의 Assertion의 의미도 다르지 않다.
```js
const el = document.querySelector("body");
```
이렇게 선언한다면 el은 HTMLBodyElement 이거나 null일 수 있다는 문구가 나온다.

만에하나 html 코드에 body가 없다면 쿼리 선택자는 요소를 찾지 못할 수 도 있다. 그러나 일반적인 html코드는 body를 반드시 포함한다.

하지만 타입스크립트의 입장에선 해당 요소가 있음을 확신할 수 없다.

이럴 때 사용하는 것이 Assertion이다.

바로 이렇게

```javascript
const el = document.querytSelector('body') as HTMLBodyElement
```

그러나 body태그가 아니라 일반적인 요소라면? 존재하지 않는 요소일 수 있다. 이럴 때 문제가 되는 게 아래의 경우다.

타입 단언을 통해 el이 null이 아니라고 단언해주었기 때문에 타입 에러가 발생하지 않지만 실제로는 에러가 발생하게되는데...

```javascript
const el = document.querytSelector('.title')
el!.textContent = 'Hello world?!'
```

어떻게 해결할 수 있을까? 바로 자바스크립트에서 false로 인식하는 대표적인 undefined를 체크하는 것이다. 다음과 같다.

```javascript
const el = document.querytSelector('.title')
// el이 존재할 때만 아래 내용을 수행한다.
if(el) {
  el.textContent = 'Hello world?!'
}
```

```javascript
function getNumber(x: number | null | undefined) {
  return Number(x.toFixed(2))
}
```
toFixed는 숫자에 사용할 수 있는 함수이다.

그러니 null과 undifined에는 사용할 수 없다.

이럴 때 Assertion을 사용할 수 있다.

```javascript
function getNumber(x: number | null | undefined) {
  return Number((x as number).toFixed(2))
}
```

이러면 x가 숫자일경우에는 작동하지만 Null이나 undifined가 오면 오류를 발생시킨다.

```javascript
function getNumber(x: number | null | undefined) {
  if(x){
    return Number((x as number).toFixed(2))
  }
}
```

위의 코드를 보면 x는 숫자, 널, 언디파인드일 수 있다. 그래서 이를 이용해 조건문을 작성할 수도 있다. x가 null 혹은 undefined라면 조건문은 false이고 숫자 타입중에서 0을 제외한 숫자들은 모두 참이기 때문에 위와 같이 타입 단언을 해주지 않고도 코드를 작성할 수 있다.


```javascript
function getValue(x: string | number, isNumber: boolean) {
  if(isNumber) {
    return Number(x.toFixed(2))
  }
  return x.toUpperCase()
}

getValue('hello world', false)
getValue(3.14, true)
```

로직상으론 아무 문제가 없다. 그러나 타입스크립트에선 오류를 발생시키는데
x라는 타입 모두에서 toUpperCase를 수행할 수 없다는 에러이다.

타입스크립트가 이해하지 못하는 것이다.

그래서 이런 부분에서도 Assertion을 사용할 수 있다.


```javascript
function getValue(x: string | number, isNumber: boolean) {
  if(isNumber) {
    return Number((x as number).toFixed(2))
  }
  return (x as string).toUpperCase()
}

getValue('hello world', false)
getValue(3.14, true)
```

이렇게 작성하면 오류가 발생하지 않는다.

as라는 키워드를 사용했지만 Assertion은 as만 있지 않다. 느낌표를 사용해서 널이 아니라고 단언을 해줄 수 있다.


```javascript
function getNumber(x: number | null | undefined) {
  return Number(x!.toFixed(2));
}
getNumber(3.141592)
getNumber(null)
```
이렇게.

### 할당 단언
```js
let num: number
console.log(num) // error
num = 123
```

위의 코드에서 에러가 나는 이유는 타입스크립트가 값이 할당되지 않은 변수의 사용을 감지하고 에러를 발생시킨다.

이럴 경우 할당 단언을 사용할 수 있다.

```js
let num!: number
console.log(num) // 정상 작동
```

### 타입 가드

예를 들어 HtmlElement를 인자로 받는 함수가 있다고 해보자.

```typescript
function logText(el: Element) {
  console.log(el.textContent)
}
const h1El = document.querySelector('h1')
logText(h1El)
```
vscode에서는 에러를 발생시킨다.  
쿼리 선택자를 통해서 요소를 찾고있지만 요소가 없을 수도 있기 때문이다.

이런 경우 타입 단언을 사용해볼 수 있다.
```typescript
function logText(el: Element) {
  console.log(el.textContent)
}
const h1El = document.querySelector('h1') as HTMLHeadingElement
logText(h1El)
```
이렇게 작성하면 더 이상 vscode에서 에러로 검출되지 않는다.
그러나 실제 실행을 시켜보면 h1태그가 없는 경우 에러가 난다.

널인 프로퍼티를 읽을 수 없다는 에러로 해당 태그를 찾지 못했을 때 발생하는 대표적인 오류다.

이럴 때 타입 가드의 개념을 사용할 수 있다.  
```typescript
function logText(el: Element) {
  console.log(el.textContent)
}
const h1El = document.querySelector('h1')
if(h1El) {
  logText(h1El)
}
```
이렇게 작성한다면 vscode상에서도 에러가 발생하지 않고 실제 실행시켰을 때도 에러가 발생하지 않는다.

이런 개념이 바로 타입 가드이고 타입스크립트를 사용하면 다음과 같다.
```typescript
function logText(el: Element) {
  console.log(el.textContent)
}
const h1El = document.querySelector('h1')
if(h1El instanceof HTMLHeadingElement) {
  logText(h1El)
}
```

## 인터페이스

인터페이스는 자바와 비슷하게 어떤 약속을 의미한다.  
따라서 name부분에 String타입이 아닌 데이터를 넣거나 속성으로 명시되어있는 값을 지정해주지 않으면 에러가 발생한다.
```typescript
interface User {
  name: string
  age: number
  isValid: boolean
}

const son: User = {
  name: 'Son',
  age: 31,
  isValid: true
}
```
만약 어떤 속성이 반드시 필요한 것이 아니라면 ?기호를 사용해서 선택적 속성으로 사용할 수도 있다.  
`age?: number`  

읽기 전용 개념을 사용할 수도 있다.  
`readonly age: number`  
이렇게 하고 `son.age = 32`와 같이 작성하면 에러가 난다.

함수 타입도 사용이 가능하다.
```typescript
interface GetName {
  (message: string): string
}
interface User {
  name: string
  age: number
  getName: GetName
}

const son: User = {
  name: 'son',
  age: '31',
  getName(message: string) {
    console.log(message)
    return this.name
  }
}
son.getName('Hello~~');
//Hello~~
```

```typescript
interface Fruits {
  [item: number]: string
}
```

### 인덱스 가능 타입

대표적인 인덱스를 사용할 수 있는 자료구조는 배열과 객체가 있다.
```typescript
interface Fruits {
  [item: number]: string
}

const fruits: Fruits = ['Apple', 'Banana', 'Cherry']
console.log(fruits[0])
```
이런 사용 방식을 인덱스 시그니처라고한다.  
```typescript
interface Payload {
  [key: string]: unknown
}
interface Users {
  [key: string]: unknown
  name: string
  age: number
}
function logValues(payload: Payload) {
  for (const key in payload) {
    console.log(payload[key]);
  }
}

const son: User = {
  name: 'Son',
  age: 31,
  isValid: true
}
logValues(son)
```
유저 인터페이스와 페이로드 인터페이스를 작성했다.
이렇게 작성해보면 오류가난다. User인터페이스의 인덱스 시그니쳐 유형이 없다는 것인데 User인터페이스가 `logValues(son)`에서도 사용될 수 있으려면 User 인터페이스도 인덱스 시그니처 형식으로 만들어줘야한다.

즉, 같은 객체 데이터라도 인덱스 가능한 타입인지 아닌지가 달라진다.

### 상속

자바스크립트 클래스를 사용할 때 extends키워드를 이용해서 상속하여 클래스를 확장해서 사용할 수 있었다. 타입스크립트의 인터페이스도 상속을 사용할 수 있다.

```js
interface UserA {
  name: string
  age: number
}
interface UserB  extends UserA {
  isValid: boolean
}

const son: UserA {
  name: 'son',
  age: 31
  isValid: true // error
}
const park: UserB {
  name: 'Park',
  age: 43,
  isValid: true
}
```
UserB에는 isValid속성이 있어서 park은 에러가 나지 않지만 son은 UserA를 사용하고 있기때문에 에러가 난다.

```typescript
interface FullName {
  firstName: string
  lastName: string
}
interface FullName {
  middleName: string
  lastName: boolean
}
```
이처럼 사용하는 것도 가능하다.  
중복되는 인터페이스를 작성하고 그 안에 다른 필드를 추가할 수 있다.

그러나 lastName에서 에러가 생기는데 위의 인터페이스에선 lastName을 string으로 설정했는데 아래에선 lastName을 boolean타입으로 설정했기 때문에 같은 이름으로 속성을 사용할 때에는 같은 타입으로 선언해주어야한다.

## 타입 별칭

타입 별칭이란 타입스크립트에서 사용하는 타입을 직접 정의할 수 있는 기능을 말한다.
```typescript
type TypeA = string
type TypeB = string | number | boolean
type User = {
  name: string
  age: number
  isValid: boolean
} | [string, number, boolean]

const userA: User = {
  name: 'Son'
  age: 31
  isValid: true
}
const userB: User = ['James', 31, false]

function doSomething(param: TypeB): TypeA {
  switch (typeof parma) {
    case 'string':
      return param.toUpperCase()
    case 'number': 
      return param.toFixed(2)
    default:
      return true
  }
}

```
parma이라는 이름으로 TypeB라는 타입을 받고있고 리턴 타입은 TypeA로 위에서 지정한대로 string이다.  
보통 하나의 타입을 별칭을 지정해서 사용하지 않고 유니온 타입이나 인터섹션타입을 지정해서 재사용할 수 있다.  
기본 문법은 `type`키워드와 함께 지정하는 식이다.

doSomething함수의 마지막 부분을 보면 불리언 타입을 리턴하고있는데 리턴 타입은 TypeA여야하기 때문에 에러가 생긴다.  

마치 인터페이스와 비슷한 느낌이다.  
```typescript
type TypeUser = {
  name: string
  age: number
  isValid: boolean
}

interface InterfacedUser {
  name: string
  age: number
  isValid: boolean
}

const son: TypeUser = {
  name: 'son'
  age: 31,
  isValid: true
}
const james: InterfacedUser = {
  name: 'James'
  age: 32,
  isValid: true
}
```
기능적으로는 차이가 없지만 여러 개의 타입의 별칭을 지정하는 의미가 있는 것은 타입 별칭으로 인터페이스는 객체 데이터를 전재하기 때문에 그 의미에 좀 더 맞는 것은 인터페이스다.

## 명시적 this

```typescript
interface Cat {
  name: string
  age: number
}
const cat: Cat = {
  name: 'Lucy',
  age: 3
}

function hello(message: string)  {
  console.log(`Hello ${this.name}, ${message}`);
}
hello.call(cat, 'You are pretty awesome!')
```
call 메소드는 함수나 메소드 어떤 대상에서 실행될지 설정할 수 있다.  
위의 예에선 cat이라는 객체가 된다. this는 cat을 가리킨다.  
실행은 되지만 vscode상에서는 빨간줄이 생기게 되는데 메시지는 this에는 형식 주석이 없기 때문에 암시적으로 any형식이 포함된다는 것이다.

타입스크립트는 형식을 엄격히 지키는 것이 목표임으로 any가 사용되는 것은 좋지 못하다. 따라서 이렇게 수정할 수 있다.
```typescript
function hello(this: Cat ,message: string)  {
  console.log(`Hello ${this.name}, ${message}`);
}
```
이 것을 명시적 this라고 한다.

## 오버로딩

```typescript
function add1(a: string, b: string) {
  return a + b
}
function add2(a: number, b: number) {
  return a + b
}
add1('Hello', 'world')
add2(2, 3)
add1(3, 4) // 에러
add2('simple', 'test') // 에러 
```
```typescript
function add(a: string, b: string): string
function add(a: number, b: number): number
function add(a: any, b: any) {
  return a + b
}
add('Hello', 'world')
add(2, 3)
```
위와 같이 함수의 입력 타입을 나누어서 여러개를 정의했고 마지막에는 a + b를 리턴하는데 이 부분은 함수의 기능을 정의하는 부분이다.

## 접근 제어자

자바의 접근 제어자와 같은 개념이다.  
하나의 클래스에 얼마만큼의 접근 권한을 주느냐를 설정한다.
```typescript
class UserA {
  first: string
  last: string
  age: number

  constructor(first: string, last: string, age: number){
    this.first = first;
    this.last = last;
    this.age = age;
  }
  getAge() {
    return `${this.first} ${this.last} is ${this.age}`
  }
}
```
위의 필드 first, last, age 앞에는 접근제어자가 붙어있지 않다. 이러면 자동으로 public 접근 제어자가 설정된다.  
public 접근 제어자는 어디서든 해당 필드에 접근할 수 있다는 것을 의미한다.
`son.first`

다음 접근 제어자는 protected가 있다. 자기 자신과 자식 클래스에서 접근 가능하다.  
private는 내 클래스에서만 접근이 가능하다.

예를 들어
```typescript
class UserA {
  constructor(
    public first: string = '',
    protected last: string = '', 
    private age: number = 0
    ) {
    this.first = first;
    this.last = last;
    this.age = age;
  }
  protected getAge() {
    return `${this.first} ${this.last} is ${this.age}`
  }
}
class UserB extends UserA {
  getAge() {
    return return `${this.first} ${this.last} is ${this.age}`
  }
}
class UserC extends UserB {
  getAge() {
    return return `${this.first} ${this.last} is ${this.age}`
  }
}

const son = new UserA('Son', `Heungmin`, 31);
console.log(son.first)
console.log(son.last) // 에러
console.log(son.age) // 에러
son.getAge() // 에러
```
last속성은 보호된 속성이며 UserA와 하위 클래스에서만 엑세스 할 수 있다는 메시지가  나온다.   
age속성은 private로 선언되었으며 클래스 내부에서만 엑세스할 수 있다는 메시지가 나온다.

## 제너릭

### 함수

```typescript
interface Obj {
  x: number
}
type Arr = [number, number]

function toArray(a: string, b: string): string[]
function toArray(a: number, b: number): number[]
function toArray(a: boolean, b: boolean): boolean[]
function toArray(a: Obj, b: Obj): Obj[]
function toArray(a: Arr, b: Arr): Arr[]
function toArray(a: any, b: any): any[] {
  return [a, b]
}

console.log(
  toArray('Neo', 'Anderson'),
  toArray(1, 2),
  toArray(true, false),
  toArray({x: 1}, {x: 2}),
  toArray([1, 2], [3, 4])
)
```
이렇게 경우의 수가 많아지면 함수 오버로딩이 길어지는 문제가 생긴다. 이를 해결할 수 있는 방법이 바로 제너릭이다.
```typescript
interface Obj {
  x: number
}
type Arr = [number, number]

function toArray<T>(a: T, b: T) {
  return [a, b]
}

console.log(
  toArray('Neo', 'Anderson'),
  toArray(1, 2),
  toArray(true, false),
  toArray({x: 1}, {x: 2}),
  toArray([1, 2], [3, 4])
  toArray([1, 2], [3, 4, 5]) // 배열
)
```
toArray를 정의하는 부분에 `<>`기호를 사용하고있고 안에 T라는 문자를 사용하고 있다. 이는 어떤 이름으로 지어도 상관이 없지만 타입이라는 의미로 T로 사용하는 것이 일반적이다.
여기에는 타입 정보를 담고 있기 때문에 매개변수의 타입으로 사용할 수 있다.  
예를 들어 `toArray('Neo', 1)`과 같이 작성하면 첫번째 인자로 들어간 `"Neo"`의 영향으로 T의 타입이 결정되게 되고 따라서 두번째 매개변수도 역시 string으로 정해지기 때문에 다른 타입을 넣으면 에러가 발생한다.  
아니면 함수를 호출할 때 타입을 명시할 수도 있다.  
`toArray<string>()`과 같이 사용할 수 있다.

위에서 Arr타입을 정의할 때 배열에 두가지 인수를 넣어줬다 그렇기 때문에`toArray([1, 2], [3, 4, 5])`이렇게 작성하면 에러가 날 것 같지만 숫자 배열로 인식되어 정상 동작한다.  
그러나 두 개의 인수만 가지도록 의도했다면 명시적으로 Arr와 같이 타입을 정의해주어야한다.
`toArr<Arr>`

### 클래스

이번에는 클래스에서 제너릭 문법을 사용하는 방법으로 다음과 같다.
```typescript
class User<P> {
  constructor(public payload: P) {
    
  }
  getPayload() {
    return this.payload
  }
}
```
```typescript
interface UserAType {
  name: string
  age: number
  isValid: boolean
}
interface UserBType {
  name: string
  age: number
  emails: string[]
}

const son = new User<UserAType> {
  name: 'son',
  age: 31,
  isValid: true
  emails: [] // 에러
}
const park = new User<UserBType> {
  name: 'park',
  age: 31,
  emails: []
}
```

### 인터페이스와 제약조건

```typescript
interface MyData<T> {
  name: string
  value: T
}
const dataA: MyData<string> = {
  name: 'Data A',
  value: 'Hello world'
}
const dataB: MyData<number> = {
  name: 'Data A',
  value: 1234
}
```
그러나 만약 T를 사용하되 가능한 타입을 제한하고 싶다면 다음과 같이 사용할 수 있다.  
이를 제약조건이라고 한다.
```typescript
interface MyData<T extends string | number> {
  name: string
  value: T
}
```
