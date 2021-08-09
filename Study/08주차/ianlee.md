### 📅 2021년 7월 27일 화요일
# 📚 27장 배열
- 자바스크립트에 배열이라는 타입은 존재하지 않는다. 배열은 객체 타입이다. 배열은 배열 리터럴, Array 생성자 함수, Array.of, Array.from 메서드로 생성할 수 있다. 배열의 생성자 함수는 Array이며, 배열의 프로토타입 객체는 Array.prototype이다.

    ![https://blog.kakaocdn.net/dn/bz0vrV/btraqs12ZNz/IU18v6pLsOKrxQeitP3Ui0/img.png](https://blog.kakaocdn.net/dn/bz0vrV/btraqs12ZNz/IU18v6pLsOKrxQeitP3Ui0/img.png)

    객체 리터럴과 배열 리터럴의 프로토타입

```jsx
// 1. 배열 리터럴
const arr = [1, 2, 3];
console.log(arr.length); // 3

// 2. Array 생성자 함수
const arr = new Array(10);
console.log(arr); // [empty * 10]
console.log(arr.length); // 10

// 3. Array.of (전달된 인수를 요소로 갖는 배열)
Array.of(1); // [1]
Array.of(1, 2, 3); // [1, 2, 3]

// 4. Array.from (유사 배열 객체, 이터러블 객체 인수로 전달받아 배열로 반환)
Array.from({ length: 2, 0: 'a', 1: 'b'}); // ['a', 'b']
Array.from('Hello'); // ['H', 'e', 'l', 'l', 'o']
```

- 밀집 배열(dense array)은 배열의 요소가 하나의 데이터 타입으로 통일되어 있으며 서로 연속적으로 인접해 있고, 희소 배열(sparse array)는 배열의 요소가 연속적으로 이어져 있지 않다. 자바스크립트의 배열은 일반적인 배열의 동작을 흉내 낸 특수한 객체다.
- 자바스크립트는 희소 배열을 문법적으로 허용한다. 배열을 생성할 경우에는 희소 배열을 생성하지 않고 같은 타입의 요소를 연속적으로 위치시키는 것이 최선이다. 희소 배열을 만들지 않으면서 배열의 특정 요소를 완전히 삭제하려면 Array.prototype.splice 메서드를 사용한다.

```jsx
const arr = [1, 2, 3];

delete arr[1];
console.log(arr); // [1, empty, 3]
// length 프로퍼티에 영향을 주지 않으므로 희소 배열이 됨
console.log(arr.length); // 3

const arr = [1, 2, 3];

// Array.prototype.splice(삭제 시작할 인덱스, 삭제할 요소 수)
arr.splice(1, 1);
console.log(arr); // [1, 3]
console.log(arr.length); // 2
```

> 💡 직접 변경하는 메서드 vs 변경하지 않는 메서드
- 배열에는 원본 배열을 직접 변경하는 메서드(mutator method)와 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드(accessor method)가 있다.
- 직접 변경하는 메서드: push, pop, unshift, shift, splice, reverse, fill
- 직접 변경하지 않는 메서드: concat, slice, flat

> 💡 Array.prototype 메서드

Array.prototype.isArray: 전달된 인수가 배열이면 true, 배열이 아니면 false를 반환

Array.prototype.indexOf: 인수로 전달된 요소를 검색하여 인덱스를 반환, 없으면 -1 반환(배열에 특정 요소 존재 확인)

Array.prototype.includes: indexOf 메서드 대신 ES7에서 도입된 메서드로 가독성이 더 좋다

Array.prototype.push: 인수로 전달받은 모든 값을 원본 배열의 마지막 요소로 추가하고 변경된 length 프로퍼티 값을 반환

Array.prototype.pop: 원본 배열에서 마지막 요소를 제거하고 제거한 요소를 반환

Array.prototype.unshift: 인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가하고 변경된 length 프로퍼티 값 반환

Array.prototype.shift: 원본 배열에서 첫 번째 요소를 제거하고 제거한 요소를 반환

Array.prototype.concat: 인수로 전달된 값들을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환(변경X)

Array.prototype.splice: 원본 배열의 중간에 요소를 추가하거나 중간에 있는 요소를 제거

Array.prototype.slice: 인수로 전달된 범위의 요소들을 복사하여 배열로 반환(변경X)

Array.prototype.join: 원본 배열의 모든 요소를 문자열로 변환한 후 구분자로 연결한 문자열 반환(기본 구분자는 콤마)

Array.prototype.reverse: 원본 배열의 순서를 반대로 뒤집는다

Array.prototype.fill: 인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채움

Array.prototype.flat: 인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화

- pop 메서드와 push 메서드를 사용하면 스택을 쉽게 구현할 수 있다. 스택(stack)은 데이터를 마지막에 밀어 넣고, 마지막에 밀어 넣은 데이터를 먼저 꺼내는 후입 선출(LIFO - Last In First Out) 방식의 자료구조다.
- shift 메서드와 push 메서드를 사용하면 큐를 쉽게 구현할 수 있다. 큐(Queue)는 데이터를 마지막에 밀어 넣고, 처음 데이터, 즉 가장 먼저 밀어 넣은 데이터를 먼저 꺼내는 선입 선출(FIFO - First In First Out) 방식의 자료구조다.

![https://blog.kakaocdn.net/dn/b45CYt/btraFENhGeB/L9el2xBa6cCxnfHCjlZgW0/img.png](https://blog.kakaocdn.net/dn/b45CYt/btraFENhGeB/L9el2xBa6cCxnfHCjlZgW0/img.png)

- push와 unshift 메서드는 원본 배열을 직접 변경하지만 concat 메서드는 원본 배열을 변경하지 않고 새로운 배열을 반환한다. 따라서 push와 unshift 메서드를 사용할 경우 원본 배열을 반드시 변수에 저장해 두어야 하며 concat 메서드를 사용할 경우 반환값을 반드시 변수에 할당받아야 한다.

> 💡 고차 함수(Higher-Order Function, HOF)

- 고차 함수는 함수를 인수로 전달받거나 함수를 반환하는 함수를 말한다. 고차 함수는 외부 상태의 변경이나 가변(mutable) 데이터를 피하고 불변성(immutability)을 지향하는 함수형 프로그래밍에 기반을 두고 있다. 함수형 프로그래밍은 순수 함수를 통해 부수 효과를 최대한 억제하여 오류를 피하고 프로그램의 안정성을 높이려는 노력을 하고 있다.

Array.prototype.sort: 배열의 요소를 정렬한다. 원본 배열을 직접 변경하며 정렬된 배열을 반환. 기본적으로 오름차순으로 정렬. 숫자 요소로 이루어진 배열을 정렬할 때는 배열의 요소를 일시적으로 문자열로 변환한 후 유니코드 코드 포인트의 순서를 기준으로 정렬하므로 정렬 순서를 정의하는 비교 함수를 인수로 전달해야 한다.

Array.prototype.forEach: for 문을 대체할 수 있는 고차 함수로 자신의 내부에서 반복문을 통해 자신을 호출한 배열을 순회하면서 수행해야 할 처리를 콜백 함수로 전달받아 반복 호출한다. forEach메서드는 콜백 함수를 호출하면서 3개(요소값, 인덱스 ,this)의 인수를 전달한다. forEach 메서드는 for 문과 달리 break, continue 문을 사용할 수 없으므로 중간에 순회를 중단할 수 없고 모든 요소들을 순회한다.

Array.prototype.map: 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다. 콜백 함수의 반환값들로 구성된 새로운 배열을 반환하고 원본 배열은 변경되지 않는다. forEach 메서드는 언제나 undefined를 반환하고 map 메서드는 콜백 함수의 반환값들로 구성된 새로운 배열을 반환하는 차이가 있다. map메서드는 콜백 함수를 호출하면서 3개(요소값, 인덱스 ,this)의 인수를 전달한다.

Array.prototype.filter: 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출하고 콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열을 반환. 원본 배열은 변경되지 않는다. filter메서드는 콜백 함수를 호출하면서 3개(요소값, 인덱스 ,this)의 인수를 전달한다.

Array.prototype.reduce: 자신을 호출한 배열의 모든 요소를 순회하며 인수로 전달받은 콜백 함수를 반복 호출하고 콜백 함수의 반환값을 다음 순회 시에 콜백 함수의 첫 번째 인수로 전달하면서 콜백 함수를 호출하여 하나의 결과값을 만들어 반환. 원본 배열은 변경되지 않는다.

Array.prototype.some: 콜백 함수의 반환값이 단 한 번이라도 참이면 true, 모두 거짓이면 false 반환

Array.prototype.every: 콜백 함수의 반환값이 모두 참이면 true, 단 한 번이라도 거짓이면 false 반환

Array.prototype.find: 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소 반환, true인 요소가 존재하지 않는다면 undefined 반환

Array.prototype.findIndex: 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소의 인덱스를 반환. 콜백 함수의 반환값이 true인 요소가 존재하지 않는다면 -1을 반환

Array.prototype.flatMap: map 메서드를 통해 생성된 새로운 배열을 평탄화한다. 즉, map 메서드와 flat 메서드를 순차적으로 실행하는 효과가 있다.

### 📅 2021년 7월 29일 목요일
# 📚 28장 Number
1. Number 생성자 함수
    - Number 생성자 함수의 인수로 숫자를 전달하면서 new 연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에 인수로 전달받은 숫자를 할당한 Number 래퍼 객체를 생성한다. [[PrimitiveValue]]라는 접근할 수 없는 프로퍼티는 [[NumberData]] 내부 슬롯을 가리킨다. 인수를 숫자로 변환할 수 없다면 NaN을 [[NumberData]] 내부 슬롯에 할당한 Number 래퍼 객체를 생성한다.

    ```jsx
    let numbObj = new Number('10');
    console.log(numObj); // Number {[[PrimitiveValue]]: 10}

    numbObj = new Number('Hello');
    console.log(numObj); // Number {[[PrimitiveValue]]: NaN}
    ```

2. Number 프로퍼티

    Number.EPSILON

    Number.MAX_VALUE

    Number.MIN_VALUE

    Number.MAX_SAFE_INTEGER

    Number.MIN_SAFE_INTEGER

    Number.POSITIVE_INFINITY

    Number.NEGATIVE_INFINITY

    Number.NAN

3. Number 메서드

    Number.inFinite

    Number.isInteger

    Number.inNaN

    Number.isSafeInteger

    Number.prototype.toExponential

    Number.prototype.toFixed

    Number.prototype.toPrecision

    Number.prototype.toString
# 📚 29장 Math
- Math는 생성자 함수가 아니므로 정적 프로퍼티와 정적 메서드만 제공한다.
1. Math 프로퍼티

    Math.PI

2. Math 메서드

    Math.abs

    Math.round

    Math.ceil

    Math.floor

    Math.sqrt

    Math.random

    Math.pow

    Math.max

    Math.min
# 📚 30장 Date
- Date 메서드

    Date.now

    Date.parse

    Date.UTC

    Date.prototype.getFullYear

    Date.prototype.setFullYear

    Date.prototype.getMonth

    Date.prototype.setMonth

    Date.prototype.getDate

    Date.prototype.setDate

    Date.prototype.getDay

    Date.prototype.getHours

    Date.prototype.setHours

    Date.prototype.getMinutes

    Date.prototype.setMinutes

    Date.prototype.getSeconds

    Date.prototype.setSeconds

    Date.prototype.getMilliseconds

    Date.prototype.setMilliseconds

    Date.prototype.getTime

    Date.prototype.setTime

    Date.prototype.getTimezoneOffset

    Date.prototype.toDateString

    Date.prototype.toTimeString

    Date.prototype.toISOString

    Date.prototype.toLocaleString

    Date.prototype.toLocaleTimeString

![https://blog.kakaocdn.net/dn/Qryjr/btraIn0iwBp/L2fHHt4qCmC8ubWkyrBWm1/img.png](https://blog.kakaocdn.net/dn/Qryjr/btraIn0iwBp/L2fHHt4qCmC8ubWkyrBWm1/img.png)

![https://blog.kakaocdn.net/dn/Zvcvu/btraLsT4vzN/3UzlhheBkBduTPSmnkWvH0/img.png](https://blog.kakaocdn.net/dn/Zvcvu/btraLsT4vzN/3UzlhheBkBduTPSmnkWvH0/img.png)
# 📚 31장 RegExp
- 정규 표현식(regular expression)은 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어(formal language)다.
- 정규 표현식 객체(RegExp 객체)를 생성하기 위해서는 정규 표현식 리터럴과 RegExp 생성자 함수를 사용할 수 있다. 일반적인 방법은 정규 표현식 리터럴을 사용하는 것이다. 리터럴은 패턴과 플래그로 구성된다.
- 정규 표현식의 패턴은 문자열의 일정한 규칙을 표현하기 위해 사용하며, 정규 표현식의 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용한다.

![https://blog.kakaocdn.net/dn/bgmUbb/btraKEUPAoh/pPoNJWjUMMxVGU5kZ0Y4g1/img.png](https://blog.kakaocdn.net/dn/bgmUbb/btraKEUPAoh/pPoNJWjUMMxVGU5kZ0Y4g1/img.png)

> 💡 플래그

![https://blog.kakaocdn.net/dn/oyV1T/btraKD9jND7/TV7zOO7rqk7klgLLTMKaN1/img.png](https://blog.kakaocdn.net/dn/oyV1T/btraKD9jND7/TV7zOO7rqk7klgLLTMKaN1/img.png)

- RegExp 메서드

    RegExp.prototype.exec

    RegExp.prototype.test

    String.prototype.match

- 임의의 문자열 검색

    . 은 임의의 문자 한 개를 의미한다. 문자의 내용은 무엇이든 상관없다.

- 반복 검색

    {m,n}은 최소 m번, 최대 n번 반복되는 문자열을 의미한다. 콤마 뒤에 공백이 있으면 정상 동작하지 않으므로 주의하기 바란다.

    {n,}은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미한다.

    +는 앞선 패턴이 최소 한번 이상 반복되는 문자열을 의미한다. 즉, +는 {1,}과 같다.

    ?는 앞선 패턴이 최대 한 번(0번 포함) 이상 반복되는 문자열을 의미한다. 즉, ?는 {0,1}과 같다.

- OR 검색

    |은 or의 의미를 갖는다. 분해되지 않은 단어 레벨로 검색하기 위해서는 +를 함께 사용한다.

    []내의 문자는 or로 동작한다. 뒤에 +를 사용하면 앞선 패턴을 한 번 이상 반복한다.

    범위를 지정하려면 []내에 -를 사용한다.

    대소문자 구별하지 않고 알파벳 검색 : /[A-Za-z]+/

    숫자 검색 : /[0-9]+/

    \d는 숫자를 의미하므로 [0-9]과 같다. \D는 \d와 반대로 숫자가 아닌 문자를 의미한다.

    \w는 알파벳, 숫자, 언더스코어를 의미한다. \w는 [A-Za-z0-9_]와 같다. \W는 \w와 반대로 알파벳, 숫자, 언더스코어가 아닌 문자를 의미한다.

- NOT 검색

    [...] 내의 ^는 not의 의미를 갖는다.

- 시작 위치로 검색

    [...] 밖의 ^는 문자열의 시작을 의미한다.

- 마지막 위치로 검색

    $는 문자열의 마지막을 의미한다
