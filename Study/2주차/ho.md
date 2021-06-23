# 8장 제어문(https://www.notion.so/8-74b688d1891246d2861cafb94b724956)
□ 제어문

- 제어문( control flow statement ) : 순차적 실행되는 코드의 흐름 제어
    - 코드 가독성 하락 : forEach, map, filter, reduce 고차 함수 사용
- 블록문( block statement ) : 코드 실행 단위( 코드 블록 )

```jsx
// 블록문
{ console.log(1); } // 자체 종결성 세미콜론( ; ) 필요 없음

// 일반적 제어문/함수 선언 사용
if(조건식) { 실행 코드 } // true 코드 실행
for(조건식) { 반복 실행 코드 } // 고정 위치 코드 실행 
function 함수명() { 실행 코드 } // 호출 위치 코드 실행
```

□ 조건문

- if문 : 조건식( boolean 데이터 타입 )에 따라 코드 블록 실행
- switch문 : 표현식( 문자 or 숫자 데이터 타입 )에 따라 코드 블록 실행

```jsx
// if
if(조건식) true 실행 코드; // 가독성 : 한줄 코드 블록 생략

// if-else
if(조건식) {
	true 실행 코드
} else {
	false 실행 코드
}

// 삼항 조건 연산자 > if문 대체
(조건식) ? true 실행 코드 : false 실행 코드;

// else-if
if(조건식) {
	true 실행 코드
} else if(조건식) {
	true 실행 코드
} else {
	false 실행 코드
}

// switch
switch(표현식) {
	case 표현식:
		표현식 일치 실행 코드
		break;
	case 표현식:
		표현식 일치 실행 코드
		break;
	default:
		표현식 불 일치 실행 코드
}
```

□ 반복문( loop statement )

- 조건식( boolean 데이터 타입 )에 따라 코드 블록 반복 실행
    - 무한 반복 주의 : 반복 종료 조건 확인
- break문 : 실행 중 문 종료
- label문 : 반복문 식별자
    - 외부 반복문 종료 : break label;
- continue문 : 반복 실행 중 코드 건너뛰기

```jsx
// for
for(변수;조건식;증감식) { 
	반복 실행 코드
}

// 중첩 for
for(변수;조건식;증감식) { 
	외부 반복 실행 코드
	for(변수;조건식;증감식) {
			내부 반복 실행 코드
	}
	외부 반복 실행 코드
}

// while
while(조건식) { 
	반복 실행 코드 
}

// do-while
do { 
	반복 코드 한번은 반드시 실행
} while(조건식);
```

- 반복문 대체 기능
    - 배열 : forEach 메서드
    - 객체 : for-in문
    - 이터러블 : for-of문

---

# 9장 타입 변환과 단축 평가(https://www.notion.so/9-3284228f0d2c4cffa94aa92edfbfe1f5)
□ 타입( 형 ) 변환 종류

- 명시적 타입 변환( type casting ) : 개발자 의도적 타입 변환
- 암묵적 타입 변환( type coercion ) : 자바스크립트 엔진 자동 타입 변환
    - 올바른 사용 : 가독성 상승 > 10+''
    - 잘못된 사용 : 오류 발생 가능성

□ 암묵적 타입 변환

- 문자 타입 변환 : 문자 결합 연산자

- 숫자 타입 변환 : 산술 연산자

- boolean 타입 변환 : 조건식
    - falsy 값 > 이외의 모든 값 truthy 값

□ 명시적 타입 변환

- 타입 변환 방법
    - 생성자 함수 new 연산자 없이 호출
    - 빌트인 메서드 사용

□ 단축 평가

- 평가 결과 확정될 경우 우항 평가 과정 생략


- 단축 평가 활용
    - 논리곱 : null 또는 undefined 객체 프로퍼티 참조 오류 방지

    ```jsx
    var obj = null;
    var key = obj.key; // Uncaught TypeError: Cannot read property 'key' of null
    var key = obj && obj.key; // null
    ```

    - 논리합 : 매개 변수 기본 값 설정

    ```jsx
    function getStringLength(str) {
        str = str || '';
        return str.length;
    }

    // ES6 문법
    function getStringLength(str='') {
        return str.length;
    }

    getStringLength(); // 0
    getStringLength('hi'); // 2
    ```

□  단축 평가 대체 연산자( ES11/2020 )

- 옵셔널 체이닝 연산자( 논리곱 대체 ) : null > undefined 반환

- 필수 사용 : 원시 값  0또는 '' > 객체로 평가 되어야 할 경우

```jsx
var str = '';
var length = str && str.length; // '' > 원시 값 평가 false
var length = str?.length; // 0 > 객체 값 평가 true
console.log(length);
```

- null 병합 연산자( 논리합 대체 )

- 필수 사용 : 원시 값  0또는 '' > 객체로 평가 되어야 할 경우

```jsx
var str = '' || 'default'; // default > '' 원시 값 평가 false
var str = '' ?? 'default'; // '' > 객체 값 평가 true
console.log(str);
```

---

# 10장 객체 리터럴(https://www.notion.so/10-b600506667b74671999a08a21eb18689)
□ 객체( object ) 기반 프로그래밍 언어

- 원시 타입 값을 제외한 나머지 모든 값 객체 타입

□ 객체( data structur )

- 프로퍼티( data ) : **객체의 상태**를 나타내는 값
- 메서드( behavior ) : 프로퍼티를 **참조하고 조작**할 수 있는 동작


□ 객체 생성 방법

- 객체 리터럴 : { key:value }
- 생성자 함수
- Object 생성자 함수
- Object.create  메서드
- 클래스( ES6 )

□ 객체 리터럴 사용 방법

```jsx
/* literal 객체 생성 */
var Obj = {
    // 프로퍼티 key( String/Symbol 타입 ): value 구성
    key: 'value',        // 네이밍 규칙 준수 : '' 생략
    'k-e-y': 'value',    // 네이밍 규칙 미 준수 : '' 명시
    method: function() { // 메서드 : 함수( 일급 객체 ) 값으로 사용 가능
        console.log(this.key);
    }
}
console.log(Obj.key);      // 네이밍 규칙 준수 : 마침표 표기법
console.log(Obj['k-e-y']); // 네이밍 규칙 미 준수 : 대괄호 표기법
console.log(Obj.method());

/* 프로퍼티 key 동적 생성 */
// ES5
var obj = {};
var key = 'key';
obj[key] = 'value'; 
console.log(obj);
// ES6
var key = 'key';
var obj = {[key]: 'value'};
console.log(obj);

/* 프로퍼티 동적 생성 */
var obj = {};
obj.key = 'value';
console.log(obj.key);

/* 프로퍼티 값 변경 */
Obj.key = 1;
console.log(obj.key);

/* 프로퍼티 삭제 */
delete obj.key;
console.log(Obj.key); // 존재하지 않는 프로퍼티 undefined 반환 주의

/* 프로퍼티 key 에러 같은 문법( 사용 X ) */
var obj = {
    key: 1,
    key: 'value',  // key 중복 선언 가능( 덮어쓰기 )
    'var': 'value' // 프로퍼티 key 예약어 사용 가능
}

/**
 * 객체 리터럴 확장 기능
 */

/* 프로퍼티 축약 표현 */
// ES6 변수명 프로퍼티 key명 동일한 경우
let x = 1, y = 2;
const obj = {x, y};

// ES5 프로퍼티
var x = 1, y = 2;
var obj = {
    x: x,
    y: y
};

/* 계산된 프로퍼티명 */
// ES6
const prefix = 'porp';
let i = 0;
const obj = {
    [`${prefix}-${++i}`]: i,
    [`${prefix}-${++i}`]: i,
    [`${prefix}-${++i}`]: i
}
console.log(obj);

// ES5
var prefix = 'porp';
var i = 0;
var obj = {};
obj[prefix+'-'+(++i)] = i;
obj[prefix+'-'+(++i)] = i;
obj[prefix+'-'+(++i)] = i;
console.log(obj);

/* 메서드 축약 표현 */
// ES6
const obj = {
    name: 'Lee',
    sayHi() {
        console.log('Hi! '+this.name);
    }
}
obj.sayHi();

// ES5
var obj = {
    name: 'Lee',
    sayHi: function() {
        console.log('Hi! '+this.name);
    }
}
obj.sayHi();
```


---

# 11장 원시 값과 객체의 비교(https://www.notion.so/11-377a817d3aef42c09231db46a9239567)
□ 원시/객체 데이터 비교

- 원시 값
    - 변경 불가능한 값/불변성( immutable value/immutability )
    - 메모리 실제 값 저장
    - 변수 할당 값 복사( pass by value ) >  값 변경 영향 없음

- 객체
    - 변경 가능한 값( mutable value )
    - 메모리 참조 값( 메모리 주소 ) 저장
    - 변수 할당 값 참조( pass by reference ) > 값 공유 변경 영향

□ 객체 복사

- 얕은 복사 : 객체 리터럴만( 중첩 객체 제외 ) 복사
- 깊은 복사 : 객체 리터럴 프로퍼티 객체( 중첩 객체 )까지 복사
