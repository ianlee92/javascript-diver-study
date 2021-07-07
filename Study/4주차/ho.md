# 16장 프로퍼티 어트리뷰트(https://www.notion.so/16-b923b6b720b94925b082096e331a3c4d)
□ 프로퍼티 어트리뷰트

- 프로퍼티 상태 나타내는 프로퍼티 어트리뷰트( [[내부 슬롯/메소드]] ) 자바스크립트 엔진 자동 정의

```jsx
const person = {
    name: 'Lee',
};

// Object.getOwnPropertyDescriptors 메서드 내부 슬롯 간접적 접근 가능
console.log(Object.getOwnPropertyDescriptors(person, 'name')); // 특정 내부 슬롯 확인

person.age = 20;
console.log(Object.getOwnPropertyDescriptors(person)); // 내부 슬롯 전체 확인
```

□ 프로퍼티 어트리뷰트 종류

- 데이터 프로퍼티 : 키와 값으로 구성된 **데이터 프로퍼티**

- 접근자 프로퍼티 : 데이터 프로퍼티 값 읽거나 저장하는 **접근자 함수**

```jsx
const person = {
		// 데이터 프로퍼티
    firstName: '홍',
		lastName: '길동',
		// 접근자 프로퍼티
    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    },
    set fullName(name) {
        [this.firstName, this.lastName] = name.split(' ');
    }
};

person.fullName = '홍 차';     // setter 호출
console.log(person.fullName); // getter 호출

console.log(Object.getOwnPropertyDescriptors(person, 'fullName'));
```

□ 프로퍼티 어트리뷰트( 프로퍼티 상태 값 ) 개발자 정의 방법

```jsx
const person = {};

// 1. 데이터 프로퍼티 정의
Object.defineProperty(person, 'firstName', {
    value: '홍',
    writable: true,
    enumerable: true,
    configurable: true
});

// 값 미 지정 : value 속성 undefined, 나머지 속성 false
Object.defineProperty(person, 'lastName', {
    value: '길동'
});

console.log(Object.keys(person)); // lastName 프로퍼티 [[Enumerable]] 속성 false
// 코드 무시 : 오류 발생 하지 않음
person.lastName = '자';           // [[Writable]] 속성 false 
delete person.lastName;           // [[Configurable]] 속성 false 
console.log(Object.getOwnPropertyDescriptors(person));

// 2. 접근자 프로퍼티 정의
Object.defineProperty(person, 'fullName', {
    get() {
        return `${this.firstName} ${this.lastName}`;
    },
    set(name) {
        [this.firstName, this.lastName] = fullName.split(' ');
    },
    enumerable: true,
    configurable: true
});

console.log(Object.getOwnPropertyDescriptors(person, 'fullName'));
person.fullName = '홍 차'; // lastName [[Writable]] 속성 false
console.log(person);
```

```jsx
// 다수 프로퍼티 속성 정의
const person = {};

Object.defineProperties(person, {
    'firstName': {
				value: '홍',
        writable: true,
        enumerable: true,
        configurable: true
		},
    'lastName': {
				value: '길동',
        writable: true,
        enumerable: true,
        configurable: true
		},
		'fullName': {
				get() {
						return `${this.firstName} ${this.lastName}`;
				},
				set(name) {
						[this.firstName, this.lastName] = name.split(' ');
				},
			  enumerable: true,
			  configurable: true
		}
});

person.fullName = '홍 차';
console.log(person);
```

□ 객체 변경 방지 메서드

- 얕은 변경 방지 : 직속 프로퍼티만 변경 방지

```jsx
// 1. 객체 확장( 추가 ) 금지
const person = {
    name: 'Lee'
};

Object.preventExtensions(person); // 객체 확장 금지
console.log(Object.isExtensible(person)); // 객체 확장 가능 여부 체크

person.age = 20; // 코드 무시 > strict mode 에러
console.log(person);

Object.defineProperty(person, 'birthday', { // 에러 발생
    value: '20210629'
});

// 2. 객체 밀봉( 읽기/쓰기 가능 ) : 프로퍼티 추가, 삭제, 어트리뷰트 재 정의 금지
const person = {
    name: 'Lee'
};

Object.seal(person); // 객체 밀봉
console.log(Object.isSealed(person)); // 객체 밀봉 여부 체크

// 프로퍼티 삭제 금지
delete person.name; // zhem 무시 > strict mode 에러
console.log(person);

console.log(Object.getOwnPropertyDescriptors(person)); // [[configurable]] false
// 프로퍼티 어트리뷰트 재 정의 금지
Object.defineProperty(person, 'name', { // 에러 발생
    configurable: true
});

// 3. 객체 동결( 읽기 전용 ) : 프로퍼티 추가, 갱신, 삭제, 어트리뷰트 재 정의 금지
const person = {
    name: '홍'
};

Object.freeze(person);
console.log(Object.isFrozen(person)); // 객체 동결 영부 체크

person.name = '길동'; // 갱신 금지
console.log(person.name);
```

- 불변 객체 : 중첩 객체까지 동결

```jsx
// 얕은 변경 방지
const person = {
    name: '홍',
    address: {
        city: 'seoul'
    }
};

Object.freeze(person);
console.log(Object.isFrozen(person));         // 객체 동결
console.log(Object.isFrozen(person.address)); // 중첩 객체 미 동결

// 불변 객체 메서드
function deepFreeze(target) {
    if(target // falsy 값 제외 true
        && typeof target === 'object' // object 체크
        && !Object.isFrozen(target)) { // 미 동결 객체 체크
        
				Object.freeze(target);
        // 모든 프로퍼티 순회 재귀 호출
        Object.keys(target).forEach(key => deepFreeze(target[key]));
    }

    return target;
}

const person = {
    name: '홍',
    address: {
        city: 'seoul'
    }
};

deepFreeze(person);
console.log(Object.isFrozen(person));         // 객체 동결
console.log(Object.isFrozen(person.address)); // 중첩 객체 동결
```

---
# 17장 생성자 함수에 의한 객체 생성(https://www.notion.so/17-9062379d86d54707a66413efe5f00578)
□ 객체 생성 방법

```jsx
// 1. 객체 리터럴 : {}
const obj = {};

// 2. 생성자 함수 : new 연산자 함께 생성자 함수 호출
/* 빌트인( 내장 ) 생성자 함수 : 함수명 첫 글자 대문자 */
const obj = new Object();
const str = new String('str');
const num = new Number(1);
const bool = new Boolean(true);
const func = new Function('x', 'return x*x');
const arr = new Array('1', 1);
const regExp = new RegExp(/ab+c/i);
const date = new Date();
```

- 생성자 함수( template/class ) : 동일한 프로퍼티 갖는 객체 다수 생성 사용

```jsx
function Circle(radius) {
    // this : 자신( 생성될 객체 ) 참조
		console.log(this); // 1. 암묵적 빈 객체( Circle {} ) this 바인딩

		this.radius = radius; // 2. 인스턴스 초기화
		this.getDiameter = function() {
			return this.radius * 2;
		};
		// 3. 암묵적 인스터스 바인딩된 this 반환 > 생성자 함수 return문 반드시 생략
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);
console.log(circle1.getDiameter());
console.log(circle2.getDiameter());

// new 연산자 누락 주의 : 일반 함수로서 호출
const circle3 = Circle(10); // return문 없음 undefined 반환
```

□ 함수 객체 추가 내부 슬롯과 메서드

- 함수 객체 함수로서 동작( 호출 ) 필요한 추가 내부 슬롯과 내부 메소드
    - 추가 내부 슬롯 : [[Environment]], [[FormalParameters]]
    - 추가 내부 메소드( callable ) : [[Call]], [[Construct]]
        - non-constructor : 일반 함수 [[Call]] 호출
        - constructor : new 생성자 함수 [[Construct]] 호출 + 일반 함수 [[Call]] 호출 가능( 하단 호출 방지 로직 확인 )

```jsx
/* new 객체 생성 가능 함수 정의 방법 */
// 함수 선언문
function foo() {}
new foo();
foo();

// 함수 표현식
const bar = function() {};
new bar();
bar();

// 프로퍼티 함수 선언
const baz = {
    x: function() {} 
};
new baz.x();
baz.x();

/* new 호출 에러 발생 함수 정의 방법 */
// 화살표 함수
const arrow = () => {}; 
// new arrow(); 에러 발생
arrow();

// ES6 메서드 축약 표현
const obj = {
    x() {}
}; 
// new obj.x(); 에러 발생
obj.x();
```

□ 생성자 함수 정의 방법

- 네이밍 : 파스칼 표기법( pascal case )

```jsx
// new 연산자 체크( new.target )
function Circle(radius) {
    if(!new.target) { // [[Call]] 호출 undefined
        return new Circle(radius);
    }
    
    this.radius = radius;
    this.getDiameter = function() {
        return this.radius * 2;
    }
}

// IE this 소속 객체 체크( scope-safe constructor 패턴 )
function Circle(radius) {
    if(!(this instanceof Circle)) { // [[Call]] 호출 window
        return new Circle(radius);
    }
    
    this.radius = radius;
    this.getDiameter = function() {
        return this.radius * 2;
    }
}
```

---
# 18장 함수와 일급객체(https://www.notion.so/18-78e940dca87e4518b37a02effa4e6fc0)
□ 자바스크립트 함수 **일급 객체**

```jsx
// 1. 리터럴( 값 )로 생성 가능
const increase = function(num) {
    return ++num;
}

const decrease = function(num) {
    return --num;
}

// 2. 변수나 자료 구조( 객체, 배열 등 ) 할당 가능
const predicates = { increase, decrease };

// 3. 함수 매개변수, 반환변수 전달 가능
function makeCounter(predicates) {
    let num = 0;

    return function() {
        num = predicates(num);
        return num;
    };
}

const increaser = makeCounter(predicates.increase);
console.log(increaser()); // 1
const decreaser = makeCounter(predicates.decrease);
console.log(decreaser()); // -1
```

□ 함수 객체 프로퍼티

- arguments 프로퍼티 : 인수들의 정보를 담고 있는 유사 배열 객체

```jsx
/* 가변 인자 함수 구현 */

// arguments 프로퍼티( 객체 )
function sum() {
    const arr = Array.prototype.slice.call(arguments); // arguments 객체 배열로 변환
    return arr.reduce(function(pre, cur) { // 배열 메소드 reduce 사용
        return pre + cur;
    }, 0);
}
sum(1, 2, 3, 4, 5); // 15

// ES6 Rest 파라미터( 배열 )
function sum(...args) { // 배열 매개 변수
    return args.reduce((pre, cur) => pre + cur, 0);
}
sum(1, 2, 3, 4, 5);
```

- caller 프로퍼티 : 비표준 프로퍼티 패스~ > 자신을 호출한 함수 가리킴

```jsx
function foo(func) {
    return func();
}

function bar() {
    return 'caller : '+ bar.caller;
}

console.log(bar());    // null
console.log(foo(bar)); // foo 함수
```

- length 프로퍼티 : 매개변수의 개수
    - arguments.length 프로퍼티 : 호출 시 전달된 인수의 개수

```jsx
function foo(x) { 
    console.log(arguments.length); // 2
}

foo(1, 2);               
console.log(foo.length); // 1
```

- name 프로퍼티 : 함수 이름 나타냄

```jsx
// 기명 함수 표현식 : 함수명
var namedFunc = function foo() {};
console.log(namedFunc.name); // foo

// 익명 함수 표현식
// ES 5 : 빈 문자열 값
// ES 6 : 함수 객체 식별자
var anonymousFunc = function() {};
console.log(anonymousFunc.name); // anonymousFunc

// 함수 선언문 : 함수 객체 식별자
function bar() {}
console.log(bar.name); // bar
```

- __proto__ 접근자 프로퍼티 : 생성자 함수 prototype 접근

```jsx
const obj = { a: 1 };
console.log(obj.__proto__ == Object.prototype); // true

// 객체 고유의 프로퍼티 확인 메서드
console.log(obj.hasOwnProperty('a')); // true
console.log(obj.hasOwnProperty('__proto__')); // 상속 프로퍼티 false
```

- 생성자 함수( constructor만 소유 ) prototype 프로퍼티 : 인스턴스 prototype 할당

```jsx
(function() {}).hasOwnProperty('prototype'); // 함수 객체 true
({}).hasOwnProperty('prototype'); // 일반 객체 false
```
