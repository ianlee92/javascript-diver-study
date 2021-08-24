### 📅 2021년 8월 24일 화요일
# 📚 41장 타이머
- 자바스크립트는 타이머를 생성할 수 있는 타이머 함수 setTimeout과 setInterval, 타이머를 제거할 수 있는 타이머 함수 clearTimeout과 clearInterval을 제공한다.
- setTimout 함수의 콜백 함수는 타이머가 만료되면 단 한 번 호출되고, setInterval 함수의 콜백 함수는 타이머가 만료될 때마다 반복 호출된다. setTimeout 함수가 반환한 타이머 id를 clearTimeout 함수의 인수로 전달하여 타이머를 취소할 수 있다.

```jsx
// 1초(1000ms) 후 타이머가 만료되면 콜백 함수가 호출된다.
// 이때 콜백 함수에 'Lee'가 인수로 전달된다.
const timerId = setTimeout(name => console.log(`Hi! ${name}.`), 1000, 'Lee');

// clearTimeout 함수의 인수로 전달하여 타이머를 취소한다.
// 타이머가 취소되면 setTimout 함수의 콜백 함수가 실행되지 않는다.
clearTimeout(tiemrId);
```

- 디바운스(debounce)는 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정시간이 경과한 이후에 이벤트 핸들러가 한 번만 호출되도록 한다. 즉, 디바운스는 짧은 시간 간격으로 발생하는 이벤트를 그룹화해서 마지막에 한 번만 이벤트 핸들러가 호출되도록 한다. resize 이벤트 처리나 input 요소에 입력된 값으로 ajax 요청하는 **입력 필드 자동완성** UI 구현, **버튼 중복 클릭 방지** 처리 등에 유용하게 사용된다.
- 스로틀(throttle)은 짧은 시간 간격으로 이벤트가 연속해서 발생하더라도 일정 시간 간격으로 이벤트 핸들러가 최대 한 번만 호출되도록 한다. 즉, 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 일정시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만든다. scroll 이벤트 처리나 **무한 스크롤 UI 구현** 등에 유용하게 사용된다.

![https://blog.kakaocdn.net/dn/HwnKQ/btrcSsSMP6U/K23sD1sMnQVf1OLxLH86rK/img.gif](https://blog.kakaocdn.net/dn/HwnKQ/btrcSsSMP6U/K23sD1sMnQVf1OLxLH86rK/img.gif)
# 📚 42장 비동기 프로그래밍
- 함수의 실행 순서는 실행 컨텍스트 스택으로 관리한다. 자바스크립트 엔진은 단 하나의 실행 컨텍스트 스택을 갖는다. 현재 실행 중인 태스크가 종료할 때까지 다음에 실행될 태스크가 대기하는 방식을 동기(synchronous) 처리라고 하고 태스크가 종료되지 않은 상태라 해도 다음 태스크를 곧바로 실행하는 방식을 비동기(asynchronous) 처리라고 한다.
- 자바스크립트의 동시성(concurrency)을 지원하는 것이 이벤트 루프(event loop)다. 태스크 큐는 비동기 함수의 콜백 함수 또는 이벤트 핸들러가 일시적으로 보관되는 영역이다. 이벤트 루프는 콜 스택에 현재 실행 중인 실행 컨텍스트가 있는지, 그리고 태스크 큐에 대기 중인 함수가 있는지 반복해서 확인한다. 만약 콜 스택이 비어 있고 태스크 큐에 대기 중인 함수가 있다면 이벤트 루프는 순차적(FIFO)으로 태스크 큐에 대기 중인 함수를 콜 스택으로 이동시킨다.

    ![https://blog.kakaocdn.net/dn/dxnJGt/btrc3qrUact/opAKs4htIelLnVtRUCweD1/img.png](https://blog.kakaocdn.net/dn/dxnJGt/btrc3qrUact/opAKs4htIelLnVtRUCweD1/img.png)

- 비동기 함수인 setTimeout의 콜백 함수는 태스크 큐에 푸시되어 대기하다가 콜 스택이 비게 되면, 다시 말해 전역 코드 및 명시적으로 호출된 함수가 모두 종료하면 비로소 콜 스택에 푸시되어 실행된다.
- 자바스크립트는 싱글 스레드 방식으로 동작한다. 이때 싱글 스레드 방식으로 동작하는 것은 브라우저가 아니라 브라우저에 내장된 자바스크립트 엔진이다. 즉, 자바스크립트 엔진은 싱글 스레드로 동작하지만 브라우저는 멀티 스레드로 동작한다.

    ![https://blog.kakaocdn.net/dn/bp6Tod/btrcXk7zoOr/n7gwEb5KOZzqLcwKMk78W0/img.png](https://blog.kakaocdn.net/dn/bp6Tod/btrcXk7zoOr/n7gwEb5KOZzqLcwKMk78W0/img.png)

    ![https://blog.kakaocdn.net/dn/uUwgS/btrc3qZK8AU/3a00YcmKwsXSPB8EHigFlK/img.png](https://blog.kakaocdn.net/dn/uUwgS/btrc3qZK8AU/3a00YcmKwsXSPB8EHigFlK/img.png)
# 📚 43장 Ajax
- Ajax(Asynchronous JavaScript and XML)란 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식을 말한다. Ajax는 브라우저에게 제공하는 Web API인 XMLHttpRequest 객체를 기반으로 동작한다.

    ![https://blog.kakaocdn.net/dn/T96iI/btrc1vtTkRI/LtCTLAMToXtMGnJvsJUcA0/img.png](https://blog.kakaocdn.net/dn/T96iI/btrc1vtTkRI/LtCTLAMToXtMGnJvsJUcA0/img.png)

- JSON(JavaScript Object Notation)은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다. 자바스크립트에 종속되지 않는 언어 독립형 데이터 포맷으로, 대부분의 프로그래밍 언어에서 사용할 수 있다. JSON은 객체 리터럴과 유사하게 키와 값으로 구성된 순수한 텍스트로 키는 반드시 큰따옴표로 묶어야 한다. 값은 객체 리터럴과 같은 표기법을 그대로 사용할 수 있지만 문자열은 반드시 큰따옴표로 묶어야 한다.

    ```jsx
    {
      "name": "Lee",
      "age": 20,
      "alive": true,
      "hobby": ["traveling", "tennis"]
    }
    ```

- JSON.stringify 메서드는 객체를 JSON 포맷의 문자열로 변환하고(직렬화) JSON.parse 메서드는 JSON 포맷의 문자열을 객체로 변환한다.(역직렬화)
- 브라우저는 주소창이나 HTML의 form 태그 또는 a 태그를 통해 HTTP 요청 전송 기능을 기본 제공한다. 자바스크립트를 사용하여 HTTP 요청을 전송하려면 Web API인 XMLHttpReqeust 객체를 사용한다.

### 📅 2021년 8월 26일 목요일
# 📚 44장 REST API
# 📚 45장 프로미스
