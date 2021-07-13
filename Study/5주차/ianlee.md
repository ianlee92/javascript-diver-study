### 📅 2021년 7월 6일 화요일
# 📚 19장 프로토타입 (p280~)
> 💡  객체의 생성 방법

- 프로토타입은 추상 연산 OrdinaryObjectCreate에 전달되는 인수에 의해 결정된다. 이 인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 결정된다.
1. 객체 리터럴에 의해 생성된 객체의 프로토타입
    - 객체 리터럴에 의해 생성된 객체의 프로토타입은 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype다. 즉 객체 리터럴에 의해 생성되는 객체의 프로토타입은 Object.prototype이다.
    - 객체 리터럴이 평가되면 객체는 constructor 프로퍼티와 hasOwnProperty 메서드 등을 소유하지 않지만 상속을 통해 자신의 자산인 것처럼 자유롭게 사용할 수 있다.

    ```jsx
    const obj = { x: 1 };

    // 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 상속받는다.
    console.log(obj.constructor === Object);  // true
    console.log(obj.hasOwnProperty('x'));     // true
    ```

2. Object 생성자 함수에 의해 생성된 객체의 프로토타입
    - Object 생성자 함수를 인수 없이 호출하면 빈 객체가 생성된다. Object 생성자 함수를 호출하면 객체 리터럴과 마찬가지로 추상 연산 OrdinaryObjectCreate가 호출된다. 프로토타입 또한 Object.prototype.

    ```jsx
    const obj = new Object();
    obj.x = 1;

    // Object 생성자 함수에 의해 생성된 obj 객체는 Object.prototype을 상속받는다.
    console.log(obj.constructor === Object);  // true
    console.log(obj.hasOwnProperty('x'));     // true
    ```

    - 객체 리터럴과 Object 생성자 함수에 의한 객체 생성 방식의 차이는 프로퍼티를추가하는 방식에 있다. 객체 리터럴 방식은 객체 리터럴 내부에 프로퍼티를 추가하지만 Object 생성자 함수 방식은 일단 빈 객체를 생성한 이후 프로퍼티를 추가해야 한다.
3. 생성자 함수에 의해 생성된 객체의 프로토타입
    - new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하면 다른 객체 생성 방식과 마찬가지로 추상 연산 OrdinaryObjectCreate가 호출된다. 생성자 함수에 의해 생성되는 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.

    ```jsx
    function Person(name) {
      this.name = name;
    }

    // 프로토타입 메서드 (상속)
    Person.prototype.sayHello = function () {
      console.log(`Hi! My name is ${this.name}`);
    };

    const me = new Person('Lee');

    me.sayHello(); // Hi! My name is Lee

    // hasOwnProperty는 Object.prototype의 메서드다.
    console.log(me.hasOwnProperty('name')); // true
    ```

    - 프로토타입의 프로토타입은 언제나 Object.prototype이다. 위의 예제에서 Person 생성자 함수에 의해 생성된 me 객체는 Object.prototype의 메서드도 호출할 수 있는데 이것은 me 객체가 Person.prototype뿐만 아니라 Object.prototype도 상속받았다는 것을 의미한다.

    ```jsx
    Object.getPrototypeOf(me) === Person.prototype; // true
    Object.getPrototypeOf(Person.prototype) === Object.prototype; // true
    ```

    ![https://blog.kakaocdn.net/dn/bVCOPD/btq8VGlBWdZ/D2XhMC7bmGVrAlRJaz2lqk/img.jpg](https://blog.kakaocdn.net/dn/bVCOPD/btq8VGlBWdZ/D2XhMC7bmGVrAlRJaz2lqk/img.jpg)

    - 내부 슬롯의 참조에 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색하는데 이를 프로토타입 체인이라 한다. 프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘이다.

> 💡  instanceof 연산자

- instanceof 연산자는 이항 연산자로서 좌변에 객체를 가리키는 식별자, 우변에 생성자 함수를 가리키는 식별자를 피연산자로 받는다. 만약 우변의 피연산자가 함수가 아닌 경우 TypeError가 발생한다.

    ```jsx
    객체 instanceof 생성자 함수
    ```

- 우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true로 평가되고, 그렇지 않은 경우에는 false로 평가된다.

    ```jsx
    // 생성자 함수
    function Person(name) {
      this.name = name;
    }
    const me = new Person('Lee');

    // Person.prototype이 me 객체의 프로토타입 체인 상에 존재함
    console.log(me instanceof Person); // true
    // Object.prototype이 me 객체의 프로토타입 체인 상에 존재함
    console.log(me instanceof Object); // true

    // 프로토타입으로 교체할 객체
    const parent = {};

    // 프로토타입의 교체
    Object.setPrototypeOf(me, parent);

    // Person.prototype이 me 객체의 프로토타입 체인 상에 존재하지 않음
    console.log(me instanceof Person); // false
    // Object.prototype이 me 객체의 프로토타입 체인 상에 존재함
    console.log(me instanceof Object); // true

    // parent 객체를 Person 생성자 함수의 prototype 프로퍼티에 바인딩한다
    Person.prototype = parent;

    // Person.prototype이 me 객체의 프로토타입 체인 상에 존재함
    console.log(me instanceof Person); // true
    // Object.prototype이 me 객체의 프로토타입 체인 상에 존재함
    console.log(me instanceof Object); // true
    ```

    - Person.prototype이 me 객체의 프로토타입 체인 상에 존재하지 않으면 false로 평가되고 프로토타입으로 교체한 parent 객체를 Person 생성자 함수의 prototype 프로퍼티에 바인딩하면 me instanceof Person은 true로 평가된다.

> 💡  in 연산자, Reflect.has, Object.prototype.hasOwnProperty 메서드

- 객체에 특정 프로퍼티가 존재하는지 확인하기 위해 in 연산자(ES6에서 도입된 Reflect.has메서드) 또는 Object.prototype.hasOwnProperty 메서드를 사용하면 된다.

    ```jsx
    const person = {
      name: 'Lee',
      address: 'Seoul'
    };

    console.log('name' in person); // true
    console.log('age' in person);  // false

    console.log(Reflect.has(person, 'name'));   // true
    console.log(person.hasOwnProperty('name')); // true
    ```

    - in 연산자는 person 객체가 속한 프로토타입 체인 상에 존재하는 모든 프로토타입에서 검색하고 Object.prototype.hasOwnProperty 메서드는 인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우에만 true를 반환하고 상속받은 프로토타입의 프로퍼티 키인 경우 false를 반환한다.

    ```jsx
    console.log('toString' in person); // true
    console.log(person.hasOwnProperty('toString')); // false
    ```

> 💡  for ... in 문

- 객체의 모든 프로퍼티를 순회하며 열거(enumeration)하려면 for ... in 문을 사용한다.

    ```jsx
    const person = {
      name: 'Lee',
      address: 'Seoul'
    };

    for (const key in person) {
      console.log(key + ': ' + person[key]);
    }

    // name: Lee
    // address: Seoul
    ```

- for ... in문은 in 연산자처럼 순회 대상 객체의 프로퍼티뿐만 아니라 상속받은 프로토타입의 프로퍼티까지 열거하지만 toString과 같은 Object.prototype의 프로퍼티는 열거되지 않는다. for ... in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 true인 프로퍼티를 순회하며 열거(enumeration)한다.

> 💡  Object.keys, Object.values, Object.entries 메서드

- 객체 자신의 고유 프로퍼티만 열거하기 위해서는 for ... in 문을 사용하는 것보다 Object.keys/values/entries 메서드를 사용하는 것을 권장한다.

    ```jsx
    const person = {
      name = 'Lee',
      address: 'Seoul',
      __proto__: { age: 20 }
    };

    console.log(Object.keys(person)); // ["name", "address"]
    console.log(Object.values(person)); // ["Lee", "Seoul"]
    console.log(Object.entries(person)); // [["name", "Lee"], ["address", "Seoul"]]
    Object.entries(person).forEach(([key, value]) => console.log(key, value));
    /*
    name Lee
    address Seoul
    */
    ```

    - Object.keys 메서드는 객체 자신의 열거 가능한(enumberable) **프로퍼티 키**를 배열로 반환한다. ES8에서 도입된 Object.values 메서드는 객체 자신의 열거 가능한 **프로퍼티 값**을 배열로 반환하고 Object.entries 메서드는 객체 자신의 열거 가능한 **프로퍼티 키와 값의 쌍의 배열**을 배열에 담아 반환한다.
    
### 📅 2021년 7월 8일 목요일
# 📚 20장 strict mode
- 잠재적인 오류를 발생시키기 어려운 개발 환경을 만들기 위한 해결책으로 ES5부터 strict mode(엄격 모드)가 추가되었다. 린트 도구는 strict mode가 제한하는 오류는 물론 코딩 컨벤션을 설정 파일 형태로 정의하고 강제할 수 있기 때문에 더욱 강력한 효과를 얻을 수 있다. 따라서 strict mode보다 린트 도구의 사용을 선호한다.

    ![https://blog.kakaocdn.net/dn/bm40CG/btq83iNf4vn/GtJ6iOWJIWlbGII1mL5JB1/img.png](https://blog.kakaocdn.net/dn/bm40CG/btq83iNf4vn/GtJ6iOWJIWlbGII1mL5JB1/img.png)

- strict mode를 적용하려면 전역의 선두 또는 함수 몸체의 선두에 'user strict';를 추가한다. 전역에 strict mode를 적용하는 것과 함수 단위로 strict mode를 적용하는 것은 피해야한다. 따라서 strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.
- strict mode가 발생시키는 에러는 선언하지 않은 변수를 참조하는 암묵적 전역과 delete 연산자로 변수, 함수, 매개변수의 삭제 그리고 중복된 매개변수 이름을 사용하거나 with 문을 사용하면 에러가 발생한다.
# 📚 21장 빌트인 객체
- 자바스크립트 객체는 표준 빌트인 객체, 호스트 객체, 사용자 정의 객체 크게 3개의 객체로 분류할 수 있다.

    ![https://blog.kakaocdn.net/dn/umeTe/btq88yNWFvY/CPAQHjThzI1uSdoFya3Gvk/img.png](https://blog.kakaocdn.net/dn/umeTe/btq88yNWFvY/CPAQHjThzI1uSdoFya3Gvk/img.png)

    ![https://blog.kakaocdn.net/dn/ch8ltg/btq85VKx8ZQ/QWCqFzqtSYPv3rjcRL5kZK/img.png](https://blog.kakaocdn.net/dn/ch8ltg/btq85VKx8ZQ/QWCqFzqtSYPv3rjcRL5kZK/img.png)

- 자바스크립트는 40여 개의 표준 빌트인 객체를 제공한다. Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다.
- 원시값은 객체가 아니므로 프로퍼티나 메서드를 가질 수 없는데도 원시값인 문자열이 마치 객체처럼 동작한다. 마침표 표기법(또는 대괄호 표기법)으로 접근하면 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환해 주기 때문이다. 이처럼 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 래퍼 객체(wrapper object)라 한다.

    ```jsx
    const str = 'hi';

    // 원시 타입인 문자열이 래퍼 객체인 String 인스턴스로 변환된다.
    console.log(str.length); // 2
    console.log(str.toUpperCase()); // HI

    // 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌린다.
    console.log(typeof str); // string
    ```

> 💡  빌트인 전역 프로퍼티와 빌트인 전역 함수

![https://blog.kakaocdn.net/dn/b9IDkG/btq86fJssyW/M97IilGNzoRxcAnekTl1Lk/img.png](https://blog.kakaocdn.net/dn/b9IDkG/btq86fJssyW/M97IilGNzoRxcAnekTl1Lk/img.png)

- 빌트인 전역 프로퍼티(built-in global property)는 전역 객체의 프로퍼티를 의미한다. 주로 애플리케이션 전역에서 사용하는 값을 제공한다. Infinity, NaN, undefined.
- 빌트인 전역 함수(built-in global function)는 애플리케이션 전역에서 호출할 수 있는 빌트인 함수로서 전역 객체의 메서드다. eval, isNaN, parseFloat, parseInt, encodeURI/decodeURI
- parseInt 전달받은 문자열 인수를 정수(integer)로 해석(parsing)하여 반환한다.

    ```jsx
    /*
    전달받은 문자열 인수를 정수로 해석하여 반환한다.
    @param {string} string - 변환 대상 값
    @param {number} [radix] - 진법을 나타내는 기수(2 ~ 36, 기본값 10)
    @returns {number} 변환 결과
    */

    parseInt(string, radix);
    ```

    ```jsx
    parseInt('10');     // 10
    parseInt('10.123'); // 10
    parseInt(10);       // 10
    parseInt(10.123);   // 10

    // '10'을 2진수로 해석하고 그 결과를 10진수 정수로 반환한다.
    parseInt('10', 2);  // 2
    ```

- 10진수 숫자를 해당 기수의 문자열로 변환하여 반환하고 싶을 때는 Number.prototype.toString메서드를 사용한다.

    ```jsx
    const x = 15;

    // 10진수 15를 2진수로 변환하여 그 결과를 문자열로 반환한다.
    x.toString(2); // 1111

    // 문자열 '1111'을 2진수로 해석하고 그 결과를 10진수 정수로 반환한다.
    parseInt(x.toString(2), 2); // 15
    ```

```jsx
// 'A'는 10진수로 해석할 수 없다.
parseInt('A0'); // NaN

// 공백으로 구분된 문자열은 첫 번째 문자열만 변환한다.
parseInt('34 45 66'); // 34
parseInt('40 years'); // 40

// 첫 번째 문자열을 숫자로 변환할 수 없다면 NaN을 반환한다.
parseInt('He was 40'); // NaN

// 앞뒤 공백은 무시된다.
parseInt(' 60 '); // 60
```

- encodeURI 함수는 완전한 URI(Uniform Resource Identifier)를 문자열로 전달받아 이스케이프 처리를 위해 인코딩한다. URI는 인터넷에 있는 자원을 나타내는 유일한 주소로 하위개념으로 URL, URN이 있다. 이스케이프 처리는 네트워크를 통해 정보를 공유할 때 어떤 시스템에서도 읽을 수 있는 아스키 문자 셋으로 변환하는 것이다.

    ![https://poiemaweb.com/img/uri.png](https://poiemaweb.com/img/uri.png)

    ```jsx
    const uri = 'http://example.com?name=이웅모&job=programmer&teacher';

    // encodeURI 함수는 완전한 URI를 전달받아 이스케이프 처리를 위해 인코딩한다.
    const enc = encodeURI(uri);
    console.log(enc);
    // http://example.com?name=%EC%9D%B4%EC%9B%85%EB%AA%A8&job=programmer&teacher

    // decodeURI 함수는 인코딩된 완전한 URI를 전달받아 이스케이프 처리 이전으로 디코딩한다.
    const dec = decodeURI(enc);
    console.log(dec);
    // http://example.com?name=이웅모&job=programmer&teacher
    ```

- encodeURIComponent 함수는 인수로 전달된 문자열을 URI의 구성요소인 쿼리 스트링의 일부로 간주하기 때문에 쿼리 스트링 구분자로 사용되는 =, ?, &까지 인코딩한다.
