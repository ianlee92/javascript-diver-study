# 25장 클래스, 26장 ES6 함수

- 클래스는 일급 객체! 함수로 평가됨.

![Untitled](https://user-images.githubusercontent.com/8978613/127155713-b29765b0-d669-4586-9c7e-3162337b76d8.png)

- new 없으면 에러!
- 몸체에는 생성자, 프로토타입 메서드, 정적 메서드 3개의 메서드만 생성 가능

### 생성자 constructor

- 메서드가 아닌 함수 객체 코드의 일부가 됨.
- 2개 이상 포함하면 에러
- 생략 가능하지만 암묵적으로 정의됨.
- 인스턴스 생성 시 초기 값이 전달됨.
- 암묵적 this 반환
- 반환문이 있으면 안됨! (기본 동작 훼손함)

### 프로토타입 메서드

- 클래스 몸체에 정의한 메서드는 prototype 프로퍼티 필요없음

### 정적 메서드 static

- 인스턴스로 호출 불가!
- 프로토타입 메서드와 속해있는 프로토타입 체인이 다름

### 클래스 메서드 특징

- function 키워드 생략
- 객체처럼 , 필요없음 (자바의 클래스 흉내!!)
- 암묵적 strict mode
- 열거 불가
- new 연산자와 함께 호출 불가

### 프로퍼티

- 인스턴스 프로퍼티는 생성자 내부에!
- 접근제한자 (public, private, protected) 지원 하지 않음! 언제나  public
- 접근자 getter, setter 는 객체와 동일하게 사용

### 클래스 필드

- 자바 따라감. 갈 예정
- this 는 생성자 내에서만!
- 함수를 할당하면 프로토타입 메서드가 아닌 인스턴스 메서드 ⇒ 권장하지 않음
- #을 붙이면 private 필드(추후)
- static 붙이면 정적필드 (추후)

---

2번째!

### 상속

기존 클래스를 상속 받아 새로운 클래스를 확장 (기존 생성자 함수랑 다른 개념이 나왔다)

![extends](https://user-images.githubusercontent.com/8978613/127155885-4496ce83-345d-4b04-9332-7c2669045ec1.png)

![extends_method](https://user-images.githubusercontent.com/8978613/127155973-d031a74f-fec2-4295-af61-41fe2c11387b.png)

- extends 키워드로 클래스 확장
- 클래스 extends 함수객체로 평가되는 모든 표현식 ⇒ (신선함 ^^)

```jsx
function Base1() {}
class Base2 {}

let condition = false

class Derived extends (condition ? Base1 : Base2) {}

const derived = new Derived();
console.log(derived)
console.log(derived instanceof Base1) 
console.log(derived instanceof Base2)
```

- super() : 수퍼클래스의 constructor 호출
    1. 서브클래스에서 constructor 사용 시 반드시 super 호출 
    2. 서브클래스 constructor 에서 super 호출하기전 this 참조 불가 
    3. constructor 에서만 super() 호출
- 수퍼클래서 메소드 참조 시 super.메소드 이름으로 참조 가능
- 정적 메소드는 수퍼클래스의 정적메소드 참조 super.정적메소드

빌트인 함수 확장 : superclass의 메소드를 사용해도 subclass 의 instance. 메서드 체이닝 불가 

```jsx
console.log(myArray.filter(v => v % 2) instanceof MyArray); // true
```

# 26장  ES6 함수의 추가 기능

ES6 이전의 모든 함수는 일반함수, 생성자 함수로도 호출 가능 

⇒ 3가지 종류로 구분함 

![functions](https://user-images.githubusercontent.com/8978613/127156057-fa60dfe0-aba4-4f92-8719-5a2e71febe8f.png)

- 메서드
    - 메서드 축약표현으로 정의된 함수만 의미
    - 메서드가 아닌 함수는 super 키워드 사용 불가
- 화살표 함수
    - 매개변수 여러개인 경우 소괄호 필요
    - 매개변수 한개인 경우 소괄호 생략 가능
    - 매개변수 없는 경우 소괄호 필요
    - 콜백함수로 사용할 때 this 가 전역객체를 가리키지 않음