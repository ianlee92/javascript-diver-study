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
