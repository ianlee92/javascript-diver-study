□ 컴퓨터 구조

- CPU : 데이터 연산 장치
- 데이터 기억 장치( 2 진수 저장 )
    - 메모리 : 데이터 임시 기억 장치
    - 디스크 : 데이터 영구 기억 장치

□ 애플리케이션 기능

- **데이터 입력( Input ) 받아 처리 결과 출력( Output )**
    - 변수 : 하나의 값을 저장하기 위한 메모리 공간 식별자
    - 자료 구조( 배열 []과 객체 {} ) : 연관 데이터 그룹화( 구조화 ) 저장

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/082f8ff2-1fbb-4385-b43e-2c0d803600f0/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/082f8ff2-1fbb-4385-b43e-2c0d803600f0/Untitled.png)

□ 변수 정리

```jsx
/**
□ 변수 키워드
	- ES5 : var > 함수 레벨 스코프
	- ES6 : let, const > 블록 레벨 스코프

□ 변수와 상수
	- 변수( 재 할당 가능 ) : var, let
	- 상수( 재 할당 불 가능 ) : const
*/

// 변수 선언과 값( Data ) 할당
var 변수명; // 변수 선언 + undefined 암묵적 초기화( Initialization )
변수명 = 값; // 값 할당

var 변수명 = 값; // 변수 선언 및 초기 값 할당

/**
□ 변수 호이스팅
	1. 소스 코드 평가( Evaluation ) : 변수 선언+암묵적 초기화
	2. 소스 코드 실행( Runtime ) : 값 할당
*/

/**
□ **네이밍 중요성**
	- 목적 명확 변수 네이밍 코드 가독성 상승

□ 네이밍 규칙( Naming Rule )
	- 예약어 사용 불가
	- 숫자 시작 불가
	- _, $ 이외 특수 문자 사용 불가

□ 네이밍 컨벤션( Naming Convention )
	- 변수/함수 : var camelCase;
	- 클래스/생성자함수 : var PascalCase;
*/
```

□ 예약어( keyword )

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/adf150ba-0cf0-4c14-985d-bfe8a9e4b324/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/adf150ba-0cf0-4c14-985d-bfe8a9e4b324/Untitled.png)

□ garbage collector( 자동 메모리 관리 )

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8e1c3ce0-0f92-48a2-bdcb-33b18158aa67/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8e1c3ce0-0f92-48a2-bdcb-33b18158aa67/Untitled.png)
