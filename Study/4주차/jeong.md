# 16장 프로퍼티 어트리뷰트 (속성)

### 프로퍼티 속성 종류

- 레퍼런스 예시 [https://tc39.es/ecma262/multipage/numbers-and-dates.html#sec-date-objects](https://tc39.es/ecma262/multipage/numbers-and-dates.html#sec-date-objects)
- 데이터 프로퍼티  (기본 값 자동 정의)
    - [[Value]], [[Writable]], [[Enumerable]], [[Configurable]]

    ```jsx
    const obj1 = {}
    obj1.name = 'name1'
    console.log(Object.getOwnPropertyDescriptor(obj1, 'name')) 
    // { configurable: true, enumerable: true, value: "name1", writable: true }

    // 재정의 
    obj1.age = 30
    Object.defineProperty(obj1, 'age', {
    	writable: false,
      enumerable: false
    });
    console.log(Object.getOwnPropertyDescriptor(obj1, 'age'))
    // { configurable: true, enumerable: false, value: 30, writable: false }
    ```

- 접근자 프로퍼티 : 접근자 함수로 구성
    - [[Get]], [[Set]], [[Enumerable]], [[Configurable]]

    ```jsx
    const obj2 = {
    	name: 'jeong', 
      get myName() {
      	return this.name;
      },
      set myName(name) {
      	this.name = name
      }
    }
    console.log(Object.getOwnPropertyDescriptor(obj2, 'myName'))
    /* {
      configurable: true,
      enumerable: true,
      get: get myName() {
        return this.name;
      },
      set: set myName(name) {
        this.name = name
      }
    } */
    ```

### 객체 변경 방지

- 객체 확장 금지 : 프로퍼티 추가 금지 (Object.isExtensible)
- 객체 밀봉 : 읽기 쓰기만 가능 (Object.isSealed)
- 객체 동결 : 읽기만 가능 (Object.isFrozen)
- 불변 객체 : 중첩 객체까지 재귀적으로 freeze 호출 해야함.

# 17장 생성자 함수의 의한 객체 생성

```jsx
const obj1 = {}
const obj2 = new Object()
obj1.name = 'name1'
obj2.name = 'name2'
console.log(obj1)   // {name:"name1"}
console.log(obj2)   // {name:"name2"}
```

- 클래스와 같은 형태로 사용 시 new 인스턴스 생성
- new 와 함께 호출하면 생성자 함수, new 없으면 일반 함수
- 생성자는 this 반환, return 은 생략해야함.
- 일반함수와 형식 차이가 없기 때문에 파스칼 케이스로 쓸 것!
- new.target : 생성자 함수를 생성자 없이 호출 시 new 를 붙이지 않았는지 체크하여 내부에서 재귀함수로 붙일 수 있음

|Constructor | Non-Constructor|
|------------|----------------|
|함수 선언문, 함수 표현식 | 화살표 함수, 메서드 축약표현|
|생성자 함수 호출 가능 | 생성자 함수로 호출 시 에러|


# 18장 함수와 일급 객체

- 일급객체
    1. 런타임에 생성 가능 
    2. 변수나 객체, 배열등에 저장가능
    3. 함수 매개변수로 전달 가능 
    4. 함수 반환값으로 사용 가능 
- 함수 프로퍼티

    : arguments, caller, length, name, prototype

- arguments
    - 유사 배열 객체
    - 배열 메서드 사용 시 에러
    - 배열로 사용하려면 간접 호출 사용

    ```jsx
    const array = Array.prototype.slice.call(arguments);
    ```

- length
    - 함수 정의할 때 선언한 매개 변수 개수
- name
    - (식별자 아닌) 함수 이름
- __proto__ 접근자
    - 간접 접근 방법 제공 하는 경우에 한하여 접근 가능
    - 상속 받은 객체가 부모에 접근할 때 필요 ???
    - 상호 참조에 프로토타입 체인이 생성되는 것을 방지하기 위해 사용 (19장 내용)
- prototype
    - 생성자 함수만 소유하는 프로퍼티

# 19. 프로토타입

- 자바스크립트는 멀티 패러다임 프로그래밍 언어
    - 명령형, 함수형, 프로토타입 기반 객체 지향
- 객체 지향 프로그래밍
    - 추상화, 상속
    - 객체 : 속성을 통해 여러개 값을 하나의 단위로 구성한 자료구조
    - 객체의 집합으로 프로그램을 표현
- 상속과 프로토타입
    - 프로토타입으로 상속 받음
    - 포로토타입의 모든 프로퍼티와 메서드를 상속 받음

    ```jsx
    function Original(radius) {
        this.radius = radius 
    }

    Original.prototype.newMethod = function() {}
    const org = new Original(1);
    org.newMethod()
    ```

- `__proto__`
    - 접근자 프로퍼티
    - 모든 객체 소유
    - 객체가 자신의 프로토타입에 접근 또는 교체하기 위해
- prototype
    - 생성자 소유
    - 생성자가 자신이 생성할 객체의 프로토타입을 할당하기 위해

    ```jsx
    function Person(name) {
        this.name = name
    }
    const me = new Person('Lee')
    console.log(Person.prototype === me.__proto__) // 참 
    console.log(me.constructor === Person) // 참 

    const you = {}
    console.log(you.constructor === Object) // 참 

    function they() {} 
    console.log(they.constructor === Function) // 참
    ```

    - 프로토타입과 생성자 함수는 쌍으로 존재 (생성자 함수가 있으면 prototype 존재)
    - 생성자 함수가 생성되는 시점에 프로토타입도 생성
