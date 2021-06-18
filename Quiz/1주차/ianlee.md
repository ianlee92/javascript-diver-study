> 📖 4장 - 변수 호이스팅 문제

```jsx
console.log(score); // (1)

score = 80;
var score;

console.log(score); // (2)
```

<img src="../answer.jpeg">

⇒ (1) undefined (2) 80

- 변수 선언은 소스코드가 순차적으로 실행되는 시점인 런타임 이전에 먼저 실행되지만 값의 할당은 소스코드가 순차적으로 실행되는 시점인 런타임에 실행된다. var 키워드로 선언한 변수는 어떠한 값도 할당하지 않아도 undefined라는 값을 갖는다.

* * *


> 📖 4장 - 함수 레벨 스코프(function-level scope) VS 블록 레벨 스코프(block-level scope)

```jsx

var x = 0;
{
  var x = 1;
  console.log(x); // (1)
}
console.log(x); // (2)

let y = 0;
{
  let y = 1;
  console.log(y); // (3)
}
console.log(y); // (4)

const z = 0;
{
  const z = 1;
  console.log(z); // (5)
}
console.log(z); // (6)
```

<img src="../answer.jpeg">

⇒ (1) 1 (2) 1 (3) 1 (4) 0 (5) 1 (6) 0

- 함수 레벨 스코프 (var) : 함수 내에서 선언된 변수는 함수 내에서만 유효하며 함수 외부에서는 참조할 수 없다. 즉, 함수 내부에서 선언한 변수는 지역 변수이며 함수 외부에서 선언한 변수는 모두 전역 변수이다.
- 블록 레벨 스코프 (let, const) : 모든 코드 블록(함수, if문, for문, while문, try/catch 문 등)내에서 선언된 변수는 코드 블록 내에서만 유효하며 코드 블록 외부에서는 참조할 수 없다. 즉, 코드 블록 내부에서 선언한 변수는 지역 변수이다.
