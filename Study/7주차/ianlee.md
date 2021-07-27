### 📅 2021년 7월 20일 화요일
# 📚 25장 클래스 (~p447)
- 클래스를 프로토타입 기반 객체 생성 패턴의 단순한 문법적 설탕이라고 보기보다는 새로운 객체 생성 메커니즘으로 보는 것이 좀 더 합당하다.
- 클래스는 class 키워드를 사용하여 정의한다. 클래스 몸체에서 정의할 수 있는 메서드는 constructor(생성자), 프로토타입 메서드, 정적 메서드의 세 가지가 있다.
- 클래스 선언문으로 정의한 클래스는 함수 선언문과 같이 소스코드 평가 과정, 즉 런타임 이전에 먼저 평가되어 함수 객체를 생성한다.
- 클래스 선언문도 변수 선언, 함수 정의와 마찬가지로 호이스팅이 발생한다. 단, 클래스는 let, const 키워드로 선언한 변수처럼 호이스팅된다. var, let, const, function, function*(generator), class 키워드를 사용하여 선언된 모든 식별자는 호이스팅된다. 모든 선언문은 런타임 이전에 먼저 실행되기 때문이다.
- 클래스는 생성자 함수이며 new 연산자와 함께 호출되어 인스턴스를 생성한다.

```jsx
class Person {}

// 인스턴스 생성
const me = new Person();
console.log(me); // Person {}
```

- constructor는 인스턴스를 생성하고 초기화하기 위한 특수한 메서드다. constructor는 이름을 변경할 수 없다.

```jsx
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
}
```

```jsx
// 인스턴스 생성
cosnt me = new Person('Lee');
console.log(me);
```

- 생성자 함수와 마찬가지로 constructor 내부에서 this에 추가한 프로퍼티는 인스턴스 프로퍼티가 된다. constructor 내부의 this는 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스를 가리킨다.
- constructor는 클래스 내에 최대 한 개만 존재할 수 있고 2개 이상이 포함되면 문법 에러가 발생한다. constructor는 생략할 수 있고 생략하면 빈 constructor가 암묵적으로 정의된다.
- constructor 내에서는 인스턴스의 생성과 동시에 인스턴스 프로퍼티 추가를 통해 인스턴스의 초기화를 실행한다. 따라서 인스턴스를 초기화하려면 Constructor를 생략해서는 안된다.

```jsx
class Person {
  constructor(name, address) {
    // 인수로 인스턴스 초기화
    this.name = name;
    this.address = address;
  }
}

// 인수로 초기값을 전달한다. 초기값은 constructor에 전달된다.
const me = new Person('Lee', 'Seoul');
console.log(me); // Person {name: "Lee", address: "Seoul"}
```

- consturctor 내부에서 명시적으로 this가 아닌 다른 값을 반환하는 것은 클래스의 기본 동작을 훼손한다. 따라서 constructor 내부에서 return 문을 반드시 생략해야 한다.
- 클래스 몸체에서 정의한 메서드는 생성자 함수에 의한 객체 생성 방식과는 다르게 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 된다. 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 되고 프로토타입 체인을 생성한다.
- 클래스는 생성자 함수와 같이 인스턴스를 생성하는 생성자 함수라고 볼 수 있다. 다시 말해, 클래스는 생성자 함수와 마찬가지로 프로토타입 기반의 객체 생성 메커니즘이다.

```jsx
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
  
  // 프로토타입 메서드
  sayHi() {
    console.log(`My name is ${this.name}`);
  }
}
  
const me = new Person('Lee');
me.sayHi(); // My name is Lee
```

- 클래스에서는 메서드에 static 키워드를 붙이면 정적 메서드(클래스 메서드)가 된다. 정적 메서드는 클래스에 바인딩된 메서드가 되고 클래스는 함수 객체로 평가되므로 자신의 프로퍼티/메서드를 소유할 수 있다.

```jsx
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
  
  // 정적 메서드
  static sayHi() {
    console.log('Hi!');
  }
}

// 정적 메서드는 인스턴스 없이 클래스로 호출한다
Person.sayHi(); // Hi!
```

- 정적 메서드와 프로토타입 메서드는 자신이 속해 있는 프로토타입 체인이 다르고, 정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출하며, 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

```jsx
class Square {
  // 정적 메서드
  static area(width, height) {
    retrun width * height;
  }
}

console.log(Square.area(10, 10)); // 100
```

```jsx
class Square {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }
  
  // 프로토타입 메서드
  area() {
    return this.width * this.height;
  }
}

const sqaure = new Square(10, 10);
console.log(square.area()); // 100
```

- 프로토타입 메서드는 인스턴스로 호출해야 하므로 프로토타입 메서드 내부의 this는 프로토타입 메서드를 호출한 인스턴스를 가리키고, 정적 메서드는 클래스로 호출해야 하므로 정적 메서드 내부의 this는 인스턴스가 아닌 클래스를 가리킨다. 즉, 프로토타입 메서드와 정적 메서드 내부의 this 바인딩이 다르다. 따라서 메서드 내부에서 인스턴스 프로퍼티를 참조할 필요가 있다면 this를 사용해야 하며, 이러한 경우 프로토타입 메서드로 정의해야 한다. this를 사용하지 않는 메서드는 정적 메서드로 정의하는 것이 좋다.
- 정적 메서드는 애플리케이션 전역에서 사용할 유틸리티 함수를 전역 함수로 정의하지 않고 메서드로 구조화할 때 유용하다.

> 💡  private 필드 정의 제안

- constructor 내부에서 this에 추가한 프로퍼티는 언제나 클래스가 생성한 인스턴스의 프로퍼티가 된다. ES6의 클래스는 다른 객체지향 언어처럼 private, public, protected 키워드와 같은 접근 제한자(access modifier)를 지원하지 않는다. 따라서 인스턴스 프로퍼티는 언제나 public하다.
- private 필드의 선두에는 #을 붙여준다. private 필드를 참조할 때도 #을 붙어주어야 한다.
- public 필드는 어디서든 참조할 수 있지만 private 필드는 클래스 내부에서만 참조할 수 있다. (자식 클래스 내부, 클래스 인스턴스를 통한 접근으로는 불가)

```jsx
class Person {
  // private 필드 정의는 클래스 몸체에서 해야 한다
  #name = '';
  
  constructor(name) {
    // private 필드 참조
    this.#name = name;
  }
}

const me = new Person('Lee');

// private 필드 #name은 클래스 외부에서 참조할 수 없다.
console.log(me.#name);
// SyntaxError: Private field '#name' must be declared in an enclosing class
```

### 📅 2021년 7월 22일 목요일
# 📚 25장 클래스 (p448~)
- 프로토타입 기반 상속은 프로토타입 체인을 통해 다른 객체의 자산을 상속받는 개념이지만 상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장(extends)하여 정의하는 것이다.
- 상속을 통해 클래스를 확장하려면 extends 키워드를 사용하여 상속받을 클래스를 정의한다.

```jsx
// 수퍼(베이스/부모)클래스
class Base {}

// 서브(파생/자식)클래스
class Derived extends Base {}
```

- 수퍼클래스와 서브클래스는 인스턴스의 프로토타입 체인뿐만 아니라 클래스 간의 프로토타입 체인도 생성한다. 이를 통해 프로토타입 메서드, 정적 메서드 모두 상속이 가능하다.
- 매개변수 ...을 붙이면 Rest 파라미터가 된다. Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.
- super를 호출하면 수퍼클래스의 constructor(super-constructor)를 호출한다.

```jsx
// 수퍼클래스
class Base {
  constructor(a, b) {
    this.a = a;
    this.b = b;
  }
}

// 서브클래스
class Derived extends Base {
  // 다음과 같이 암묵적으로 constructor가 정의된다.
  // constructor(...args) {super(...args);}
}

const derived = new Derived(1, 2);
console.log(derived); // Derived {a: 1, b: 2}
```

- 수퍼클래스에서 추가한 프로퍼티와 서브클래스에서 추가한 프로퍼티를 갖는 인스턴스를 생성한다면 서브클래스의 constructor를 생략할 수 없다. 이때 new 연산자와 함께 서브클래스를 호출하면서 전달한 인수 중에서 수퍼클래스의 constructor에 전달할 필요가 있는 인수는 서브클래스의 constructor에서 호출하는 super를 통해 전달한다.

> 💡 super를 호출할 때 주의할 사항

1. 서브클래스에서 constructor를 생략하지 않는 경우 서브클래스의 constructor에는 반드시 super를 호출해야 한다.
2. 서브클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없다.
3. super는 반드시 서브클래스의 constructor에서만 호출한다. 서브클래스가 아닌 클래스의 constructor나 함수에서 super를 호출하면 에러가 발생한다.
- 메서드 내에서 super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.

```jsx
// 수퍼클래스
class Base {
  constructor(name) {
    this.name = name;
  }
  
  sayHi() {
    return `Hi! ${this.name}`;
  }
}

//
class Derived extends Base {
  sayHi() {
    // super.sayHi는 수퍼클래스의 프로토타입 메서드를 가리킨다.
    return `${super.sayHi()}. how are you doing?`;
  }
}

const derived = new Derived('Lee');
console.log(derived.sayHi()); // Hi! Lee. how are you doing?
```

- 주의할 것은 ES6의 메서드 축약 표현으로 정의된 함수만이 [[HomeObject]]를 갖는다는 것이다. 메서드는 내부 슬롯 [[HomeObject]]를 가져야 자신을 바인딩하고 있는 객체를 가리킬 수 있다. 즉, [[HomeObject]]를 가지는 함수만이 super 참조를 할 수 있다.

```jsx
const obj = {
  // foo는 ES6의 메서드 축약 표현으로 정의한 메서드다. 따라서 [[HomeObject]]를 갖는다.
  foo() {},
  
  // bar는 일반함수로 [[HomeObject]]를 갖지 않는다.
  bar: fucntion () {}
};
```

> 💡 상속 클래스의 인스턴스 생성 과정

서브클래스의 super 호출 ⇒ 수퍼클래스의 인스턴스 생성과 this 바인딩 ⇒ 수퍼클래스의 인스턴스 초기화 ⇒ 서브클래스 constructor로의 복귀와 this바인딩 ⇒ 서브클래스의 인스턴스 초기화 ⇒ 인스턴스 반환

```jsx
// 수퍼클래스
class Rectangle {
  // 서브클래스가 new 연산자와 함께 호출되면 서브클래스 constructor 내부의 super 키워드 호출시
  // 수퍼클래스의 constructor(super-construcotr)가 호출된다.
  constructor(width, height) {
    // 2. 수퍼클래스의 인스턴스 생성과 this 바인딩
    // 암묵적으로 빈 객체, 즉 인스턴스가 생성되고 this에 바인딩된다.
    console.log(this); // ColorRectangle {}
    
    // 3. 수퍼클래스의 인스턴스 초기화
    this.width = width;
    this.height = height;
  }
  
  getArea() {
    return this.width * this.height;
  }
  
  toString() {
    return `width ${this.width}, height = ${this.height}`;
  }
}

// 서브클래스
class ColorRectangle extends Rectangle {
  constructor(width, height, color) {
   // 1. 서브클래스의 super 호출
   // 서브클래스는 자신이 직접 인스턴스를 생성하지 않고 수퍼클래스에게 인스턴스 생성 위임
   // 이것이 바로 서브클래스의 constructor에서 반드시 super를 호출해야 하는 이유
    super(width, height);
    // 4. 서브클래스 constructor로의 복귀와 this 바인딩
    // super가 반환한 인스턴스가 this에 바인딩된다.
    console.log(this); // ColorRectangle {width: 2, height: 4}
    
    // 5. 서브클래스의 인스턴스 초기화
    this.color = color;
    // 6. 인스턴스 반환
    // 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
    console.log(this); // ColorRectangle {width: 2, height: 4, color: "red"}
  }
  
  // 메서드 오버라이딩
  toString() {
    return super.toString() + `, color = ${this.color}`;
  }
}

const colorRectangle = new ColorRectangle(2, 4, 'red');
console.log(colorRectangle); // ColorRectangle {width: 2, height: 4, color: "red"}

// 상속을 통해 getArea 메서드를 호출
console.log(colorRectangle.getArea()); // 8
// 오버라이딩된 toString 메서드를 호출
console.log(colorRectangle.toString()); // width = 2, height = 4, color = red
```
# 📚 26장 ES6 함수의 추가 기능
- 호출할 수 있는 함수 객체를 collable이라 하며, 인스턴스는 생성할 수 있는 함수 객체를 constructor, 인스턴스를 생성할 수 없는 함수 객체를 non-constructor라고 부른다.
- ES6 이전의 모든 함수는 사용 목적에 따라 명확한 구분이 없으므로 호출 방식에 특별한 제약이 없고 생성자 함수로 호출되지 않아도 프로토타입 객체를 생성했고 이는 혼란스러우며 실수를 유발할 가능성이 있고 성능에도 좋지 않았고 이러한 문제를 해결하기 위해 ES6에서는 함수를 사용 목적에 따라 일반 함수, 메서드, 화살표 함수 세 가지 종류로 명확히 구분했다.
- 일반 함수는 constructor이지만 ES6의 메서드와 화살표 함수는 non-constructor다.
- ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다. ES6 사양에서 정의한 메서드는 인스턴스를 생성할 수 없는 non-constructor다. 따라서 ES6 메서드는 생성자 함수로서 호출할 수 없고 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.

    ```jsx
    const obj = {
      x: 1,
      // foo는 메서드다.
      foo() { return this.x; },
      // bar에 바인딩된 함수는 메서드가 아닌 일반 함수다.
      bar: function() { return this.x; }
    };

    console.log(obj.foo()); // 1
    console.log(obj.bar()); // 1

    new obj.foo(); // TypeError: obj.foo is not a constructor
    new obj.bar(); // bar {}

    obj.foo.hasOwnProperty('prototype'); // false
    obj.bar.hasOwnProperty('prototype'); // true
    ```

- ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]]를 갖기 때문에 super키워드를 사용할 수 있다.
- ES6 메서드는 본연의 기능(super)을 추가하고 의미적으로 맞지 않는 기능(constructor)은 제거했다.

> 💡 화살표 함수

- 화살표 함수(arrow function)는 function 키워드 대신 화살표(=>, fat arrow)를 사용하여 기존의 함수 정의 방식보다 간략하게 함수를 정의할 수 있다. 내부동작도 기존의 함수보다 간략하고 콜백 함수 내부에서 this가 전역 객체를 가리키는 문제를 해결하기 위한 대안으로 유용하다.
- 화살표 함수도 일급 객체이므로 Array.prototype.map, filter, reduce같은 고차 함수(Higher-Order Function, HOF)에 인수로 전달할 수 있다. 이 경우 일반적인 함수 표현식보다 표현이 간결하고 가독성이 좋다.

    ```jsx
    // ES5
    [1, 2, 3].map(function (v) {
      return v * 2;
    });

    // ES6
    [1, 2, 3].map(v => v * 2); // [2, 4, 6]
    ```

> 💡 화살표 함수와 일반 함수의 차이

1. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor다.

    ```jsx
    const Foo = () => {};
    // 화살표 함수는 생성자 함수로서 호출할 수 없다.
    new Foo(); // TypeError: Foo is not a constructor
    // 화살표 함수는 prototype 프로퍼티가 없다.
    Foo.hasOwnProperty('prototype'); // false
    ```

2. 중복된 매개변수 이름을 선언할 수 없다.

    ```jsx
    // 일반 함수
    function normal(a, a) { return a + a; }
    console.log(normal(1, 2)); // 4

    // 화살표 함수
    const arrow = (a, a) => a + a;
    // SyntaxError: Duplicate parameter name not allowed in this context
    ```

3. 화살표 함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 this, arguments, super, new.target을 참조하면 스코프 체인을 통해 상위 스코프의 this, arguments, super, new.target을 참조한다.

> 💡 this

- 화살표 함수가 일반 함수와 구별되는 가장 큰 특징은 바로 this다. this 바인딩은 함수의 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다. 다시 말해, 함수를 정의할 때 this에 바인딩할 객체가 정적으로 결정되는 것이 아니고, 함수를 호출할 때 함수가 어떻게 호출되었는지에 따라 this에 바인딩할 객체가 동적으로 결정된다.
- ES6에서는 화살표 함수를 사용하여 "콜백 함수 내부의 this 문제"를 해결할 수 있다. 화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다. 이를 lexical this라 한다. 이는 마치 렉시컬 스코프와 같이 화살표 함수의 this가 함수가 정의된 위치에 의해 결정되는 것을 의미한다.

    ```jsx
    // Bad
    const person = {
      name: 'Lee',
      sayHi: () => console.log(`Hi ${this.name}`)
    };
    person.sayHi(); // Hi

    // Good
    const person = {
      name: 'Lee',
      sayHi() {
        console.log(`Hi ${this.name}`)
      }
    };

    person.sayHi(); // Hi Lee
    ```

- sayHi 프로퍼티에 할당한 화살표 함수 내부의 this는 메서드를 호출한 객체인 person을 가리키지 않고 상위 스코프인 전역의 this가 가리키는 전역 객체를 가리킨다. 따라서 화살표 함수로 메서드를 정의하는 것은 바람직하지 않다. 메서드를 정의할 때는 ES6 메서드 축약 표현으로 정의한 ES6 메서드를 사용하는 것이 좋다.

> 💡 Rest 파라미터

- 화살표 함수로 가변 인자 함수를 구현해야 할 때는 반드시 Rest 파라미터를 사용해야 한다. Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.

    ```jsx
    function foo(...rest){
      // 매개변수 rest는 인수들의 목록을 배열로 전달받는 Rest 파라미터다.
      console.log(rest); // [1,2,3,4,5]
    }

    foo(1,2,3,4,5);
    ```

- Rest 파라미터는 이름 그대로 먼저 선언된 매개변수에 할당된 인수를 제외한 나머지 인수들로 구성된 배열이 할당되므로 Rest 파라미터는 반드시 마지막 파라미터이어야 하고 단 하나만 선언할 수 있다.

    ```jsx
    function foo(param1, param2, ...rest){
      console.log(param1); // 1
      console.log(param2); // 2
      console.log(rest); // [3,4,5]
    }

    bar(1,2,3,4,5);
    ```

- 함수와 ES6 메서드는 Rest 파라미터와 arguments 객체를 모두 사용할 수 있지만 화살표 함수는 함수 자체의 arguments 객체를 갖지 않는다. 따라서 화살표 함수로 가변 함수를 구현해야 할 때는 반드시 Rest 파라미터를 사용해야 한다.
- 함수를 호출할 때 매개변수의 개수만큼 인수를 전달하는 것이 바람직하지만 그렇지 않은 경우에도 에러가 발생하지 않는다. 인수가 전달되지 않은 매개변수의 값은 undefined인데 이를 방치하면 의도치 않은 결과가 나올 수 있다.
- ES6에서 도입된 매개변수 기본값을 사용하면 함수 내에서 수행하던 인수 체크 및 초기화를 간소화할 수 있다.

    ```jsx
    function sum(x = 0, y = 0) {
      return x + y;
    }

    console.log(sum(1 ,2)); // 3
    console.log(sum(1));    // 1

    function logName(name = 'Lee') {
      console.log(name);
    }

    logName();          // Lee
    logName(undefined); // Lee
    logname(null);      // null
    ```
