# 12장 

### 자바스트립트의 함수

- **함수는 (호출할 수 있는) 객체다**. (타언어와 다른점)
- 함수는 표현식이 아닌 문이나 문맥에 따라 표현식인 문으로 해석됨
- () 내부 함수 리터럴은 선언문이 아닌 표현식
- 함수 선언 및 호출 순서

    ![https://user-images.githubusercontent.com/8978613/123204876-e344a400-d4f3-11eb-9a67-a76df786b012.png](https://user-images.githubusercontent.com/8978613/123204876-e344a400-d4f3-11eb-9a67-a76df786b012.png)

- 자바스크립트의 **함수는 일급 객체**(변수할당, 프로퍼티 값, 배열의 요소)

```jsx
var add = function foo(x, y) { return x + y; }
add(3,6)  // 정상
~~foo(2,5)  // 불가능!!!~~
```

- 함수선언문 / 함수 표현식 생성 시점 다름
- 암묵적 식별자 및 함수 호이스팅을 알 수 있는 예제

1.  [https://jsfiddle.net/MiaLee/v8as4L0y/5/](https://jsfiddle.net/MiaLee/v8as4L0y/5/)

```jsx
var bar = 'b'
function bar() { return 2}

console.log(bar)
console.log(bar())
```

2. [https://jsfiddle.net/MiaLee/v8as4L0y/13/](https://jsfiddle.net/MiaLee/v8as4L0y/13/)

```jsx
function bar() { return 2}

function foo() {
	var bar = bar() 
  console.log(bar)
}
foo()
```

- 함수의 호이스팅은 함수를 호출하기 전에 반드시 함수를 선언해야한다는 당연한 규칙을 무시한다. 그래서 함수 선언문 대신 함수 표현식을 사용할 것을 권장한다 ??????

### 함수 호출

- 매개 변수의 개수와 인수의 개수가 일치하는지 체크하지 않음
    - 부족하면 undefined, 초과하면 무시, 인수는 arguments 로 전달됨.

         [https://jsfiddle.net/MiaLee/v8as4L0y/18/](https://jsfiddle.net/MiaLee/v8as4L0y/18/)

    ```jsx
    function foo(x, y) {
    	return arguments[0] + arguments[1] + arguments[2];
    }

    console.log(foo(1,2,3))
    ```

- 매개변수 개수 제한 없음, 최대한 적게! 많으면 객체를 인수로!

### 함수 형태

1. 즉시 실행 함수 
    - 재사용 불가, 변수나 함수 충돌 방지
2. 재귀함수
3. 중첩함수(내부함수)
    - 헬퍼 함수 역할
4. **콜백 함수**
    - 헬퍼 함수 역할
    - 고차  함수 : 매개변수를 통해 콜백함수를 전달 받음 함수, 함수를 반환하는 함수
5. 순수 함수 vs 비순수 함수 
    - 순수 함수 : 함수 호출 후 부수 효과가 없는 함수
    - 비순수 함수 : 부수 효과가 있는 함수
    - **순수 함수를 통해 프로그래밍 할 것!!!!**



# 13. 스코프(유효범위)

- 두 번째 부터 중복 선언된 var 변수는 var 가 무시됨.

### 스코프 체인

중첩함수(inner) → 외부 함수 (outer) → 전역의 순서로 변수 검색 

|블록 레벨 스코프 |함수 레벨 스코프 |
|------------------|------------|
|대부분 프로그래밍 언어, let, const | var | 
|블록 내부에서만 유효 | 함수 내부에서만 유효, 블록은 전역으로 처리 | 


- 동적 스코프 : 함수를 어디서 호출했는지에 따라 상위 스코프 결정 (이렇게 동작하는 언어는 거의 없음)
- 렉시컬 스코프 (정적 스코프) : 함수를 어디서 정의했는지에 따라 상위 스코프 결정

    예시 : [https://jsfiddle.net/MiaLee/3daeLjt9/4/](https://jsfiddle.net/MiaLee/3daeLjt9/4/)

# 14. 전역 변수의 문제점

### 전역 객체

- 코드가 실행되기 이전 단계에서 생성
- window, self, this, global 등 ⇒ globalThis 로 동일 (ES11)
- 프로퍼티 : 표준 빌트인 객체, 환경별 호스트 객체, var 전역 변수 및 전역 함수

```jsx
var foo = 'This is foo'
console.log(globalThis.foo)
console.log(window.foo)
```

- 참고) var 키워드가 생략된건...? 전역변수!

```jsx
function foo() {
    x = 1
}
foo()
console.log(x)    // 1
```

### 전역 변수 문제점

- 전역변수의 검색 속도가 가장 느림
- 파일이 분리되어도 하나의 전역 스코프 공유하여 오류 발생 가능성 높음

대책

- 모든 코드를 즉시 실행함수로~
- 네이스페이스 객체 사용~
- 모듈 패턴 (클래스처럼 사용)
- 모듈 <script type="module"

### 요약

- 지역변수는 함수의 생명주기와 일치
- 전역 변수는 전역 객체 생명 주기와 일치
- 호이스팅은 스코프 단위로 동작
- 변수의 스코프는 좁을 수록 좋음

# 15. let, const 키워드와 블록 레벨 스코프

- for 문에서 var 로 선언한 변수도 전역 변수가 됨.

### let

- 선언단계와 초기화 단계 분리 : 변수 선언문에서 초기화 함!
- let 전역 변수는 전역 객체 프로퍼티가 아님!

### const

- 선언과 동시에 초기화!!!
- 재할당 금지
- 가독성을 위해 자주 사용~