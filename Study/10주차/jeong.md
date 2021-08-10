# 38장 브라우저의 렌더링 과정

브라우저 렌더링 과정 

<img width="665" alt="스크린샷_2021-08-10_오후_12 13 30" src="https://user-images.githubusercontent.com/8978613/128868360-d99c6a9f-ea67-4899-a87c-2100623849fb.png">


1. 브라우저 리소스 요청 → 서버
2. 서버 → 리소스 응답
3. 브라우저 HTML, CSS 파싱 → DOM, CSSOM 생성 → 렌더 트리 생성 
4. 브라우저 자바스크립트 엔진 → 자바스크립트 파싱 → AST(Abstract Syntax tree) 생성 → 바이트 코드 변환 → DOM, CSSOM 변경 → 렌더트리로 결합
5. 렌더트리 → 레이아웃 계산 → 브라우저 화면에 요소 페인팅 

HTTP/1.1 vs HTTP/2.0

- 1.1 : 리소스 요청 개수에 비례하여 응답 시간 증가
- 2.0 : 다중 요청 / 응답 가능! 로드 속도가 약 50%정도 빠름

<img width="220" alt="http11" src="https://user-images.githubusercontent.com/8978613/128868575-89b4ef22-c394-4fb6-a74d-494059902977.png">

<img width="220" alt="http2" src="https://user-images.githubusercontent.com/8978613/128868584-bd476cae-4d0a-430e-8134-e1646da29022.png">

HTML 파싱 

HTML : 순수 텍스트 

DOM : HTML을 브라우저가 이해할 수 있는 자료구조로 파싱한 결과물 

<img width="599" alt="dom" src="https://user-images.githubusercontent.com/8978613/128868747-d03a4638-5da8-4238-b2bf-70c79db1ce52.png">

파싱 과정 

1. 바이트 응답. 
2. meta 태그의 charset 에 선언된 인코딩 방식에 의해 문자열 변환하여 HTML 문서 읽음
3. 문법적 의미를 갖는 최소단위인 토큰으로 분해
4. 각 토큰을 객체로 변환하여 노드를 생성 (4가지 요소)
5. HTML 요소간의 부자 관계를 반영한 트리 자료구조(DOM) 구성 

CSS  파싱

link / style 태그를 만나면 DOM생성 일시 중단 CSS 파일 로드 후 파싱! 파싱 완료 후 DOM 생성 재개

파싱 과정 

1. 바이트 응답 
2. 문자 변환 
3. 토큰 생성 
4. 노드 생성
5. CSSOM(css object model) 생성

렌더 트리 생성 

DOM + CSSOM ⇒ Render tree

- 브라우저 화면에 렌더링되는 노드만으로 구성
- DOM 트리와 1:1 대응되지 않음
- 비 시각적인 요소는 제외함.

<img width="567" alt="rendertree" src="https://user-images.githubusercontent.com/8978613/128868830-6f733f5c-fefd-4bd7-a4bc-dea49ab01fe7.png">

자바스크립트 파싱

- 자바스크립트 엔진 ⇒ 파싱 ⇒ AST(Abstract Syntax Tree) 생성 ⇒ 바이트코드 생성 ⇒ 실행

    <img width="934" alt="compile" src="https://user-images.githubusercontent.com/8978613/128868918-6fa9bbc1-66be-4952-bf48-77f0a6fff99c.png">

- JavaScript, 인터프리터 언어일까? : [https://oowgnoj.dev/review/advanced-js-1](https://oowgnoj.dev/review/advanced-js-1)
    - 결론은, 자바스크립트는 실행되는 플랫폼에 따라 인터프리팅과 컴파일이 혼합되어 사용된다. 이 방식은 자바스크립트의 성능을 크게 향상시켰다.

- DOM API 를 통해 DOM / CSSOM 변경된 경우 ⇒ 리플로우(레이아웃 계산) ⇒ 리페인트 (재결합된 렌더트리 기반으로 다시 페인트)

- script 태그를 만나면 HTML 파싱 중단 후 JS 파싱 및 실행 ⇒ body 아래에 배치해야함
    - DOM이 완성되지 않은 상태에서 자바스크립트가 DOM을 조작하면 에러가 발생할 수 있음
    - 자바스크립트 로딩 /파싱 / 실행으로 인해 HTML 요소들의 렌더링에 지장받는 일이 발생하지 않아 로딩 시간 단축
    - async : HTML 파싱과 외부 자바스크립트 파일의 로드가 비동기적으로 동시 진행. 자바스크립트 실행은 자바스크립트 파일 로드가 완료 된 직후 진행. 이 때 HTML 파싱 중단. 태그가 여러개일경우 순서 보장되지 않음
    - defer : HTML 파싱과 외부 자바스크립트 파일의 로드가 비동기적으로 동시 진행. HTML 파싱 완료된 직후 (DOMContentLoaded 이벤트 발생) 자바스크립트 파싱 과 실행

<img width="803" alt="process" src="https://user-images.githubusercontent.com/8978613/128869003-aa962d7e-e5f2-4a70-8d3f-7f4a9ecbe117.png">

- 네이버 D2 : 브라우저는 어떻게 동작하는가?

[https://d2.naver.com/helloworld/59361](https://d2.naver.com/helloworld/59361)

- 브라우저는 어떻게 동작하는가 + 발표

[https://helloinyong.tistory.com/286](https://helloinyong.tistory.com/286)

- Google 모던 웹 브라우저 들여다보기

[https://developers.google.com/web/updates/2018/09/inside-browser-part1?hl=ko](https://developers.google.com/web/updates/2018/09/inside-browser-part1?hl=ko)

# 39장 DOM

노드 객체들로 구성된 트리 자료구조. DOM 트리

- 12개의 노트타입. 중요한 노트타입 4가지 (문서, 요소, 어트리뷰트, 텍스트)

<img width="612" alt="domnode" src="https://user-images.githubusercontent.com/8978613/128869093-f308803b-3654-4920-a4b2-a5bc1fae6d5a.png">

1. 문서 노드 (Document node)

    : 최상위 루트 노드, document 객체, HTML 문서당 document 객체는 유일함. DOM  트리 노드에 접근 진입점 

2. 요소 노드 (Element node)

    : HTML 요소. 요소간 중첩으로 부자관계 가짐, 문서 구조 표현 

3. 어트리뷰트 노드 (Attribute node)

    : HTML 요소의 속성을 가리키는 객체, 요소 노드에 연결, 요소에 접근하여 참조나 변경

4. 텍스트 노드 (Text node)

    : 문서의 정보를 표현, 요소노드 자식, 자식 노드 가질 수 없음 

- 노드 객체 상속 구조

Get Element Node  요소 노도 취득

1. getElementById : id 를 이용
    - id 값과 동일한 이름의 전역 변수가 암묵적으로 선언,
    - id 값과 동일한 이름의 전역변수가 할당되어 있으면 노드 객체가 재할당되지 않음 (변수 우선!)
2. getElementsByTagName : tag 이름 이용 
    - HTMLCollection 객체 반환 (유사배열객체 이터러블)
    - 모든 요소 취득 시 document.getElementsByTagName('*')
3. getElementsByClassName : 클래스 이름 이용 
    - HTMLCollection 객체 반환
4. querySelector :  css 선택자 하나의 노드 반환 
    - 만족시키는 첫 번째 노드 하나 반환
    - ex) document.querySelector('.banana')
5. querySelectorAll : css 선택자 모든 요소 반환 
    - NodeList (유사배열객체, 이터러블) 반환
    - 모든 요소 취득 시 document.querySelectorAll('*')
    - ex) documents.querySelectorAll('ul > li')
    - getElementBy*** 보다 느림
6. matches : css 선택자를 통해 요소를 취득할 수 있는지 확인