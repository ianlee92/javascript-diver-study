> 📖 13장 - 식별자 결정(identifier resolution) 문제

```jsx
var x = 'global';

function foo () {
  var x = 'function scope';
  console.log(x);
}

foo(); // (1)
console.log(x); // (2)
```

<img src="../icon.png">

⇒ (1) function scope (2) global

- 스코프란 자바스크립트 엔진이 식별자를 검색할 때 사용하는 규칙이라고 할 수 있다.
  
* * *
  
> 📖 13장 - 스코프 체인에 의한 함수 검색 문제

```jsx
// 전역 함수
function foo() {
  console.log('global function foo');
}

function bar() {
  // 중첩 함수
  function foo() {
    console.log('local function foo');
  }
  foo(); // (1)
}

bar();
```

<img src="../icon.png">

⇒ (1) local function foo

- 함수도 식별자에 할당되기 때문에 스코프를 갖는다. 사실 함수는 식별자에 함수 객체가 할당된 것 외에는 일반 변수와 다를 바 없다. 따라서 스코프를 "변수를 검색할 때 사용하는 규칙"이라고 표현하기보다는 "식별자를 검색하는 규칙"이라고 표현하는 편이 좀 더 적합하다.

* * *

> 📖 13장 - 렉시컬 스코프(lexical scope) 문제

```jsx
var x = 1;

function foo() {
  var x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); // (1)
bar(); // (2)
```

<img src="../icon.png">

⇒ (1) 1 (2) 1

- 함수의 상위 스코프는 함수 정의가 실행될 때 정적으로 결정된다. 함수 정의(함수 선언문 또는 함수 표현식)가 실행되어 생성된 함수 객체는 이렇게 결정된 상위 스코프를 기억한다. 함수가 호출될 때마다 함수가 정의된 상위 스코프를 참조할 필요가 있기 때문이다.
- bar 함수는 전역에서 정의된 함수다. 함수 선언문으로 정의된 bar 함수는 전역 코드가 실행되기 전에 먼저 평가되어 함수 객체를 생성한다. 이때 생성된 bar 함수 객체는 자신이 정의된 스코프, 즉 전역 스코프를 기억한다. 그리고 bar 함수가 호출되면 호출된 곳이 어디인지 관계없이 언제나 자신이 기억하고 있는 전역 스코프를 상위 스코프로 사용한다.

* * *
  
> 📖 14장 - 지역 변수의 생명 주기 문제

```jsx
var x = 'global';

function foo() {
  console.log(x); // (1)
  var x = 'local';
}

foo();
console.log(x); // (2)
```

<img src="../icon.png">

⇒ (1) undefined (2) global

- foo 함수 내부에서 선언된 지역 변수 x는 1의 시점에서 이미 선언되었고 undefined로 초기화되어 있다. 지역 변수는 함수 전체에서 유효하다. 단, 변수 할당문이 실행되기 이전까지는 undefined 값을 갖는다. 이처럼 호이스팅은 스코프를 단위로 동작한다.
