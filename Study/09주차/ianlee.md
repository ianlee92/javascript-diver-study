### 📅 2021년 8월 3일 화요일
# 📚 32장 String
- String 객체의 메서드는 언제나 새로운 문자열을 반환한다. 문자열은 변경 불가능(immutable)한 원시 값이기 때문에 String 래퍼 객체도 읽기 전용(read only)객체로 제공된다.

> 💡 String 메서드

- String.prototype.indexOf: 인수로 전달받은 문자열 검색하여 첫 번째 인덱스 반환, 검색 실패시 -1 반환
- String.prototype.includes: ES6에서 도입된 메서드로 문자열 포함 결과를 true 또는 false로 반환한다.

    ```jsx
    if (str.indexOf('Hello') !== -1 {
      // 문자열 str에 'Hello'가 포함되어 있는 경우에 처리할 내용
    }

    if (str.includes('Hello')) {
      // 문자열 str에 'Hello'가 포함되어 있는 경우에 처리할 내용
    }
    ```

- String.prototype.search: 인수로 전달받은 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스 반환, 검색 실패시 -1 반환
- String.prototype.startsWith: 대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인하여 true 또는 false로 반환
- String.prototype.endsWith: 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인하여 true 또는 false로 반환
- String.prototype.charAt: 대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환
- String.prototype.substring: 첫 번째 인수 인덱스 문자부터 두 번째 인수 인덱스 문자의 바로 이전 문자까지 문자열 반환. 두 번째 인수 생략시 마지막 문자까지 문자열 반환
- String.prototype.slice: substring과 동일하게 동작하지만 음수 인수도 전달 가능하고 음수는 문자열 가장 뒤에서부터 잘라내어 반환

    ```jsx
    const str = 'hello world';

    str.substring(0, 5); // 'hello'
    str.slice(0, 5); // 'hello'

    // 인수 < 0 또는 NaN인 경우 0으로 취급된다.
    str.substring(-5); // 'hello world'
    // slice 메서드는 음수인 인수를 전달할 수 있다. 뒤에서 5자리를 잘라내어 반환
    str.slice(-5); // world
    ```

    ![https://blog.kakaocdn.net/dn/c0gXYu/btra1ZxLUdc/TUUp93pTWAITpUssGV1Sp0/img.png](https://blog.kakaocdn.net/dn/c0gXYu/btra1ZxLUdc/TUUp93pTWAITpUssGV1Sp0/img.png)

- String.prototype.toUpperCase: 문자열을 모두 대문자로 변경한 문자열 반환
- String.prototype.toLowerCase: 문자열을 모두 소문자로 변경한 문자열 반환
- String.prototype.trim: 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열 반환
- String.prototype.repeat: 인수로 전달받은 정수만큼 반복, 인수 생략 또는 정수가 0이면 빈 문자열 반환, 음수면 RangeError 발생
- String.prototype.replace: 첫 번째 인수로 전달받은 문자열, 정규표현식을 두 번째 인수로 전달한 문자열로 치환한 문자열 반환
- String.prototype.split: 첫 번째 인수로 전달한 문자열, 정규표현식을 구분한 후 분리된 각 문자열로 배열을 반환

    ```jsx
    // 인수로 전달받은 문자열을 역순으로 뒤집는다
    function reverseString(str) {
      return str.split('').reverse().join('');
    }

    reverseString('Hello world!'); // '!dlrow olleH'
    ```
# 📚 33장 7번째 데이터 타입 Symbol
- 심벌 값은 Symbol 함수를 호출하여 생성한다. 다른 원시값, 즉 문자열, 숫자 불리언, undefined, null 타입의 값은 리터럴 표기법을 통해 값을 생성할 수 있지만 심벌 값은 Symbol 함수를 호출하여 생성해야 한다. 다른 값과 절대 중복되지 않는 유일무이한 값이다.

    ```jsx
    // Symbol 함수를 호출하여 유일무이한 심벌 값을 생성한다
    const mySymbol = Symbol();
    console.log(typeof mySymbol); // symbol

    // 심벌 값은 외부로 노출되지 않아 확인할 수 없다
    console.log(mySymbol); // Symbol()
    ```

- Symbol 함수는 String, Number, Boolean 생성자 함수와는 달리 new 연산자와 함께 호출하지 않는다. 선택적으로 문자열을 인수로 전달할 수 있고 심벌 값에 대한 설명으로 디버깅 용도로만 사용되며 심벌값 생성에 어떠한 영향도 주지 않는다. 코드 주석과 비슷한 역할로 콘솔 찍었을 때 편하게 보기 위한 용도이다.

    ```jsx
    new Symbol(); // TypeError: Symbol is not a constructor

    // 심벌 값에 대한 설명이 같더라도 유일무이한 심벌 값을 생성한다
    const mySymbol1 = Symbol('mySymbol');
    const mySymbol2 = Symbol('mySymbol');

    console.log(mySymbol1 === mySymbol2); // false
    ```

- Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다. Symbol.for 메서드를 사용하면 심벌 값을 단 하나만 생성하여 전역 심벌 레지스트리를 통해 공유할 수 있다.

    ```jsx
    // 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
    const s1 = Symbol.for('mySymbol');
    // 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값을 반환
    const s2 = Symbol.for('mySymbol');

    console.log(s1 === s2); // true
    ```

- Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다.

    ```jsx
    // 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
    const s1 = Symbol.for('mySymbol');
    // 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
    Symbol.keyFor(s1); // mySymbol

    // Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
    const s2 = Symbol('foo');
    // 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
    Symbol.keyFor(s2); // undefined
    ```

- 심벌 값을 프로퍼티 키로 사용하려면 프로퍼티 키로 사용할 심벌 값에 대괄호를 사용해야 한다. 프로퍼티에 접근할 때도 마찬가지로 대괄호를 사용해야 한다.

    ```jsx
    const obj = {
      // 심벌 값으로 프로퍼티 키를 생성
      [Symbol.for('mySymbol')]: 1
    };

    obj[Symbol.for('mySymbol')]; // 1
    ```

- 기본적으로 Symbol은 필드를 객체 내부에 숨기기 위해서 사용하고 private 필드라고 직관적으로 알려 줄 수 있다. Symbol을 이용해서 내부 변수를 선언했다면 우연히 중복되게 겹쳐 실수로 내부 필드를 덮어버리는 것을 방지할 수 있다. Symbol을 통해 해당 변수에 추가적인 의미를 줄 수 있고 키 값이 겹쳐서 꼬이는걸 방지해줄 수 있다.
# 📚 34장 이터러블
- ES6에서 도입된 이터레이션 프로토콜(iteration protocol)은 데이터 컬렉션(자료구조)를 순회하기 위한 프로토콜이다. 이터레이션 프로토콜을 준수한 객체는 for...of문으로 순회할 수 있고 Spread 문법, 배열 디스트럭처링 할당의 대상으로 사용할 수 있다. 이터레이션 프로토콜에는 이터러블 프로토콜(interable protocol)과 이터레이터 프로토콜(iterator protocol)이 있다.

    ![https://blog.kakaocdn.net/dn/Y1itO/btra08OEZIL/R36za5DdFxiocR8E2cDwf1/img.png](https://blog.kakaocdn.net/dn/Y1itO/btra08OEZIL/R36za5DdFxiocR8E2cDwf1/img.png)

- 이터러블 프로토콜을 준수한 객체를 이터러블이라 한다. 즉, 이터러블은 Symbol.iterator 메소드를 구현하거나 프로토타입 체인에 의해 상속한 객체를 말한다. Symbol.iterator 메소드는 이터레이터를 반환한다.

    ```jsx
    const array = [1, 2, 3];

    // 배열은 Symbol.iterator 메소드를 소유한다.
    // 따라서 배열은 이터러블 프로토콜을 준수한 이터러블이다.
    console.log(Symbol.iterator in array); // true

    // 이터러블 프로토콜을 준수한 배열은 for...of 문에서 순회 가능하다.
    for (const item of array) {
      console.log(item);
    }
    ```

- 이터레이터 프로토콜은 next 메서드를 소유하며 value, done 프로퍼티를 갖는 이터레이터 리절트(iterator result) 객체를 반환한다.

    ```jsx
    // 배열은 이터러블 프로토콜을 준수한 이터러블이다.
    const array = [1, 2, 3];

    // Symbol.iterator 메소드는 이터레이터를 반환한다.
    const iterator = array[Symbol.iterator]();

    // 이터레이터 프로토콜을 준수한 이터레이터는 next 메소드를 갖는다.
    console.log('next' in iterator); // true

    // 이터레이터의 next 메소드를 호출하면 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다.
    // next 메소드를 호출할 때 마다 이터러블을 순회하며 이터레이터 리절트 객체를 반환한다.
    console.log(iterator.next()); // {value: 1, done: false}
    console.log(iterator.next()); // {value: 2, done: false}
    console.log(iterator.next()); // {value: 3, done: false}
    console.log(iterator.next()); // {value: undefined, done: true}
    ```

- 빌트인 이터러블: Array, String, Map, Set, TypedArray, Arguments, DOM data structure(NodeList, HTMLCollection)

    ```jsx
    // for ... of 문은 이터러블을 순회하면서 이터러블의 요소를 변수에 할당한다
    for (변수선언문 of 이터러블) { ... }
    // for ... of 문은 for ... in 문의 형식과 매우 유사하다
    for (변수선언문 in 객체) { ... }
    ```

- for ... of 문은 내부적으로 이터레이터의 next 메서드를 호출하여 이터러블을 순회하며 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티 값을 for ... of 문의 변수에 할당한다.

    ```jsx
    for (const item of [1, 2, 3]) {
      // item 변수에 순차적으로 1, 2, 3이 할당된다
      console.log(item); // 1 2 3
    }
    ```

- for ... of 문의 내부 동작을 for 문으로 표현하면 다음과 같다.

    ```jsx
    // 이터러블
    const iterable = [1, 2, 3];

    // 이터러블의 Symbol.iterator 메서드를 호출하여 이터레이터 생성
    const iterator = iterable[Symbol.iterator]();

    for (;;) {
      // 이터레이터의 next 메소드를 호출하여 이터러블을 순회한다
      // 이때 next 메서드는 이터레이터 리절트 객체를 반환한다
      const res = iterator.next();

      // next 메소드가 반환하는 이터레이터 리절트 객체의 done 프로퍼티가 true가 될 때까지 반복한다.
      if (res.done) break;
      
      // 이터레이터 리절트 객체의 value 프로퍼티 값을 item 변수에 할당한다.
      const item = res.value;
      console.log(item); // 1 2 3
    }
    ```
    
### 📅 2021년 8월 6일 금요일
# 📚 35장 스프레드 문법
- 스프레드 문법(spread syntax: 전개 문법) ... 은 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만든다. Array, String, Map, Set, DOM 컬렉션(NodeList, HTMLCollection), arguments와 같이 for ... of 문으로 순회할 수 있는 이터러블에 한정된다.
1. 함수 호출문의 인수 목록
    - 스프레드 문법이 제공되기 이전에는 배열을 펼쳐서 요소들의 목록을 함수의 인수로 전달하고 싶은 경우 Function.prototype.apply를 사용했다. 스프레드 문법을 사용하면 더 간결하고 가독성이 좋다.

    ```jsx
    // Math.max 메서드에 숫자가 아닌 배열을 인수로 전달하면 최대값을 구할 수 없다
    Math.max([1, 2, 3]); // NaN

    // Function.prototype.apply
    var arr = [1, 2, 3];
    var max = Math.max.apply(null, arr); // 3

    // 스프레드 문법
    const arr = [1, 2, 3];
    const max = Math.max(...arr); // 3
    ```

    - Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받기 위해 매개변수 이름 앞에 ... 을 붙이는 것이고 스프레드 문법은 여러 개의 값이 하나로 뭉쳐 있는 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만드는 것이다. 따라서 Rest 파라미터와 스프레드 문법은 서로 반대의 개념이다.

    ```jsx
    // Rest 파라미터는 인수들의 목록을 배열로 전달받는다.
    function foo(...rest) {
      console.log(rest); // 1, 2, 3 -> [1, 2, 3]
    }

    // 스프레드 문법은 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만든다.
    // [1, 2, 3] -> 1, 2, 3
    foo(...[1, 2, 3]);
    ```

2. 배열 리터럴 내부
    - 2개의 배열을 1개의 배열로 결합

    ```jsx
    // ES5 concat
    var arr = [1, 2].concat([3, 4]);
    console.log(arr); // [1, 2, 3, 4]

    // ES6 스프레드
    const arr = [...[1, 2], ...[3, 4]];
    console.log(arr); // [1, 2, 3, 4]
    ```

    - 배열 중간에 다른 배열의 요소들을 추가하거나 제거

    ```jsx
    // ES5 splice
    var arr1 = [1, 4];
    var arr2 = [2, 3];
    Array.prototype.splice.apply(arr1, [1, 0].concat(arr2));
    console.log(arr1); // [1, 2, 3, 4]

    // ES6 스프레드
    const arr1 = [1, 4];
    const arr2 = [2, 3];
    arr1.splice(1, 0, ...arr2);
    console.log(arr1); // [1, 2, 3, 4]
    ```

    - 이터러블이 아닌 유사 배열 객체를 배열로 변경하려면 ES6에 도입된 Array.from 메서드를 사용한다.

    ```jsx
    // Array.from은 유사 배열 객체 또는 이터러블을 배열로 변환한다.
    Array.from(arrayLike); // [1, 2, 3]
    ```

3. 객체 리터럴 내부
    - Rest 프로퍼티와 함께 스프레드 프로퍼티를 사용하면 객체 리터럴의 프로퍼티 목록에서도 스프레드 문법을 사용할 수 있다. 스프레드 문법의 대상은 이터러블이어야 하지만 스프레드 프로퍼티 제안은 일반 객체를 대상으로 스프레드 문법의 사용을 허용한다.

    ```jsx
    // 스프레드 프로퍼티가 제안되기 이전에는 ES6에서 도입된 Object.assign 메서드를 사용하여
    // 여러 개의 객체를 병합하거나 특정 프로퍼티를 변경 또는 추가했다.
    const merged = Object.assign({}, {x:1, y:2}, {y:10, z:3});
    console.log(merged); // {x:1, y:10, z:3}

    // 객체 병합. 프로퍼티가 중복되는 경우 뒤에 위치한 프로퍼티가 우선권을 갖는다.
    const merged = { ...{x:1, y:2}, ...{y:10, z:3} };
    console.log(merged); // {x:1, y:10, z:3}
    ```
# 📚 36장 디스트럭처링 할당
- 디스트럭처링 할당(destructuring assignment: 구조 분해 할당)은 구조화된 배열과 같은 이터러블 또는 객체를 destructuring(비구조화, 구조 파괴)하여 1개 이상의 변수에 개별적으로 할당하는 것을 말한다. 배열과 같은 이터러블 또는 객체 리터럴에서 필요한 값만 추출하여 변수에 할당할 때 유용하다.
1. 배열 디스트럭처링 할당
    - 배열 디스트럭처링 할당의 기준은 배열의 인덱스다. 즉, 순서대로 할당된다. 이때 변수의 개수와 이터러블의 요소 개수가 반드시 일치할 필요는 없다. 배열 디스트럭처링 할당을 위한 변수에 기본값을 설정할 수 있고 기본값보다 할당된 값이 우선된다.

    ```jsx
    const [a, b] = [1, 2];
    console.log(a, b); // 1 2

    const [c, d] = [1];
    console.log(c, d); // 1 undefined

    const [e, f] = [1, 2, 3];
    console.log(e, f); // 1 2

    const [g, , h] = [1, 2, 3];
    console.log(g, h); // 1 3

    const [a, b, c = 3] = [1, 2];
    console.log(a, b, c); // 1 2 3

    const [e, f = 10, g = 3] = [1, 2];
    console.log(e, f, g); // 1, 2, 3
    ```

2. 객체 디스트럭처링 할당
    - 객체 디스트럭처링 할당의 기준은 프로퍼티 키다. 즉, 순서는 의미가 없으며 선언된 변수 이름과 프로퍼티 키가 일치하면 할당된다. 객체 리디스트럭처링 할당을 위한 변수에 기본값을 설정할 수 있고 객체를 인수로 전달받는 함수의 매개변수에도 사용할 수 있다.

    ```jsx
    const { lastName, firstName } = { firstName: 'Ungmo', lastName: 'Lee' };

    const { lastName, firstName } // SyntaxError
    const { lastName, firstName } = null; // TypeError

    const user = { firstName: 'Ungmo', lastName: 'Lee' };
    const { lastName: ln, firstName: fn } = user;
    console.log(fn, ln); // Ungmo Lee

    const { firstName = 'Ungmo', lastName } = { lastName: 'Lee'};
    console.log(firstName, lastName); // Ungmo Lee

    function printTodo({ content, completed }) {
      console.log(`할일 ${content}은 ${completed ? '완료' : '비완료'} 상태입니다.`);
    }

    printTodo({ id: 1, content: 'HTML', completed: true }); // 할일 HTML은 완료 상태입니다.
    ```

    - 배열의 요소가 객체인 경우 배열 디스트럭처링 할당과 객체 디스트럭처링 할당을 혼용할 수 있다.

    ```jsx
    const todos = [
      { id: 1, content: 'HTML', completed: true },
      { id: 2, content: 'CSS', completed: false },
      { id: 3, content: 'JS', completed: false }
    ];

    // todos 배열의 두 번째 요소인 객체로부터 id 프로퍼티만 추출한다.
    const [, { id }] = todos;
    console.log(id); // 2

    const [, , { content }] = todos;
    console.log(content); // JS
    ```
# 📚 37장 Set과 Map
> 💡 Set

- Set 객체는 중복되지 않는 유일한 값들의 집합(set)이다. Set 객체는 배열과 유사하지만 다음과 같은 차이가 있다.

    ![https://blog.kakaocdn.net/dn/eudQxd/btrbllgdzNn/BvOgNJMsyKu0I5I1DwBVE1/img.png](https://blog.kakaocdn.net/dn/eudQxd/btrbllgdzNn/BvOgNJMsyKu0I5I1DwBVE1/img.png)

- Set 객체는 Set 생성자 함수로 생성하고 이터러블을 인수로 전달받아 생성한다. 이때 이터러블의 중복된 값은 Set 객체에 요소로 저장되지 않으므로 배열에서 중복된 요소를 제거할 수 있다.

    ```jsx
    const set1 = new Set([1, 2, 3, 3]);
    console.log(set1); // Set(3) {1, 2, 3}

    // Set.prototype.size: Set 객체의 요소 개수를 확인
    const { size } = new Set([1, 2, 3, 3]);
    console.log(size); // 3

    // Set.prototype.add: Set 객체에 요소 추가
    const set = new Set();
    console.log(set); // Set(0) {}
    set.add(1);
    console.log(set); // Set(1) {1}
    set.add(2).add(2).add(3); // 중복 무시
    console.log(set); // Set(3) {1, 2, 3}

    // Set.prototype.has: Set 객체 특정 요소 존재 여부 확인
    const set = new Set([1, 2, 3]);
    console.log(set.has(2)); // true
    console.log(set.has(4)); // false

    // Set.prototype.delete: Set 객체 특정 요소 삭제
    const set = new Set([1, 2, 3]);
    set.delete(2);
    console.log(set); // Set(2) {1, 3}
    set.delete(1).(delete(3); // TypeError 연속 호출 불가능

    // Set.prototype.clear: Set 객체 요소 일괄 삭제
    set.clear();
    console.log(set); // Set(0) {}
    ```

- Set 객체의 요소를 순회하려면 Set.prototype.forEach 메서드를 사용한다. 첫 번째 인수와 두 번째 인수는 같은 값인데 Array.prototype.forEach 메서드와 인터페이스를 통일하기 위함이며 다른 의미는 없고 콜백 함수는 두 번째 인수로 현재 순회 중인 요소의 인덱스를 전달받지만 Set 객체는 순서에 의미가 없어 배열과 같이 인덱스를 갖지 않는다.

    ```jsx
    // Set.prototype.forEach(현재순회중인요소값, 현재순회중인요소값,현재순회중인 Set객체자체)
    const set = new Set([1, 2, 3]);
    set.forEach((v, v2, set) => console.log(v, v2, set));
    /*
    1 1 Set(3) {1, 2, 3}
    2 2 Set(3) {1, 2, 3}
    3 3 Set(3) {1, 2, 3}
    */
    ```

- Set 객체는 이터러블이므로 for ... of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수도 있다.
- Set 객체는 수학적 집합을 구현하기 위한 자료구조다. 교집합 intersection, 합집합 union, 차집합 difference, 부분집합과 상위집합 isSuperset을 통해 확인이 가능하다.

> 💡 Map

- Map 객체는 키와 값의 쌍으로 이루어진 컬렉션이다. Map 객체는 객체와 유사하지만 다음과 같은 차이가 있다.

    ![https://blog.kakaocdn.net/dn/cJyjqF/btrbuIAExKl/ur6unsUJ04oVddaLuTwfT0/img.png](https://blog.kakaocdn.net/dn/cJyjqF/btrbuIAExKl/ur6unsUJ04oVddaLuTwfT0/img.png)

- 생성자 함수는 이터러블을 인수로 전달받아 Map 객체를 생성한다. 이때 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 한다.

    ```jsx
    const map1 = new Map([['key1', 'value1'], ['key2', 'value2']]);
    console.log(map1); // Map(2) {"key1" => "value1", "key2" => "value2"}

    // 중복된 키를 갖는 요소가 존재하면 값이 덮어써진다. (Map 객체 중복된 키 요소 존재 불가)
    const map = new Map([['key1', 'value1'], ['key1', 'value2']]);
    console.log(map); // Map(1) {"key1" => "value2"}

    // Map.prototype.size: 요소 개수 확인
    const { size } = new Map([['key1', 'value1'], ['key2', 'value2']]);
    console.log(size); // 2

    // Map.prototype.set: 요소 추가
    const map = new Map();
    consol.log(map); // Map(0) {}
    map.set('key1', 'value1');
    consol.log(map); // Map(1) {"key1" => "value1"}

    // Map.prototype.get: 요소 취득
    const map = new Map();
    const lee = {name: 'Lee'};
    const kim = {nameL 'Kim'};
    // 연속적으로 호출(method chaning)가능
    map
      .set(lee, 'developer')
      .set(kim, 'designer');
    console.log(map.get(lee)); // developer
    console.log(map.get('key')); // undefined

    // Map.prototype.has: 요소 존재 여부 확인
    console.log(map.has(lee)); // true
    console.log(map.has('key')); // false

    // Map.prototype.delete: 요소 삭제 (연속 호출 불가)
    map.delete(kim);
    console.log(map); // Map(1) { {name: "Lee"} => "developer" }

    // Map.prototype.clear: 요소 일괄 삭제
    map.clear();
    console.log(map); // Map(0) {}
    ```

- Map 객체의 요소를 순회하려면 Map.prototype.forEach 메서드를 사용한다.

    ```jsx
    // Map.prototype.forEach(현재순회중인요소값, 현재순회중인요소키, 현재순회중인 Map객체자체)
    const lee = { name: 'Lee' };
    const kim = { name: 'Kim' };
    const map = new Map([[lee, 'developer'], [kim, 'designer']]);

    map.forEach((v, k, map) => console.log(v, k, map));
    /*
    developer {name: "Lee"} Map(2) {
      {name: "Lee"} => "developer",
      {name: "Kim"} => "designer"
    }
    designer {name: "Kim"} Map(2) {
      {name: "Lee"} => "developer",
      {name: "Kim"} => "Desinger"
    }
    */
    ```

- Map 객체는 이터러블이므로 for ... of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수도 있다.
- Map 객체는 이터러블이면서 동시에 이터레이션 객체를 반환하는 메서드를 제공한다.

    ```jsx
    const lee = { name: 'Lee' }
    const kim = { name: 'Kim' }

    const map - new Map([[lee, 'developer'], [kim, 'designer']]);

    // Map.prototype.keys: Map 객체 요소키 반환
    for (const key of map.keys()) {
      console.log(key); // {name: "Lee"} {name: "Kim"}
    }

    // Map.prototype.values: Map 객체 요소값 반환
    for (const value of map.values()) {
      console.log(value); // developer designer
    }

    // Map.prototype.entries: Map 객체 요소키와 요소값 반환
    for (const entry of map.entries()) {
      console.log(entry); // [{name: "Lee"}, "developer"] [{name: "Kim", "designer"]
    }
    ```
