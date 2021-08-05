# 19장 프로토타입(https://www.notion.so/19-dd441273cd4d40dc83ee5c56e658dcfa)
□ 객체 지향 프로그래밍

- 절차지향적 관점 벗어나 여러 개의 독립적 단위( 객체 ) 집합 프로그램
    - 추상화 + 모델링 : 실세계의 실체( 사물이나 개념 ) 특징이나 성질을 필요한 속성만 인식하거나 구별
    - 생성자 함수 + 클래스 : 데이터와 기능을 하나의 논리적 단위로 묶은 자료구조
        - 상태( state ) : 데이터/프로퍼티
        - 동작( behavior ) : 기능/메서드

```jsx
const circle = { // 원 객체
    // 상태
    radius: 5, // 반 지름
    // 동작
    getDiameter() {
        return this.radius * 2; // 원의 지름 : 반지름 * 2
    },
    getPerimeter() {
        return this.radius * 2 * Math.PI; // 원의 둘레 : 반지름 * 2 * 3.14( 원주율 )
    },  
    getArea() {
        return this.radius **2 * Math.PI ; // 원의 넓이 : 반지름 * 반지름 * 3.14( 원주율 )
    }
};

console.log(circle);
console.log(circle.getDiameter());
console.log(circle.getPerimeter());
console.log(circle.getArea());
```

□ 프로토타입 기반 상속

- 상속 : 프로토타입 프로퍼티 공통( 공유 ) 사용
- 상속 필요성 : 코드 중복 제거 + 공통 메서드 생성 메모리 효율적 사용

```jsx
// 생성자 함수 : 동일한 구조 객체 생성
function Circle(radius) {
    this.radius = radius;
    this.getArea = function() { 
        return this.radius ** 2 * Math.PI;
    }
}

const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea); // false 다른 메서드 
console.log(circle1.getArea());
console.log(circle2.getArea());

// 상속( 공통 ) : 모든 인스턴스 공유 사용
function Circle(radius) {
    this.radius = radius;
}

Circle.prototype.getArea = function() { // 상속
    return this.radius ** 2 * Math.PI;
}

const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea); // true 동일 메서드 
console.log(circle1.getArea());
console.log(circle2.getArea());
```

□ __proto__ 접근자 프로퍼티, prototype 프로퍼티, prototype.constructor 프로퍼티 관계

```jsx
function Person(name) {
    this.name = name;
}

const me = new Person('Lee'); 

// __proto__ 접근자 프로퍼티( Object.prototype 상속 )와 prototype 프로퍼티 동일한 프로퍼티 참조
console.log(Person.prototype === me.__proto__); // true

// prototype.constructor 프로퍼티 생성자 함수 참조
console.log(me.constructor === Person); // true
```

- 생성자 함수와 프로토타입 언제나 쌍( pair )으로 존재

□ 프로토타입 체인

- 스코프 체인과  프로토타입 체인 협력하여 식별자와 프로퍼티 검색

```jsx
function Person(name) {
    this.name = name;
}

Person.prototype.sayHello = function() {
    console.log(`Hi! My name is ${this.name}`);
}

const me = new Person('Lee');
console.log(me.hasOwnProperty('name'));

// __proto__ 접근자 프로퍼티 > protytype 프로퍼티 접근
Object.getPrototypeOf(me) === Person.prototype; // true
Object.getPrototypeOf(Person.prototype) === Object.prototype; // true
```

□ instanceof 연산자

```jsx
// 생성자함수.prototype이 인스턴스 프로토타입 체인 상에 존재하면 true
인스턴스 instanceof 생성자함수
```

□ 프로토타입 교체

- 생성자 함수 프로토타입 교체 : 생성할 객체 프로토타입 교체

```jsx
const Person =(function() {
    // 생성자 함수
    function Person(name) {
        this.name = name;
    }

    // 생성자 함수 프로토타입 교체
    Person.prototype = {
        constructor: Person, // constructor 프로퍼티와 생성자 함수 연결
        sayHello() {
            console.log(`Hi! My name is ${this.name}`);
        }
    }

    // 생성자 함수 반환
    return Person;
}());

const me = new Person('Lee');
console.log(me.constructor === Person); // true
```

- 인스턴스 프로토타입 교체 : 생성된 객체 프로토타입 교체

```jsx
// 생성자 함수
function Person(name) {
    this.name = name;
}

// 인스턴스 생성
const me = new Person('Lee');

const parent = {
    constructor: Person, // constructor 프로퍼티와 생성자 함수 연결
    sayHello() {
        console.log(`Hi! My name is ${this.name}`);
    }
}

// 인스턴스 프로토타입 교체 시 생성자 함수 prototype 연결 코드 필요
Person.prototype = parent; 
// 인스턴스 프로토타입 교체
Object.setPrototypeOf(me, parent);

console.log(me.constructor === Person); // true
```

- 프로토타입 교체 주의 사항

```jsx
// 1. __proto__ 접근자 프로퍼티 코드 사용 제한
const obj = Object.create(null); // 직접 상속 : Object.prototype 상속하지 않고 객체 생성 가능
console.log(obj.__proto__); // undefined
console.log(Object.getPrototypeOf(obj)); // null

const parent = {};
const child = {};

// 프로토타입 참조 취득
Object.getPrototypeOf(parent); // getter
Object.getPrototypeOf(child);

// 2. 프로토타입 상호 참조 에러 발생( 단 방향 링크드 리스트 연결)
Object.setPrototypeOf(child, parent); //setter
//child.__proto__ = parent; // 자식 > 부모 참조
Object.setPrototypeOf(parent, child);
//parent.__proto__ = child; // 부모 > 자식 참조
```
※ **프로토타입 직접 변경보다 직접 상속이나 ES6 클래스 사용 권장**

□ 직접 상속( Object.create 메서드 객체 생성 )

```jsx
/* 상속 없이 객체 생성 */
let obj = Object.create(null); // Object.prototype 상속 X
console.log(Object.getPrototypeOf(obj) === null); // true

console.log(obj.hasOwnProperty('a')); // 오류 발생
// **Objcet.prototype 빌트인 메서드 간접 호출 권장**
console.log(Object.prototype.hasOwnProperty.call(obj, 'a')); // false

/* Objcet 상속 객체 생성 */
obj = Object.create(Object.prototype); // Object.prototype 상속
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true
// 프로퍼티 어트리뷰트 지정 생성
obj = Object.create(Object.prototype, {
    x: {
        value: 1,
        writable: true,
        enumerable: true,
        configurable: true
    }
});
console.log(obj.x); // 1

/* 객체 리터럴 상속 객체 생성 */
const myProto = { x: 10 };
obj = Object.create(myProto); // Object.prototype + 프로퍼티 상속
console.log(obj.x);
console.log(Object.getPrototypeOf(obj) === myProto); // true

/* 생성자 함수 상속 객체 생성 */
function Person(name) {
    this.name = name;
}
obj = Object.create(Person.prototype);
obj.name = '홍길동';
console.log(obj.name);
console.log(Object.getPrototypeOf(obj) === Person.prototype); // true

/* ES6 객체 리터럴 내부 __proto__ 의한 직접 상속 */
obj = {
    y: 20,
    __proto__: myProto
};
console.log(obj.y);
console.log(Object.getPrototypeOf(obj) === myProto);
```

□ 오버라이딩과 프로퍼티 섀도잉

```jsx
const Person =(function() {
    // 생성자 함수
    function Person(name) {
        this.name = name;
    }

    // 프로토타입 메서드
    Person.prototype.sayHello = function() {
        console.log(`Hi! My name is ${this.name}`);
    }

    // 생성자 함수 반환
    return Person;
}());

const me = new Person('Lee');
// 인스턴스 메서드( 오버라이딩 )
me.sayHello = function() {
    console.log('오버라이딩과 프로퍼티 섀도잉');
}
me.sayHello();

// 오버로딩 arguments 객체 사용 구현
```

□ 정적( static ) 프로퍼티/메서드

- 생성자 함수( 객체 ) 소유 프로퍼티/메서드

```jsx
// 생성자 함수
function Person(name) {
    this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function() {
    console.log(`Hi! My name is ${this.name}`);
}

// 정적 프로퍼티
Person.staticProp = 'static prop';

// 정적 메서드
Person.staticMethod = function() {
    console.log('static method');
}

const me = new Person('Lee');
me.staticMethod(); // 인스턴스 호출 오류

// 프로퍼티/메서드 생성자함수로 호출
Person.staticMethod();
console.log(Person.staticProp);

/* prototype 메소드 this 참조 코드 없는 경우 > 정적 메서드 변경 가능 */
// 생성자 함수
function Foo() {}

// prototype 메서드 : 인스턴스 생성 후 호출
Foo.prototype.x = function() {
    console.log('x');
}
const foo = new Foo();
foo.x();

// 생성자 함수 정적 메서드 : 생성자 함수 호출
Foo.x = function() {
    console.log('x');
}
Foo.x();
```

□ 프로퍼티 존재 확인

```jsx
/** 
 * key: 프로퍼티 키를 나타내는 문자열
 * object: 객체로 평가되는 표현식
 * key in object
 */

const person = {
    name: 'Lee',
    address: 'Seoul'
};

console.log('name' in person); // true
console.log('age' in person);  // false
console.log('toString' in person); // true > 프로토타입( 상속 ) 프로퍼티 존재 true

// ES6 Reflect.has 메서드 동일 동작
console.log(Reflect.has(person, 'name'));     // true
console.log(Reflect.has(person, 'toString')); // true

// 객체 고유의 프로퍼티만 확인
console.log(person.hasOwnProperty('name')); // true
console.log(person.hasOwnProperty('toString')); // flase
```

□ 프로퍼티 열거

- 객체( 프로퍼티 열거 ) : for-in문

```jsx
/** 
 * for(변수선언문 in 객체) { 반복 로직 }
 * for-in문 : 프로토타입 프로퍼티까지 열거( [[Enumerable]] true )
 * 열거 제외 : symbol 프로퍼티, [[Enumerable]] false
 * 열거 순서 보장하지 않음 주의 > 브라우저 정렬 후 출력
 */

const person = {
    name: 'Lee',
    address: 'Seoul'
};

// 프로토타입 프로퍼티까지 열거
for(const key in person) {
    console.log(key, ' : ', person[key]);
}

// 객체 고유 프로퍼티만 열거
for(const key in person) {
    if(!person.hasOwnProperty(key)) continue; // 고유 프로퍼티 체크
    console.log(key, ' : ', person[key]);
}

/* Object.key/values/entries 메서드 사용 권장 */
// Object.keys : 객체 자신 열거 가능( [[enumerable]] true ) 프로퍼티 배열 반환
console.log(Object.keys(person)); 

//ES8 Object.values : Object.keys 동일
console.log(Object.values(person));

//ES8 Object.entries : 프로퍼티 키 배열와 값 배열 배열에 담아 반환
Object.entries(person).forEach(([key, value]) => console.log(key, value));
```

- 배열( 요소 열거 ) : for문, for-of문, forEach메서드

```jsx
const arr = [1, 2, 3];

// for문
for(let i=0; i<arr.length; i++) {
    console.log(arr[i]);
}

// for-of문
for(const value of arr) {
    console.log(value);
}

// forEach메서드
arr.forEach(v => console.log(v));
```
