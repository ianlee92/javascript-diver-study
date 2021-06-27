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
