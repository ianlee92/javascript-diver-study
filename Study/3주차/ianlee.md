### 📅 2021년 6월 22일 화요일
# 📚 12장 함수
![https://blog.kakaocdn.net/dn/bzpObZ/btq7UN6UuWP/ukCm1LRcHNGC5kyIVxYvR0/img.png](https://blog.kakaocdn.net/dn/bzpObZ/btq7UN6UuWP/ukCm1LRcHNGC5kyIVxYvR0/img.png)

- 함수 내부로 입력을 전달받는 변수를 매개변수(parameter), 입력을 인수(argument), 출력을 반환값(return value)라 한다.

![https://blog.kakaocdn.net/dn/bwQQCL/btq7QbAmtwD/WCwumNuwtXeLN7B7TWXPuk/img.png](https://blog.kakaocdn.net/dn/bwQQCL/btq7QbAmtwD/WCwumNuwtXeLN7B7TWXPuk/img.png)

- 자바스크립트 엔진이 코드의 문맥에 따라 동일한 함수 리터럴을 표현식이 아닌 문인 **함수 선언문**으로 해석하는 경우와 표현식인 문(값으로 평가되므로 변수에 할당할 수 있는 문)인 **함수 리터럴 표현식**으로 해석하는 경우가 있다.
- 자바스크립트 엔진은 생성된 함수를 호출하기 위해 함수 이름과 동일한 이름의 식별자를 암묵적으로 생성하고, 거기에 함수 객체를 할당한다. 함수는 함수 이름으로 호출하는 것이 아니라 함수 객체를 가리키는 식별자로 호출한다. 자바스크립트 엔진은 함수 선언문을 함수 표현식으로 변환해 함수 객체를 생성한다고 생각할 수 있다.

![https://blog.kakaocdn.net/dn/cGleFq/btq7I0Uz5DA/y50Z6uYhUtwwIYkees92hK/img.png](https://blog.kakaocdn.net/dn/cGleFq/btq7I0Uz5DA/y50Z6uYhUtwwIYkees92hK/img.png)

- 값의 성질을 갖는 객체를 일급 객체라 한다. 자바스크립트의 함수는 일급 객체다. 함수가 일급 객체라는 것은 함수를 값처럼 자유롭게 사용할 수 있다는 의미다. 함수는 일급 객체이므로 함수 리터럴로 생성한 함수 객체를 변수에 할당할 수 있는데 이러한 함수 정의 방식을 함수 표현식(function expression)이라 한다.
- 함수 리터럴의 함수 이름은 생략할 수 있고 이러한 함수를 익명 함수라 한다. 함수 표현식의 함수 리터럴은 함수 이름을 생략하는 것이 일반적이다.

```jsx
// 기명 함수 표현식(named function expression)
var foo = function foo (x, y) {
  return x + y;
};

// 함수 객체를 가리키는 식별자로 호출
console.log(foo(2, 5)); // 7

// 함수 이름으로 호출하면 ReferenceError가 발생한다.
// 함수 이름은 함수 몸체 내부에서만 유효한 식별자다.
console.log(foo(10, 5)); // ReferenceError: foo is not defined
```

- 함수 선언문은 "표현식이 아닌 문"이고 함수 표현식은 "표현식인 문"이다.
- 함수 선언문으로 정의한 함수는 함수 선언문 이전에 호출할 수 있다. 그러나 함수 표현식으로 정의한 함수는 함수 표현식 이전에 호출할 수 없다. 이는 함수 선언문으로 정의한 함수와 함수 표현식으로 정의한 함수의 생성 시점이 다르기 때문이다. 함수 선언문으로 함수를 정의하면 런타임 이전에 함수 객체가 먼저 생성되어 함수 이름과 동일한 식별자에 할당까지 완료된 상태다.

```jsx
// 함수 참조
console.dir(add); // f add(x, y)
console.dir(sub); // undefined

// 함수 호출
console.log(add(2, 5)); // 7
console.log(sub(2, 5)); // TypeError : sub is not a function

// 함수 선언문
function add(x, y) {
  return x + y;
}

// 함수 표현식
var sub = function (x, y) {
  return x - y;
}
```

- var 키워드를 사용한 변수 선언문 이전에 변수를 참조하면 변수 호이스팅에 의해 undefined로 평가되지만 함수 선언문으로 정의한 함수를 함수 선언문 이전에 호출하면 함수 호이스팅에 의해 호출이 가능하다. 함수 표현식으로 함수를 정의하면 함수 호이스팅이 발생하는 것이 아니라 변수 호이스팅이 발생한다.
- 함수는 매개변수의 개수와 인수의 개수가 일치하는지 체크하지 않는다. 인수가 부족해서 인수가 할당되지 않는 매개변수의 값은 undefined이며 매개변수보다 인수가 더 많은 경우 초과된 인수는 무시된다.

```jsx
// 인수가 전달되지 않은 경우 단축 평가를 사용해 매개변수에 기본값을 할당하는 방법

function add(a, b, c) {
  a = a || 0;
  b = b || 0;
  c = c || 0;
  return a + b + c;
}

console.log(add(1, 2, 3)); // 6
console.log(add(1, 2)); // 3
console.log(add(1); // 1
console.log(add()); // 0
```

- ES6에서 도입된 매개변수 기본값을 사용하면 함수 내에서 수행하던 인수 체크 및 초기화를 간소화할 수 있다. 매개변수 기본값은 매개변수에 인수를 전달하지 않았을 경우와 undefined를 전달한 경우에만 유효하다.

```jsx
// ES6에서 도입된 매개변수 기본값

function add(a = 0, b = 0, c = 0) {
  return a + b + c;
}

console.log(add(1, 2, 3)); // 6
console.log(add(1, 2)); // 3
console.log(add(1); // 1
console.log(add()); // 0
```

> 💡  Call-by-value vs Call-by-reference

![https://blog.kakaocdn.net/dn/dCI4p4/btq7Qbgze22/VVMS69j4mi8AEbarTgTVIk/img.png](https://blog.kakaocdn.net/dn/dCI4p4/btq7Qbgze22/VVMS69j4mi8AEbarTgTVIk/img.png)

1. 원시 타입 인수는 Call-by-value(값에 의한 호출)로 동작한다. 이는 함수 호출 시 원시 타입 인수를 함수에 매개변수로 전달할 때 매개변수에 값을 복사하여 함수로 전달하는 방식이다. 이때 함수 내에서 매개변수를 통해 값이 변경되어도 전달이 완료된 원시 타입 값은 변경되지 않는다.

```jsx
function foo(primitive) {
  primitive += 1;
  return primitive;
}

var x = 0;

console.log(foo(x)); // 1
console.log(x);      // 0
```

2. 객체형(참조형) 인수는 Call-by-reference(참조에 의한 호출)로 동작한다. 이는 함수 호출 시 참조 타입 인수를 함수에 매개변수로 전달할 때 매개변수에 값이 복사되지 않고 객체의 참조값이 매개변수에 저장되어 함수로 전달되는 방식이다. 이때 함수 내에서 매개변수의 참조값이 이용하여 객체의 값을 변경했을 때 전달되어진 참조형의 인수값도 같이 변경된다. 매개변수를 통해 객체를 전달받으면 비순수 함수가 된다.

- 객체는 변경할 수 있는 값이며, 참조에 의한 전달 방식으로 동작하기 때문에 발생하는 부작용이다. 이러한 문제의 해결 방법은 객체를 불변 객체로 만들어 사용해 외부 상태가 변경되는 부수 효과를 없앨 수 있다. 외부 상태를 변경하지 않고 외부 상태에 의존하지도 않는 즉, 부수 효과(side effect)가 없는 순수 함수를 통해 부수효과를 최대한 억제하여 오류를 피한다.

```jsx
// 매개변수 prmitive는 원시 값을 전달받고, 매개변수 obj는 객체를 전달받는다
function changeVal(primitive, obj) {
  primitive += 100;
  obj.name = 'Kim';
  obj.gender = 'female';
}

// 외부 상태
var num = 100;
var obj = {
  name: 'Lee',
  gender: 'male'
};

console.log(num); // 100
console.log(obj); // Object {name: 'Lee', gender: 'male'}

// 원시 값은 값 자체가 복사되어 전달되고 객체는 참조 값이 복사되어 전달된다
changeVal(num, obj);

// 원시 값은 원본이 훼손되지 않는다
console.log(num); // 100

// 객체는 원본이 훼손된다
console.log(obj); // Object {name: 'Kim', gender: 'female'}
```

- 함수 정의와 동시에 즉시 호출되는 함수를 즉시 실행 함수(IIFE, Immediately Invoked Function Expression)이라 한다. 즉시 실행 함수는 단 한번만 호출되며 다시 호출할 수 없다.

```jsx
// 기명 즉시 실행 함수(named immediately-invoked function expression)
(function myFunction() {
  var a = 3;
  var b = 5;
  return a * b;
}());

// 익명 즉시 실행 함수(immediately-invoked function expression)
(function () {
  var a = 3;
  var b = 5;
  return a * b;
}());

// SyntaxError: Unexpected token (
// 함수선언문은 자바스크립트 엔진에 의해 함수 몸체를 닫는 중괄호 뒤에 ;가 자동 추가된다.
function () {
  // ...
}(); // => };();

// 따라서 즉시 실행 함수는 소괄호로 감싸준다.
(function () {
  // ...
}());

(function () {
  // ...
})();
```

- 함수 자기 자신을 호출하는 것을 재귀 호출이라 한다. 재귀 함수는 자기 자신을 호출하는 행위, 즉 재귀호출을 수행하는 함수를 말한다.
- 함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수를 콜백 함수(callback function)라고 하며, 매개변수를 통해 함수의 외부에서 콜백 함수를 전달받은 함수를 고차 함수(Higher-Order Function, HOF)라고 한다. 고차 함수는 콜백 함수를 자신의 일부분으로 합성한다.
- 콜백 함수는 함수형 프로그래밍 패러다임뿐만 아니라 비동기 처리(이벤트 처리, Ajax 통신, 타이머 함수 등)에 활용되는 중요한 패턴이다. 콜백 함수는 비동기 처리뿐 아니라 배열 고차 함수(map, filter, reduce)에서도 사용된다.

> 💡  순수 함수 vs 비순수 함수

1. 순수 함수(pure function)는 함수형 프로그래밍에서 어떤 외부 상태에 의존하지도 않고 변경하지도 않는, 즉 부수 효과가 없는 함수로 오직 매개변수를 통해 함수 내부로 전달된 인수에게만 의존해 변환값을 만든다.

2. 비순수 함수(impure function)는 외부 상태에 의존하거나 외부 상태를 변경하는, 즉 부수 효과가 있는 함수이다. 함수 내부에서 외부 상태를 직접 참조하면 외부 상태에 의존하게 되어 반환값이 변할 수 있고 외부 상태도 변경할 수 있으므로 비순수 함수가 된다. 함수 내부에서 외부 상태를 직접 참조하지 않더라도 매개변수를 통해 객체를 전달받으면 비순수 함수가 된다.

### 📅 2021년 6월 24일 목요일
# 📚 13장 스코프
- 모든 식별자(변수 이름, 함수 이름, 클래스 이름 등)는 자신이 선언된 위치에 의해 다른 코드가 식별자 자신을 참조할 수 있는 유효 범위가 결정되는데 이를 스코프라 한다. 즉, 스코프는 식별자가 유효한 범위를 말한다.

![https://blog.kakaocdn.net/dn/bz5xXS/btq73c0EgpK/LsJguL4lZkcflkCmDhigx0/img.png](https://blog.kakaocdn.net/dn/bz5xXS/btq73c0EgpK/LsJguL4lZkcflkCmDhigx0/img.png)

- 코드는 전역(global)과 지역(local)으로 구분할 수 있고 변수는 자신이 선언된 위치(전역 또는 지역)에 의해 자신이 유효한 범위인 스코프가 결정된다. 즉, 전역에서 선언된 변수는 전역 스코프를 갖는 전역 변수이고, 지역에서 선언된 변수는 지역 스코프를 갖는 지역 변수다.
- 모든 스코프는 하나의 계층적 구조로 연결되며, 모든 지역 스코프의 최상위 스코프는 전역 스코프다. 이렇게 스코프가 계층적으로 연결된 것을 스코프 체인(scope chain)이라 한다. 변수를 참조할 때 자바스크립트 엔진은 스코프 체인을 통해 변수를 참조하는 코드의 스코프에서 시작하여 상위 스코프 방향으로 이동하며 선언된 변수를 검색(identifier resolution)한다.
- 상위 스코프에서 유효한 변수는 하위 스코프에서 자유롭게 참조할 수 있지만 하위 스코프에서 유효한 변수를 상위 스코프에서 참조할 수 없다.

> 💡  동적 스코프(dynamic scope) vs 정적 스코프(static scope)

1. 동적 스코프는 함수를 정의하는 시점에서 어디서 호출될지 알 수 없기 때문에 함수가 호출되는 시점에 동적으로 상위 스코프를 결정하는 것이다.
2. 정적 스코프는 함수 정의가 평가되는 시점에 상위 스코프가 정적으로 결정되며, 렉시컬 스코프라고도 한다. 자바스크립트를 비롯한 대부분의 프로그래밍 언어는 렉시컬 스코프를 따른다.

![https://blog.kakaocdn.net/dn/cqcE3Q/btq73cM7qLm/7tj5R7QxxPPzkljolM9c81/img.png](https://blog.kakaocdn.net/dn/cqcE3Q/btq73cM7qLm/7tj5R7QxxPPzkljolM9c81/img.png)  
```jsx
var x = 1;

function foo() {
  var x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); // 1 동적스코프였다면 10이겠지만 정적스코프이므로 1
bar(); // 1
```
- 즉, 자바스크립트는 렉시컬 스코프를 따르므로 함수를 어디서 호출했는지가 아니라 함수를 어디서 정의했는지에 따라 상위 스코프를 결정한다. 함수가 호출된 위치는 상위 스코프 결정에 어떠한 영향도 주지 않으며 함수의 상위 스코프는 언제나 자신이 정의된 스코프다.

# 📚 14장 전역 변수의 문제점
> 💡  지역 변수의 생명 주기 vs 전역 변수의 생명 주기

1. 지역 변수의 생명 주기는 함수의 생명 주기와 일치한다. 변수의 생명 주기는 메모리 공간이 확보(allocate)된 시점부터 메모리 공간이 해제(realse)되어 가용 메모리 풀(memory pool)에 변환되는 시점까지다. 따라서 변수는 자신이 등록된 스코프가 소멸(메모리 해제)될 때 까지 유효하다.
2. 전역 객체(global object)는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체다. 전역 객체는 클라이언트 사이드 환경(브라우저)에서는 window, 서버 사이드 환경(Node.js)에서는 global 객체를 의미한다. 환경에 따라 전역 객체를 가리키는 다양한 식별자(window, self, this, frames, global)가 존재했으나 ES11(ECMAScript 11)에서 globalThis로 통일되었다. var 키워드로 선언한 전역 변수의 생명 주기는 전역 객체의 생명 주기와 일치한다.

> 💡  전역 변수의 문제점

1. 암묵적 결합(implicit coupling): 코드 어디서든 참조하고 할당할 수 있는 변수를 사용하게 되면 모든 코드가 전역 변수를 참조하고 변경할 수 있는 암묵적 결합을 허용하는 것이다. 변수의 유효 범위가 클수록 코드의 가독성은 나빠지고 상태가 변경될 수 있는 위험성도 높아진다.
2. 긴 생명 주기: 전역 변수는 생명 주기가 길기 떄문에 메모리 리소스도 오랜 기간 소비한다. 전역 변수의 상태를 변경할 수 있는 시간도 길고 기회도 많으며 var 키워드는 변수의 중복 선언을 허용하므로 중복 선언, 기본 변수에 값 재할당될 가능성이 높아진다.
3. 스코프 체인 상에서 종점에 존재: 전역 변수는 변수를 검색할 때 가장 마지막에 검색되기 때문에 전역 변수의 검색 속도가 가장 느려 속도의 차이가 생긴다.
4. 네임스페이스 오염: 자바스크립트의 가장 큰 문제점 중 하나는 파일이 분리되어 있다 해도 하나의 전역 스코프를 공유한다는 것이다. 다른 파일 내에서 동일한 인름으로 명명된 전역 변수나 전역 함수가 같은 스코프 내에 존재할 경우 예상치 못한 결과를 가져올 수 있다.

> 💡  전역 변수의 사용을 억제할 수 있는 방법

1. 즉시 실행 함수: 함수 정의와 동시에 호출되는 즉시 실행 함수는 단 한번만 호출되므로 모든 코드를 즉시 실행 함수로 감싸면 모든 변수는 즉시 실행 함수의 지역 변수가 된다.

```jsx
(function () {
  var foo = 10; // 즉시 실행 함수의 지역 변수
  // ...
}());

console.log(foo); // ReferenceError: foo is not defined
```

2. 네임스페이스 객체: 전역에 네임스페이스 역할을 담당할 객체를 생성하고 전역 변수처럼 사용하고 싶은 변수를 프로퍼티로 추가하는 방법이다. 식별자 충돌을 방지하는 효과는 있으나 네임스페이스 객체 자체가 전역 변수에 할당되므로 그다지 유용해 보이지는 않는다.

```jsx
var MYAPP = {}; // 전역 네임스페이스 객체

MYAPP.name = 'Lee';

console.log(MYAPP.name); // Lee
```

3. 모듈 패턴: 클래스를 모방해서 관련이 있는 변수와 함수를 모아 즉시 실행 함수로 감싸 하나의 모듈을 만든다. 모듈 패턴은 자바스크립트의 강력한 기능인 클로저를 기반으로 동작하고 전역 변수의 억제는 물론 캡슐화까지 구현할 수 있다는 것이다.

- 캡슐화(encapsulation)는 객체의 상태(state)를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작(behavior)인 메서드를 하나로 묶는 것을 말한다. 캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보 은닉(information hiding)이라 한다.

```jsx
var Counter = (function () {
  // private 변수
  var num = 0;
  
  // 외부로 공개할 데이터나 메서드를 프로퍼티로 추가한 객체를 반환한다.
  return {
    increase() {
      return ++num;
    },
    decrease() {
      return --num;
    }
  };
}());

// private 변수는 외부로 노출되지 않는다.
console.log(Counter.num); // undefined

console.log(Counter.increase()); // 1
console.log(Counter.increase()); // 2
console.log(Counter.decrease()); // 1
console.log(Counter.decrease()); // 0
```

즉시 실행 함수는 객체를 반환하고 이 객체에는 외부에 노출하고 싶은 변수나 함수를 담아 반환한다. 이때 반환되는 객체의 프로퍼티는 외부에 노출되는 퍼블릭 멤버(public member)이며 외부로 노출되고 싶지 않은 변수나 함수는 반환하는 객체에 추가하지 않으면 외부에서 접근할 수 없는 프라이빗 멤버(private member)가 된다.

4. ES6 모듈: ES6 모듈을 사용하면 더는 전역 변수를 사용할 수 없다. ES6 모듈은 파일 자체의 독자적인 모듈 스코프를 제공한다.

# 📚 15장 let, const 키워드와 블록 레벨 스코프
> 💡  var 키워드로 선언한 변수의 문제점

1. 변수 중복 선언 허용으로 의도치 않게 먼저 선언된 변수 값이 변경되는 부작용이 발생한다.

2. 함수 레벨 스코프이기 때문에 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 된다.

3. 변수 호이스팅에 의해 변수 선언문 이전에 변수를 참조하는 것은 에러를 발생시키지 않지만 프로그램의 흐름상 맞지 않을뿐더러 가독성을 떨어뜨리고 오류를 발생시킬 여지를 남긴다.

> 💡  let 키워드

1. 변수 중복 선언 금지로 이름이 같은 변수를 중복 선언하면 문법 에러(SyntaxError)가 발생한다. let이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용하지 않는다.

2. 블록 레벨 스코프이기 때문에 오로지 함수의 코드 블록만을 지역 스코프로 인정하는 함수 레벨 스코프와 달리 모든 코드 블록(함수, if문, for문, while문, try/catch 문 등)을 지역 스코프로 인정한다.

```jsx
let foo = 1; // 전역 변수

{
  let foo = 2; // 지역 변수
  let bar = 3; // 지역 변수
}

console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined
```

3. 변수 호이스팅이 발생하지 않는 것처럼 동작하지만 var 키워드로 선언한 변수는 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 "선언 단계"와 "초기화 단계"가 한번에 진행된다. 즉시 초기화 단계에서 undefined로 변수를 초기화한다. 따라서 변수 선언문 이전에 변수에 접근해도 스코프에 변수가 존재하기 때문에 에러가 발생하지 않지만, let키워드로 선언한 변수는 "선언 단계"와 "초기화 단계"가 분리되어 진행된다. 따라서 let 키워드로 선언한 변수는 스코프의 시작 지점부터 초기화 단계 시작 지점(변수 선언문)까지 변수를 참조할 수 없는 구간 일시적 사각지대(TDZ, Temporal Dead Zone)에 위치하기 때문에 초기화 단계가 실행되기 이전에 변수에 접근하려고 하면 참조 에러(ReferrenceError)가 발생한다.

![https://blog.kakaocdn.net/dn/8WKQv/btq7ZzPX96v/ctegOKyHTniDAX7jTSE8U0/img.png](https://blog.kakaocdn.net/dn/8WKQv/btq7ZzPX96v/ctegOKyHTniDAX7jTSE8U0/img.png)

```jsx
// var 키워드로 선언한 변수
// 런타임 이전에 선언 단계와 초기화 단계가 실행된다.
// 따라서 변수 선언문 이전에 변수를 참조할 수 있다.
console.log(foo); // undefined

var foo;
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); // 1

// let 키워드로 선언한 변수
// 런타임 이전에 선언 단계가 실행되고 변수는 아직 초기화되지 않았다.
// 초기화 이전의 일시적 사각지대에서는 변수를 참조할 수 없다.
console.log(foo1); // ReferenceError: foo1 is not defined

let foo1; // 변수 선언문에서 초기화 단계가 실행된다.
console.log(foo1); // undefined

foo1 = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo1); // 1
```

- 자바스크립트는 모든 선언(var, let, const, function, function*, class 등)을 호이스팅한다. 단, ES6에서 도입된 let, const, class를 사용한 선언문은 호이스팅이 발생하지 않는 것처럼 동작한다.

4. let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다. 즉, window.foo와 같이 접근할 수 없다. let 전역 변수는 보이지 않는 개념적인 블록 내에 존재하게 된다.

> 💡  const 키워드

- const 키워드는 상수(constant)를 선언하기 위해 사용되지만 항상 그렇지는 않다. const 키워드의 특징은 let 키워드와 대부분 동일하므로 다른 점을 중심으로 살펴보자.

1. const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다. 그렇지 않으면 문법 에러가 발생한다. let 키워드로 선언한 변수와 마찬가지로 블록 레벨 스코프를 가지며 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

2. var 또는 let 키워드로 선언한 변수는 재할당이 자유로우나 const 키워드로 선언한 변수는 재할당이 금지 된다.

3. const 키워드로 선언된 변수에 원시 값을 할당한 경우 원시 값은 변경할 수 없는 값(immutable value)이고 const 키워드에 의해 재할당이 금지되므로 할당된 값을 변경할 수 있는 방법은 없다.

- 일반적으로 상수의 이름은 대문자로 선언해 상수임을 명확히 나타낸다. 여러 단어로 이뤄진 경우에는 언더스코어 (_)로 구분해서 스네이크 케이스로 표현하는 것이 일반적이다. 예) const TAX_RATE = 0.1;

4. const 키워드로 선언된 변수에 원시 값을 할당한 경우 값을 변경할 수 없지만 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다. 새로운 값을 재할당하는 것은 불가능하지만 프로퍼티 동적 생성, 삭제, 프로퍼티 값의 변경을 통해 객체를 변경하는 것은 가능하다.

```jsx
const person = {
  name: 'Lee'
};

// 객체는 변경 가능한 값이다. 따라서 재할당 없이 변경이 가능하다.
person.name = 'Kim';

console.log(person); // {name: "Kim"}
```

> 💡  var vs let vs const

- 변수 선언에는 기본적으로 const를 사용하고 let은 재할당이 필요한 경우에 한정해 사용하는 것이 좋다. 이때 변수의 스코프는 최대한 좁게 만든다. 변수를 선언할 때는 일단 const 키워드를 사용하고 반드시 재할당이 필요하다면 그때 let 키워드로 변경해도 결코 늦지 않다.

![https://blog.kakaocdn.net/dn/eesq2k/btq7ZmpfJyy/de3WYFEkrZOdf7GJ8rudV0/img.png](https://blog.kakaocdn.net/dn/eesq2k/btq7ZmpfJyy/de3WYFEkrZOdf7GJ8rudV0/img.png)

![https://blog.kakaocdn.net/dn/8LqsQ/btq74wYfEco/tonl9kkpN0S4CwzEuJezTk/img.png](https://blog.kakaocdn.net/dn/8LqsQ/btq74wYfEco/tonl9kkpN0S4CwzEuJezTk/img.png)
