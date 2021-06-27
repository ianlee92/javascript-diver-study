# 12장 함수(https://www.notion.so/12-dcb50ace953041289e91fa4d53f71a40)
□ 함수 필요성

- 코드 재 사용 : 중복된 코드 제거
- 유지보수 편의성/코드 신뢰성 : 함수 코드 수정 호출한 모든 곳 적용
- 가독성 : 함수명( 식별자 ) 기능 예측

□ 함수의 구성 요소

- 일련의 과정( 하나의 실행 단위 ) 함수로 정의

□ 함수 정의
```jsx
/**
 * 함수 선언문( statement ) : 콘솔 창 완료 값 undefined
 */
add(1, 2); // 함수 호이스팅 : 선언 전 호출 가능 > 함수 표현식 사용 권장
function add(x, y) { // 기명 함수 : 함수명 필수
    return x+y;
}

/**
 * 함수 리터럴( 일급 객체 ) : 자바스크립트의 함수 객체 타입의 값
 * 함수 표현식 : 함수 리터럴 변수( 식별자 ) 대입
 */
// var add = function add(x, y) {
var add = function(x, y) { // 익명 함수 : 함수명 옵션
    return x+y;
};
add(1, 2); // 식별자( 변수 ) 함수 호출

/**
 * Function 생성자 함수 : 객체 생성 함수 > 함수 생성 사용 금지( 클로저 동작 않함 )
 */
var add = new Function('x', 'y', 'return x+y');
add(1, 3);

/**
 * 화살표 함수( ES6 ) : 익명함수 정의+function 키워드 => 대체
 */
const add = (x, y) => x+y;
add(1, 3);
```

□ 함수 호출

- 매개변수( parameter )와 인자 : 함수 외부 값 내부로 전달
    - 스코프( 유효 범위 ) : 함수 내부
    - 매개변수 미 전달 : undefined
    - 매개변수 초과 전달 : arguments 객체 프로퍼티 전달 > 가변 인자 함수 구현 사용

```jsx
// 데이터 타입 오류 대응 로직
function add(x, y) {
    // 데이터 타입 체크 + 타입스크립트 컴파일 시점 타입 체크 가능
    if(typeof x !== 'number' || typeof y !== 'number') {
        throw new TypeError('숫자 값 전달 가능');
    }

    return x+y; // 반환문( return ) : 함수 실행 중단 + 값 반환 > 반환 값 없는 경우 undefined 반환
}
add('1', '2');

// 매개변수 미 전달 대응 로직
function add(x, y) {
    // 매개변수 기본 값 할당
    x = x || 0;
    y = y || 0;

    return x+y
}
// ES6 매개변수 기본 값 할당
function add(x=0, y=0) {
    return x+y
}
add(1);

// 매개변수 개수 제한 : 3개 이상 객체 전달( 매개변수 순서 불 필요 )
// 주의 사항 : 객체 값 공유( call by reference ) 함수 내부 변경 시 외부 영향( 부수 효과/side effect )
$.ajax({
    method: 'POST',
    url: '/path',
    data: { id: 1, name: 'Lee' },
    cache: false
});
```

□ 원시 타입( call by value )과 객체 타입( call by reference ) 매개변수

- 객체 타입 단점 : 상태 변화 추적 어려움( 코드 복잡성 증가/가독성 어려움 )
- 해결 방법 : 옵저버 패턴 또는 불변 객체( 깊은 복사 새 객체 전달 ) 사용

□ 함수의 종류

- 즉시 실행 함수 : 정의와 동시에 단 한번만 즉시 실행

```jsx
// 즉시 실행 함수 : 그룹연산자( 함수 리터럴 평가 객체 생성 ) + 익명 함수 사용
// 변수나 함수명 충돌 방지 사용
(function() {
    return console.log('즉시실행함수');
}());
```

- 재귀 함수 : 함수 자신 반복 호출( recursive call )

```jsx
// 반복문
function countDown(n) {
    for(var i = n; i >= 0; i--) {
        console.log(i);
    }
}
countDown(5);

// 재귀 함수 : 반복문보다 가독성 좋을 경우만 제한적 사용
function countDown(n) {
    if(n < 0) return; //반복 중단 조건
    console.log(n);
    
    countDown(n-1); //재귀 호출
}
countDown(5);
```

- 중첩 함수( helper function ) : 외부 함수 돕는 용도 내부에서만 호출 가능

```jsx
// 중첩 함수 : 스코프와 클로저 관련
function outer() {
    var x = 1

    function inner() {
        var y = 2;
        console.log(x+y); //외부 함수 변수 
    }
    inner();
}
outer();
```

- 콜백 함수 : 매개변수 함수( 일급 객체 ) 전달 코드 종속성 제거
    - 비동기 처리 활용 패턴 : ajax, 이벤트, 타이머 함수 등
    - 배열 고차 함수 사용 : map, filter, reduce함수

```jsx
// 함수형 프로그래밍
// 고차 함수( 공통 기능 함수 ) : 함수 매개 변수 전달 받거나 반환
function repeat(n, f) { // 콜백 함수 전달
    for(var i=0; i<n; i++) {
        f(i); // 콜백 함수 내부 호출
    }
}

// 콜백 함수( 개별 기능 함수 )
var logAll = function(i) {
    console.log(i); // 전체 로그 
}
repeat(5, logAll);

// 익명 함수 전달 : 해당 함수 내부에만 호출
repeat(5, function(i) {
	if(i%2) console.log(i); // 홀수 로그
});
```

□ 순수 함수와 비 순수 함수

- 함수형 프로그래밍( immutability ) : 순수 함수 + 헬퍼 함수 사용 종속성 제거
    - 순수 함수( 사용 권장 ) : 매개 변수 값 복사 사용 > 외부 상태 영향도 없음
    - 비 순수 함수 :  매개 변수 객체 전달 사용 + 전역( 외부 ) 변수 접근 > 외부 상태 영향

---

# 13장 스코프(https://www.notion.so/13-Scope-4d2c7ca4b3b8411fa1b87d1c83ff331b)
□ 스코프( Scope )

- 유효 범위 : 모든 식별자( 변수, 함수, 클래스 등 )  **선언 위치 따른 유효 범위 결정**
    - 전역 스코프 : 코드 가장 바깥 영역 변수 선언 > 전역 참조
    - 지역 스코프 : 함수 내부 영역 변수 선언 > 선언 함수 내부 참조

□ 스코프 체인

- 식별자 검색 방법 : 상위 스코프 방향으로 이동하며 식별자 검색

□ 함수 상위 스코프 결정 방법

- **렉시컬/정적( lexical/static ) 스코프** : 함수 선언 시점 상위 스코프 결정
- 동적( dynamic ) 스코프 : 함수 호출 시점 상위 스코프 결정

```jsx
var x = 1;
function foo() {
    var x = 10;
    bar();
}

function bar() {
    console.log(x);
}

// 자바스크립트 렉시컬/정적 스코프 : 함수 선언 시점 사위 스코프 결정
foo(); // 1 
bar(); // 1
```

---

# 14장 전역 변수의 문제점(https://www.notion.so/14-5512cce225d04ae881252b3dc2c9ad3b)
□ 스코프( 유효 범위 ) 단위 호이스팅

```jsx
var x = 'global';

function foo() {
    console.log(x); // 지역 scope 호이스팅
    var x = 'local';
}

foo(); // undefined
console.log(x); // global
```

□ 변수의 생명 주기

- 지역 변수 : 함수 생명 주기 일치
- 전역 변수 : 전역( 브라우저 window ) 객체 생명 주기 일치

□ 전역 변수 문제점

- 암묵적 결합( implicit coupling ) 허용 : 모든 코드가 변수 참조 및 변경 가능 > 코드 가독성 하락, 의도치 않은 상태 변경
- 긴 생명 주기 : 메모리 리소스 오래 사용
- 스코프 체인 종점 : 변수 검색 속도 느림
- 네임스페이스 오염 : 파일 분리( 모듈화 ) 전역 스코프 공유

□ 전역 변수 억제 방법

- 즉시 실행 함수 : 모든 변수 익명 함수의 지역 변수 포함

```jsx
(function() {
    var foo = 1;
}());

console.log(foo); //ReferenceError
```

- 네임스페이스 객체 : 하나의 전역 객체( 변수 ) 생성 후 프로퍼티 포함

```jsx
var MYAPP = {};

MYAPP.person = {
	name: 'Lee',
	address: 'Seoul'
}
console.log(MYAPP.person.name);
```

- 모듈 패턴( 클래스 모방 )
    - 연관된 변수( 상태 )와 함수( 동작 ) 그룹핑
    - 캡슐화( 정보 은닉 ) : 외부 접근으로부터 내부 보호

```jsx
var Counter = (function() {
    var num = 0; // private 변수 : 내부만 접근

    return {
        increase() {
            return ++num;
        },
        decrease() {
            return --num;
        },
    };
}());

console.log(Counter.num);        // 외부 접근 불 가능
console.log(Counter.increase()); // 메소드 이용 접근 가능
console.log(Counter.decrease());
```

- ES6 모듈 : 전역 변수( window 프로퍼티 ) 사용 불 가능 > 파일 별 모듈 스코프 제공

```jsx
<script type="module" src="모듈파일명.mjs"></script>
```

---


# 15장 let, const 키워드와 블록 레벨 스코프(https://www.notion.so/15-let-const-92b50d4c69a44a60b2102c26d2540fbf)
□ var 변수 문제점

- 중복 선언 허용 : 의도치 않은 중복 선언 및 값 변경 가능성

```jsx
var x = 1;
var x = 100;
console.log(x);

// let
let x = 1;
var x = 100; // 중복 선언 에러 발생
console.log(x);
```

- 함수 레벨 스코프 : 의도치 않은 전역 변수+ 중복 선언 및 값 변경 가능성

```jsx
// 함수 내부 선언 변수만 지역 변수
var i = 10;
for (var i = 0; i < 5; i++) { // if, for문 블록 전역 변수 > 중복 선언
    console.log(i); // 전역 변수 i 출력
}
console.log(i); // 전역 변수 i 출력

// let
let i = 10;
for (let i = 0; i < 5; i++) { // 블록 레벨 스코프 지역 변수
    console.log(i); // 지역 변수 i 출력
}
console.log(i); // 전역 변수 i 출력

```

- 변수 호이스팅 : 선언 전 사용 가능 에러 발생하지 않음

```jsx
console.log(foo); // undefined
var foo = 1;
console.log(foo); // 1

// let
console.log(foo); // 미 선언 오류 발생
let foo = 1;
console.log(foo);

// let 호이스팅 발생하지 않는 것처럼 동작
let foo = 1;
{
    console.log(foo); // let 호이스팅 발생 > 전역 변수 참조 X
    let foo = 2;
}
```

□ 브라우저 전역 객체 window

- var 전역 변수 : window 전역 객체 프로퍼티
- let, const 전역 변수 : window 전역 객체 프로퍼티 아님

□ 상수 const

```jsx
// 가독성 : 프로그램 전체 고정 값 상수명 의미 부여 및 변경 방지
const STUDY_NAME = '자바스크립트 다이버'; // 선언과 동시 초기화 필수, 초기 값 변경 시 참조한 모든 곳 적용
STUDY_NAME = 'vue react'; // 오류 발생 재 할당 불 가능

// const 변수 재 할당 금지 의미 > 불변 값 의미하지 않음
const study = {
	name: '자바스크립트 다이버'  
}
study.name = 'vue react'; 
console.log(study.name);  // 객체 값 변경 가능
```

□ ES6 변수 선언 방법

- var : 미 사용
- const : 기본 변수 선언
- let : 변수 재 할당 필요한 경우 사용
