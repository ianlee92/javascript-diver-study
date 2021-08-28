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
- REST(REpresentational State Transfer)의 기본 원칙을 성실히 지킨 서비스 디자인을 "RESTful"이라고 표현한다. 즉, REST는 HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처고, REST API는 REST를 기반으로 서비스 API를 구현한 것을 의미한다.
- URI는 리소스를 표현(동사보다는 명사)하고 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다. 주로 5가지 요청 메서드(GET, POST, PUT, PATCH, DELETE)를 사용하여 CRUD를 구현한다.

    ![https://blog.kakaocdn.net/dn/cjmU6y/btrc7OzdeOI/C2iOLoFT5z6DCb6ArUMnuk/img.png](https://blog.kakaocdn.net/dn/cjmU6y/btrc7OzdeOI/C2iOLoFT5z6DCb6ArUMnuk/img.png)

- load 이벤트는 요청이 성공적으로 완료된 경우 발생한다. status 프로퍼티 값이 200이면 정상적으로 응답된 상태이고, POST 요청 때 201(Created)일 때도 정상적으로 응답된 상태이다.
# 📚 45장 프로미스
- ES6에서 비동기 처리를 위한 또 다른 패턴으로 프로미스(Promise)를 도입했다. 프로미스는 전통적인 콜백 패턴이 가진 단점을 보완하며 비동기 처리 시점을 명확하게 표현할 수 있다는 장점이 있다.
- 비동기 함수 내부의 비동기로 동작하는 코드는 비동기 함수가 종료된 이후에 완료된다. 따라서 비동기 함수 내부의 비동기로 동작하는 코드에서 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않는다.
- 콜백 함수를 통해 비동기 처리 결과에 대한 후속 처리를 수행하는 비동기 함수가 비동기 처리 결과를 가지고 또다시 비동기 함수를 호출해야 한다면 콜백 함수 호출이 중첩되어 복잡도가 높아지는 현상이 발생하는데, 이를 콜백 헬(callback hell)이라 한다.

    ![https://blog.kakaocdn.net/dn/ElT5l/btrc1fetGXB/UYyKIilHk8b8gKsBbURJWK/img.png](https://blog.kakaocdn.net/dn/ElT5l/btrc1fetGXB/UYyKIilHk8b8gKsBbURJWK/img.png)

- Promise 생성자 함수는 비동기 처리를 수행할 콜백 함수를 인수로 전달받는데 이 콜백 함수는 resolve와 reject 함수를 인수로 전달받는다.

    ```jsx
    // 프로미스 생성
    const promise = new Promise((resolve, reject) => {
      // Promise 함수의 콜백 함수 내부에서 비동기 처리를 수행한다.
      if (/* 비동기 처리 성공 */) {
        resolve('result');
      } else { /* 비동기 처리 실패 */
        reject('failure reason');
      }
    });
    ```

- 비동기 처리가 성공하면 프로미스는 pending 상태에서 fulfilled 상태로 변화한다. 비동기 처리가 실패하면 프로미스는 pending 상태에서 rejected 상태로 변화한다. 즉, 프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체다.

    ![https://blog.kakaocdn.net/dn/brJOdT/btrc11Ntl9f/4DLUTwaPn2vLkO8YSkEOx1/img.png](https://blog.kakaocdn.net/dn/brJOdT/btrc11Ntl9f/4DLUTwaPn2vLkO8YSkEOx1/img.png)

프로미스의 후속 처리 메서드

- Promise.prototype.then: 두 개의 콜백 함수(fulfilled, rejected 상태인 경우 호출)를 인수로 전달받는다. 첫 번째 콜백 함수는 비동기 처리가 성공했을 때 호출되는 성공 처리 콜백 함수이며, 두 번째 콜백 함수는 비동기 처리가 실패했을 때 호출되는 실패 처리 콜백 함수다.

    ```jsx
    // fulfilled
    new Promise(resolve => resolve('fulfilled'))
      .then(v => console.log(v), e => console.error(e)); // fulfilled

    // rejected
    new Promise((_, reject) => reject(new Error('rejected')))
      .then(v => console.log(v), e => console.error(e)); // Error: rejected
    ```

- Promise.prototype.catch: 한 개의 콜백 함수(rejected 상태인 경우 호출)를 인수로 전달받는다.
- Promise.prototype.finally: 한 개의 콜백 함수를 인수로 전달받는다. 프로미스의 성공(fulfilled) 또는 실패(rejected)와 상관없이 무조건 한 번 호출된다.

    ```jsx
    const promiseGet = url => {
      return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        xhr.open('GET', url);
        xhr.send();
        
        xhr.onload = () => {
          if (xhr.status === 200) {
            // 성공적으로 응답을 전달받으면 resolve 함수를 호출한다.
            resolve(JSON.parse(xhr.response));
          } else {
            // 에러 처리를 위해 reject 함수를 호출한다.
            reject(new Error(xhr.status));
          }
        };
      });
    }

    // promiseGet 함수는 프로미스를 반환한다.
    promiseGet('https://jsonplaceholder.typicode.com/posts/1')
      .then(res => console.log(res))
      .catch(err => console.error(err))
      .finally(() => console.log('Bye!'));
    ```

- then, catch, finally 후속 처리 메서드는 언제나 프로미스를 반환하므로 연속적으로 호출할 수 있는데 이를 프로미스 체이닝(promise chaining)이라 한다.
- 프로미스는 프로미스 체이닝을 통해 비동기 처리 결과를 전달받아 후속 처리를 하므로 비동기 처리를 위한 콜백 패턴에서 발생하던 콜백 헬이 발생하지 않지만 프로미스도 콜백 패턴을 사용하므로 콜백 함수를 사용하지 않는 것은 아니다. 콜백 패턴은 가독성이 좋지 않기 때문에 ES8에서 도입된 async/await를 통해 해결할 수 있다.
- 프로미스의 후속 처리 메서드의 콜백 함수는 태스크 큐가 아니라 마이크로태스크 큐에 저장되며 태스크 큐보다 우선순위가 높다. 즉, 이벤트 루프는 콜 스택이 비면 먼저 마이크로태스크 큐에서 대기하고 있는 함수를 가져와 실행한 이후 마이크로태스크 큐가 비면 태스크 큐에서 대기하고 있는 함수를 가져와 실행한다.

    ```jsx
    setTimeout(() => console.log(1), 0);

    Promise.resolve()
      .then(() => console.log(2))
      .then(() => console.log(3));
      
    // 2 -> 3 -> 1의 순으로 출력된다.
    ```
