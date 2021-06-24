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