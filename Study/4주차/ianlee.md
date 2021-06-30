### 📅 2021년 6월 29일 화요일
# 📚 16장 프로퍼티 어트리뷰트
- 모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는다. 내부 슬롯은 자바스크립트 엔진의 내부 로직이므로 원칙적으로 직접 접근할 수 없지만 [[Prototype]] 내부 슬롯의 경우, __proto__를 통해 간접적으로 접근할 수 있다.
- 자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다. 프로퍼티의 상태란 프로퍼티의 값(value), 값의 갱신 가능 여부(writable), 열거 가능 여부(enumerable), 재정의 가능 여부(configurable)를 말한다.
- 프로퍼티 어트리뷰트는 자바스크립트 엔진이 관리하는 내부 상태 값(meta-property)인 내부 슬롯 [[Value]], [[Writable]], [[Enumerable]], [[Configurable]]이다. 따라서 프로퍼티 어트리뷰트에 직접 접근할 수 없지만 Object.getOwnPropertyDescriptor 메서드를 사용하여 간접적으로 확인할 수는 있다.

```jsx
const person = {
  name: 'Lee'
};

// 프로퍼티 동적 생성
person.age = 20;

// 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환한다.
console.log(Object.getOwnPropertyDescriptors(person);
/*
{
  name: {value: "Lee", writable: true, enumerable: true, configurable: true},
  age: {value: 20, writable: true, enumerable: true, configurable: true}
}
*/
```

- Object.getOwnPropertyDescriptor 메서드는 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터(PropertyDescriptor) 객체를 반환한다. ES8에서 도입된 Object.getOwnPropertyDescriptor**s** 메서드는 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환한다.
- 프로퍼티는 데이터(data) 프로퍼티와 접근자(accessor) 프로퍼티로 구분할 수 있다. 데이터 프로퍼티는 키와 값으로 구성된 지금까지 살펴본 일반적인 프로퍼티이며 접근자 프로퍼티는 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티다.

![https://blog.kakaocdn.net/dn/beT1zX/btq8lX97zqK/63jGt24nu4MgOEU5nHs4Nk/img.png](https://blog.kakaocdn.net/dn/beT1zX/btq8lX97zqK/63jGt24nu4MgOEU5nHs4Nk/img.png)

- 메서드 앞에 get, set이 붙은 메서드가 있는데 이것들이 바로 getter와 setter 함수이고, getter/setter 함수의 이름이 접근자 프로퍼티다. 접근자 프로퍼티는 자체적으로 값을 가지지 않으며 다만 데이터 프로퍼티의 값을 읽거나 저장할 때 관여할 뿐이다.
- Object.defineProperty 메서드를 사용하면 프로퍼티의 어트리뷰트를 정의할 수 있다. Object.defineProperties 메서드를 사용하면 여러 개의 프로퍼티를 한 번에 정의할 수 있다. 프로퍼티 정의란 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것을 말한다.
- Enumerable 값이 false인 경우 해당 프로퍼티는 for ... in 문이나 Object.keys 등으로 열거할 수 없고 Writable의 값이 false인 경우 해당 프로퍼티의 value의 값을 변경할 수 없고 Configurable의 값이 false인 경우 해당 프로퍼티를 삭제하거나 재정의할 수 없다.
- Object.defineProperty 메서드로 프로퍼티를 정의할 때 프로퍼티 디스크립터 객체의 프로퍼티를 일부 생략할 수 있는데 프로퍼티 디스크립터 객체에서 생략된 어트리뷰트는 value, get, set은 undefined로 writable, enumerable, configurable은 false로 기본값을 갖는다.

![https://blog.kakaocdn.net/dn/cxbZ6z/btq8f9ceZk0/m9trJEYTxzU3DEE3tdKXJ1/img.png](https://blog.kakaocdn.net/dn/cxbZ6z/btq8f9ceZk0/m9trJEYTxzU3DEE3tdKXJ1/img.png)

- 객체는 변경 가능한 값이므로 재할당 없이 직접 변경할 수 있다. 즉, 프로퍼티를 추가, 삭제할 수 있고 프로퍼티 값을 갱신할 수 있으며, Object.defineProperty 메서드를 사용하여 프로퍼티 어트리뷰트를 재정의할 수도 있다.
- 자바스크립트는 객체의 변경을 방지하는 다양한 메서드를 제공한다. 객체 변경 방지 메서드들은 객체의 변경을 금지하는 강도가 다르다. 프로퍼티 값 읽기는 모두 가능하며, 프로퍼티 추가는 모두 불가능하다. 프로퍼티 값 쓰기, 프로퍼티 삭제는 아래의 이미지를 참고하자. 프로퍼티 어트리뷰트 재정의는 삭제와 마찬가지로 preventExtensions만 가능하다.

![https://blog.kakaocdn.net/dn/ccDLQ6/btq8mlbR0vS/4yZPIISElzJ3QFjVXZnwok/img.png](https://blog.kakaocdn.net/dn/ccDLQ6/btq8mlbR0vS/4yZPIISElzJ3QFjVXZnwok/img.png)

- 변경 방지 메서드들은 얕은 변경 방지(shallow only)로 직속 프로퍼티만 변경이 방지되고 중첩 객체까지는 영향을 주지는 못한다. 객체의 중첩 객체까지 동결하여 변경이 불가능한 읽기 전용의 불변 객체를 구현하려면 객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로 Object.freeze 메서드를 호출해야 한다.

# 📚 17장 생성자 함수에 의한 객체 생성
> 💡  객체 리터럴에 의한 객체 생성 vs 생성자 함수에 의한 객체 생성

- 객체 리터럴에 의한 객체 생성 방식은 가장 일반적이고 간단한 객체 생성 방식이다. new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환한다. 빈 객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체를 완성할 수 있다.

```jsx
// 빈 객체의 생성
const person = new Object();

// 프로퍼티 추가
person.name = 'Lee';
person.sayHello = function () {
  console.log('Hi! My name is ' + this.name);
};

console.log(person); // {name: "Lee", sayHello: f}
person.sayHello(); // Hi! My name is Lee
```

- 생성자 함수(constructor)란 new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수를 말한다. 생성자 함수에 의해 생성된 객체를 인스턴스(instance)라 한다. 자바스크립트는 Object 생성자 함수 이외에도 String, Number, Boolean, Function, Array, Date, RegExp, Promise 등의 빌트인(built-in) 생성자 함수를 제공한다.
- 객체 리터럴에 의한 객체 생성 방식은 직관적이고 간편하지만 단 하나의 객체만 생성하므로 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적이다.

```jsx
const person1 = {
  name: 'Lee',
  sayHello() {
    return 'Hi! my name is ' + this.name;
  }
};

console.log(person1.sayHello()); // Hi! my name is Lee

const person2 = {
  name: 'Kim',
  sayHello() {
    return 'Hi! my name is ' + this.name;
  }
};

console.log(person2.sayHello()); // Hi! my name is Kim
```

![https://blog.kakaocdn.net/dn/b7FpJ1/btq8f9wxECM/cqEVUqmKkIOOlEovbLrNQ1/img.png](https://blog.kakaocdn.net/dn/b7FpJ1/btq8f9wxECM/cqEVUqmKkIOOlEovbLrNQ1/img.png)

- 객체는 프로퍼티를 통해 객체 고유의 상태(state)를 표현한다. 메서드를 통해 상태 데이터인 프로퍼티를 참조하고 조작하는 동작(behavior)을 표현한다. 따라서 프로퍼티는 객체마다 프로퍼티 값이 다를 수 있지만 메서드는 내용이 동일한 경우가 일반적이다.
- 생성자 함수에 의한 객체 생성 방식은 마치 객체(인스턴스)를 생성하기 위한 템플릿(클래스)처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.

```jsx
// 생성자 함수
function Person(name) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다
  this.name = name;
  this.sayHello = function(){
    return 'Hi! my name is ' + this.name;
  };
}

// 인스턴스의 생성
const foo = new Person('Lee'); // Person 객체 생성
console.log(foo.sayHello()); // Hi! my name is Lee
```

![https://blog.kakaocdn.net/dn/A8qaF/btq8jKRhMps/vnangzkQ8YiCKwougGN6dk/img.png](https://blog.kakaocdn.net/dn/A8qaF/btq8jKRhMps/vnangzkQ8YiCKwougGN6dk/img.png)

- 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다. 만약 new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작한다. 일반 함수로서 호출되면 반환문이 없으므로 암묵적으로 undefined를 반환하고 this는 전역 객체를 가리킨다.

> 💡  생성자 함수의 인스턴스 생성 과정

```jsx
// 생성자 함수
function Circle(radius) {
  // 인스턴스 초기화
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}
// 인스턴스 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
```

1. 인스턴스 생성과 this 바인딩

2. 인스턴스 초기화

3. 인스턴스 반환

```jsx
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
  console.log(this); // Circle {}
  
  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
  
  // 3-1. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
  
  // 3-2. 암묵적으로 this를 반환하고 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시된다.
  return {};
  
  // 3-3. 암묵적으로 this를 반환하고 명시적으로 원시 값을 반환하면
  // 원시 값 반환은 무시되고 암묵적으로 this가 반환된다.
  return 100;
}

// 3-1. 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: f}

// 3-2. 인스턴스 생성. Circle 생성자 함수는 명시적으로 반환한 객체를 반환한다.
const circle = new Circle(1);
console.log(circle); // {}

// 3-3. 인스턴스 생성. Circle 생성자 함수는 명시적으로 반환한 객체를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: f}
```

- 이처럼 생성자 함수 내부에서 명시적으로 this가 아닌 다른 값을 반환하는 것은 생성자 함수의 기본 동작을 훼손하므로 생성자 함수 내부에서 return 문을 반드시 생략해야 한다.
- 함수가 일반 함수로서 호출되면 함수 객체의 내부 메서드 [[Call]]이 호출되고 new 연산자와 함께 생성자 함수로서 호출되면 내부 메서드 [[Construct]]가 호출된다.

```jsx
function foo() {}

// 일반적인 함수로서 호출: [[Call]]이 호출된다.
foo();

// 생성자 함수로서 호출: [[Construct]]가 호출된다.
new foo();
```

- 함수 객체는 callable이면서 constructor이거나 callable이면서 non-constructor다. 즉, 모든 함수 객체는 호출할 수 있지만 모든 함수 객체를 생성자 함수로서 호출할 수 있는 것은 아니다.
- constructor는 함수 선언문, 함수 표현식, 클래스(클래스도 함수다), non-constructor는 메서드(ES6 메서드 축약 표현), 화살표 함수이다.

```jsx
// 일반 함수 정의: 함수 선언문, 함수 표현식
function foo() {}
const bar = function () {};
// 프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수다
// 이는 메서드로 인정하지 않는다
const baz = {
  x: function () {};
};

// 일반 함수로 정의된 함수만이 constructor다
new foo(); // foo {}
new bar(); // bar {}
new baz.x(); // x {}

// 화살표 함수 정의
const arrow = () => {};
new arrow(); // typeError: arrow is not a constructor

// 메서드 정의: ES6의 메서드 축약 표현만 메서드로 인정한다
const obj = {
  x() {}
};

new obj.x(); // TypeError: obj.x is not a constructor
```

- 함수가 어디에 할당되어 있는지에 따라 메서드인지를 판단하는 것이 아니라 함수 정의 방식에 따라 constructor와 non-constructor를 구분한다. new 연산자와 함께 호출하는 함수는 non-constructor가 아닌 constructor이어야 한다.

> 💡  ES6 new.target

- 생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해 파스칼 케이스 컨벤션을 사용한다 하더라도 실수는 언제나 발생할 수 있다. 이러한 위험성을 회피하기 위해 ES6에서는 new.target을 지원한다. new.target은 this와 유사하게 constructor인 모든 함수 내부에서 암묵적인 지역 변수와 같이 사용되며 메타 프로퍼티라고 부른다.
- 함수 내부에서 new.target을 사용하면 new 연산자와 함께 생성자 함수로서 호출되었는지 확인할 수 있다. new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다. new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined다.

```jsx
// 생성자 함수
function Circle(radius) {
  // 이 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined다.
  if (!new.target) {
    // new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
    return new Circle(radius);
  }
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
const circle = Circle(5);
console.log(circle.getDiameter()); // 10
```

- new 연산자 없이 생성자 함수를 호출하면 typeError가 발생하지만 함수 내부에서 new.target을 사용하여 new 연산자와 생성자 함수로서 호출했는지 확인하여 그렇지 않은 경우 new 연산자와 함께 재귀 호출을 통해 생성자 함수로서 호출할 수 있다.
- Object와 Function 생성자 함수는 new 연산자 없이 호출해도 new 연산자와 함께 호출했을 때와 동일하게 동작한다. String, Number, Boolean 생성자 함수는 new 연산자와 함께 호출했을 때 String, Number, Boolean 객체를 생성하여 반환하지만 new 연산자 없이 호출하면 문자열, 숫자, 불리언 값을 반환한다. 이를 통해 데이터 타입을 변환하기도 한다.

### 📅 2021년 7월 1일 목요일
# 📚 18장 함수와 일급 객체
# 📚 19장 프로토타입 (~p279)
