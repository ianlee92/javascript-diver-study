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
