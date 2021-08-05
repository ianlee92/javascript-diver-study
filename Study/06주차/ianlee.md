### 📅 2021년 7월 13일 화요일
# 📚 22장 this
- this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수(self-referencing variable)다. this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.

    ```jsx
    // this는 어디서든지 참조 가능하다.
    // 전역에서 this는 전역 객체 window를 가리킨다.
    console.log(this); // window

    function squre(number) {
      // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
      console.log(this); // window
      return number * number;
    }
    square(2);

    const person = {
      name: 'Lee',
      getName() {
        // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
        console.log(this); // {name: "Lee", getName: f} 프로퍼티, 메서드
        return this.name;
      }
    };
    console.log(person.getName()); // Lee

    function Person(name) {
      this.name = name;
      // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
      console.log(this); // Person {name: "Lee"}
    }

    const me = new Person('Lee');
    ```

> 💡  함수 호출 방식에 따라 this 바인딩은 동적으로 결정된다.

1. 일반 함수 호출
    - 일반 함수로 호출된 모든 함수(중첩 함수와 콜백 함수 포함) 내부의 this에는 전역 객체가 바인딩된다. 즉, 내부함수는 일반 함수, 메소드, 콜백함수 어디에서 선언되었든 관게없이 this는 전역객체를 바인딩한다. 또는 화살표 함수를 사용해서 this 바인딩을 일치시킬 수도 있다. 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.

        ```jsx
        var value = 1;

        var obj = {
          value: 100,
          foo: function() {
            console.log("foo's this: ",  this);  // obj
            console.log("foo's this.value: ",  this.value); // 100
            function bar() {
              console.log("bar's this: ",  this); // window
              console.log("bar's this.value: ", this.value); // 1
            }
            bar();
          }
        };

        obj.foo();
        ```

        ```jsx
        var value = 1;

        var obj = {
          value: 100,
          foo: function() {
            setTimeout(function() {
              console.log("callback's this: ",  this);  // window
              console.log("callback's this.value: ",  this.value); // 1
            }, 100);
          }
        };

        obj.foo();
        ```

2. 메서드 호출
    - 함수가 객체의 프로퍼티 값이면 메서드로서 호출된다. 이때 메서드 내부의 this는 해당 메서드를 소유한 객체, 즉 해당 메서드를 호출한 객체에 바인딩된다.

    ```jsx
    var obj1 = {
      name: 'Lee',
      sayName: function() {
        console.log(this.name);
      }
    }

    var obj2 = {
      name: 'Kim'
    }

    obj2.sayName = obj1.sayName;

    obj1.sayName(); // Lee
    obj2.sayName(); // Kim
    ```

3. 생성자 함수 호출
    - 생성자 함수 내부의 this에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩된다.

    ```jsx
    // 생성자 함수
    function Circle(radius) {
      // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
      this.radius = radius;
      this.getDiameter = function () {
        return 2 * this.radius;
      };
    }

    // 반지름이 5인 Circle 객체를 생성
    const circle1 = new Circle(5);
    // 반지름이 10인 Circle 객체를 생성
    const circle2 = new Circle(10);

    console.log(circle1.getDiameter()); // 10
    console.log(circle2.getDiameter()); // 20
    ```

4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
    - apply와 call 메서드의 본질적인 기능은 함수를 호출하는 것이다. apply와 call 메서드는 함수를 호출하면서 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩한다.

    ```jsx
    let person1 = {
        name: 'Jo'
    };

    let person2 = {
        name: 'Kim',
        study: function() {
            console.log(this.name + '이/가 공부를 하고 있습니다.');
        }
    };

    person2.study(); // Kim이/가 공부를 하고 있습니다.

    // call()
    person2.study.call(person1); // Jo이/가 공부를 하고 있습니다.
    ```

    - apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달하고 call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다. 이처럼 apply와 call 메서드는 호출할 함수에 인수를 전달하는 방식만 다를 뿐 this로 사용할 객체를 전달하면서 함수를 호출하는 것은 동일하다.

    ```jsx
    function foo(a, b, c) {
      console.log(a + b + c);
    };
    foo(1, 2, 3); // 6
    foo.call(null, 1, 2, 3); // 6
    foo.apply(null, [1, 2, 3]); // 6
    ```

    - bind 메서드는 apply와 call 메서드와 달리 함수를 호출하지 않고 this로 사용할 객체만 전달한다.

    ```jsx
    let person1 = {
        name: 'Jo'
    };

    let person2 = {
        name: 'Kim',
        study: function() {
            console.log(this.name + '이/가 공부를 하고 있습니다.');
        }
    };

    person2.study(); // Kim이/가 공부를 하고 있습니다.

    // bind()
    let student = person2.study.bind(person1);

    // bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
    student(); // Jo이/가 공부를 하고 있습니다.
    ```
# 📚 23장 실행 컨텍스트
- 자바스크립트 엔진은 먼저 전역 코드를 평가하여 전역 실행 컨텍스트를 생성하고 함수가 호출되면 함수 코드를 평가하여 함수 실행 컨텍스트를 생성한다. 이때 생성된 실행 컨텍스트는 스택(stack) 자료구조로 관리된다. 이를 실행 컨텍스트 스택이라고 부른다. 아래 코드를 실행하면 코드가 실행되는 시간의 흐름에 따라 실행 컨텍스트 스택에는 다음과 같이 실행 컨텍스트가 추가(push)되고 제거(pop)된다.

    ```jsx
    const x = 1;

    function foo () {
      const y = 2;
      
      function bar () {
        const z = 3;
        console.log(x + y + z);
      }
      bar();
    }

    foo(); // 6
    ```

    ![https://blog.kakaocdn.net/dn/lanmO/btq9rddDuMy/odraRbecnwtUfsXJkvmkj1/img.png](https://blog.kakaocdn.net/dn/lanmO/btq9rddDuMy/odraRbecnwtUfsXJkvmkj1/img.png)

    - 전역 코드의 평가와 실행 -> foo 함수 코드의 평가와 실행 -> bar 함수 코드의 평가와 실행 -> foo 함수 코드로 복귀 -> 전역 코드로 복귀

> 💡  렉시컬 환경(Lexical Environment)

- 렉시컬 환경은 식별자와 식별자에 바인딩된 값, 그리고 상위 스코프에 대한 참조를 기록하는 자료구조로 실행 컨텍스트를 구성하는 컴포넌트다. 실행 컨텍스트 스택이 코드의 실행 순서를 관리한다면 렉시컬 환경은 스코프와 식별자를 관리한다.
- 렉시컬 환경은 환경 레코드(Environment Record)와 외부 렉시컬 환경에 대한 참조(Outer Lexical Environment Reference) 두 개의 컴포넌트로 구성된다. 환경 레코드는 스코프에 포함된 식별자를 등록하고 등록된 식별자에 바인딩된 값을 관리하는 저장소이며, 외부 렉시컬 환경에 대한 참조는 상위 스코프를 가리킨다. 즉, 해당 실행 컨텍스트를 생성한 소스코드를 포함하는 상위 코드의 렉시컬 환경을 말한다.

```jsx
var x = 1;
const y = 2;

function foo (a) {
  var x = 3;
  const y = 4;
  
  function bar (b) {
    const z = 5;
    console.log(a + b + x + y + z);
  }
  bar(10);
}

foo(20); // 42
```

전역 객체 생성 → 전역 코드 평가(전역 실행 컨텍스트 생성 -> 전역 렉시컬 환경 생성 -> 전역 환경 레코드 생성 -> 객체 환경 레코드 생성 -> 선언적 환경 레코드 생성 -> this 바인딩 -> 외부 렉시컬 환경에 대한 참조 결정) → 전역 코드 실행 → foo 함수 코드 평가( 함수 실행 컨텍스트 생성 -> 함수 렉시컬 환경 생성 -> 함수 환경 레코드 생성 -> this 바인딩 -> 외부 렉시컬 환경에 대한 참조 결정) → foo 함수 코드 실행 → bar 함수 코드 평가 → bar 함수 코드 실행 (console 식별자 검색 -> log 메서드 검색 -> 표현식 a + b + x + y + z의 평가 -> console.log 메서드 호출) → bar 함수 코드 실행 종료 → foo 함수 코드 실행 종료 →  전역 코드 실행 종료


### 📅 2021년 7월 15일 목요일
# 📚 24장 클로저
- 자바스크립트가 렉시컬 스코프를 따르는 프로그래밍 언어이기 때문에 중첩함수가 아니라면 다른 함수의 변수에 접근할 수 없다.
- 렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 저장할 참조값, 즉 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 의해 결정된다. 이것이 바로 렉시컬 스코프다.
- 함수는 자신의 내부 슬롯 [[Environment]]에 자신이 정의된 환경, 즉 상위 스코프의 참조를 저장한다.
- 함수 객체의 내부 슬롯 [[Environment]]에 저장된 현재 실행 중인 실행 컨텍스트의 렉시컬 환경의 참조가 바로 상위 스코프다. 또한 자신이 호출되었을 때 생성될 함수 렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 저장될 참조값이다. 함수 객체는 내부 슬롯 [[Environment]]에 저장한 렉시컬 환경의 참조, 즉 상위 스코프를 자신이 존재하는 한 기억한다.

```jsx
const x = 1;

// (1)
function outer() {
  const x = 10;
  const inner = function() {console.log(x);}; // (2)
  return inner;
}

// outer 함수를 호출하면 중첩 함수 inner를 반환한다.
// 그리고 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거된다.
const innerFunc = outer(); // (3)
innerFunc(); // (4) 10
```

- 외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다. 이러한 중첩 함수를 클로저(closure)라고 부른다
- outer 함수의 실행 컨텍스트가 실행 컨텍스트 스택에서 제거되지만 outer 함수의 렉시컬 환경까지 소멸하는 것은 아니다. outer 함수의 렉시컬 환경은 inner 함수의 [[Environment]] 내부 슬롯에 의해 참조되고 있고 inner 함수는 전역 변수 innerFunc에 의해 참조되고 있으므로 가비지 컬렉션의 대상이 되지 않는다.
- 클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적이다.
- 클로저는 상태(state)를 안전하게 변경하고 유지하기 위해 사용한다. 다시 말해, 상태가 의도치 않게 변경되지 않도록 상태를 안전한게 은닉(information hiding)하고 특정 함수에게만 상태 변경을 허용하기 위해 사용한다.

```jsx
// 함수를 인수로 전달받고 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다
function makeCounter(predicate) {
  // 카운트 상태를 유지하기 위한 자유 변수
  let counter = 0;
  
  // 클로저를 반환
  return function () {
    // 인수로 전달받은 보조 함수에 상태 변경을 위임한다.
    counter = predicate(counter);
    return counter;
  };
}

// 보조 함수
function increase(n) {
  return ++n;
}

// 보조 함수
function decrease(n) {
  return --n;
}

// 함수로 함수를 생성한다.
// makeCounter 함수는 보조 함수를 인수로 전달받아 함수를 반환한다.
const increaser = makeCounter(increase); // (1)
console.log(increaser()); // 1
console.log(increaser()); // 2

// increaser 함수와는 별개의 독립된 렉시컬 환경을 갖기 때문에 카운터 상태가 연동하지 않는다.
const decreaser = makeCounter(decrease); // (2)
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

- makeCounter 함수가 반환한 함수는 makeCounter 함수의 렉시컬 환경을 상위 스코프로서 기억하는 클로저이며, 전역 변수인 increaser, decreaser에 할당된다. 이때 makeCounter 함수의 실행 컨텍스트는 소멸되지만 makeCounter 함수 실행 컨텍스트의 렉시컬 환경은 makeCounter 함수가 반환한 함수의 [[Environment]] 내부 슬롯에 의해 참조되고 있기 때문에 소멸되지 않는다.
- 전역 변수 increaser와 decreaser에 할당된 함수는 각각 자신만의 독립된 렉시컬 환경을 갖기 때문에 카운트를 유지하기 위한 자유 변수 counter를 공유하지 않아 카운터의 증감이 연동되지 않기 때문에 렉시컬 환경을 공유하는 클로저를 만들어야 한다. 이를 위해서는 makeCounter 함수를 두 번 호출하지 말아야 한다.

```jsx
// 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다.
const counter = (function() {
  // 카운트 상태를 유지하기 위한 자유 변수
  let counter = 0;
  
  // 함수를 인수로 전달받는 클로저를 반환
  return function (predicate) {
    // 인수로 전달받은 보조 함수에 상태 변경을 위임한다.
    counter = predicate(counter);
    return counter;
  };
}

// 보조 함수
function increase(n) {
  return ++n;
}

// 보조 함수
function decrease(n) {
  return --n;
}

// 보조 함수를 전달하여 호출
console.log(counter(increase); // 1
console.log(counter(increase); // 2

// 자유 변수를 공유한다.
console.log(counter(decrease); // -1
console.log(counter(decrease); // -2
```

- 캡슐화(encapsulation)는 객체의 상태(state)를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작(behavior)인 메서드를 하나로 묶는 것을 말한다. 캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보 은닉(information hiding)이라 한다.
- 자바스크립트는 정보 은닉을 완전하게 지원하지 않는다. 인스턴스 메서드를 사용한다면 자유 변수를 통해 private을 흉내 낼 수는 있지만 프로토타입 메서드를 사용하면 이마저도 불가능해진다. ES6의 Symbol 또는 WeakMap을 사용하여 private한 프로퍼티를 흉내 내기도 했으나 근본적으로 해결책이 되지는 않는다. 클래스에 private 필드를 정의할 수 있는 새로운 표준 사양이 제안되어 있다.
