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
# 📚 26장 ES6 함수의 추가 기능
