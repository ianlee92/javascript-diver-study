### 📅 2021년 8월 31일 화요일
# 📚 46장 제너레이터와 async/await
- ES6에서 도입된 제너레이터(generator)는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수다. 제너레이터 함수는 function* (애스터리스크) 키워드로 선언하고 하나 이상의 yield 표현식을 포함한다. 이것을 제외하면 일반 함수를 정의하는 방법과 같다.
- 제너레이터 함수는 화살표 함수로 정의할 수 없고 new 연산자와 함께 생성자 함수로 호출할 수 없다. 제너레이터 함수를 호출하면 제너레이터 객체를 생성해 반환한다. 제너레이터 함수가 반환한 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.

    ![https://blog.kakaocdn.net/dn/pqTVs/btrdJ33M1ca/hqRtWhEugguBZFt7Rw8r20/img.png](https://blog.kakaocdn.net/dn/pqTVs/btrdJ33M1ca/hqRtWhEugguBZFt7Rw8r20/img.png)

- 제너레이터 객체의 next 메서드를 호출하면 yield 표현식까지 실행되고 일시 중지된다. 이때 제너레이터 객체의 next 메서드는 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다. value 프로퍼티에는 yield 표현식에서 yield된 값이 할당되고 done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 불리언 값이 할당된다.

    ```jsx
    function* genFunc() {
      yield 1;
      yield 2;
      yield 3;
    }

    const generator = genFunc();
    console.log(generator.next()); // {value: 1, done: false}
    console.log(generator.next()); // {value: 2, done: false}
    console.log(generator.next()); // {value: 3, done: false}
    console.log(generator.next()); // {value: undefined, done: true}
    ```

- 제너레이터 객체의 next 메서드에 전달한 인수는 제너레이터 함수의 yield 표현식을 할당받는 변수에 할당된다.

    ```jsx
    function* genFunc() {
      const x = yield 1;
      const y = yield (x + 10);
      return x + y;
    }

    const generator = genFunc(0);

    // 처음 호출하는 next 메서드에는 인수를 전달하지 않고 전달하면 무시된다.
    let res = generator.next();
    console.log(res); // {value: 1, done: false}

    // next 메서드에 인수로 전달한 10은 genFunc 함수의 x 변수에 할당된다.
    res = generator.next(10);
    console.log(res); // {value: 20, done: false}

    // next 메서드에 인수로 전달한 10은 genFunc 함수의 y 변수에 할당된다.
    res = generator.next(20);
    console.log(res); // {value: 30, done: true}
    ```

- ES8에서는 제너레이터보다 간단하고 가독성 좋게 비동기 처리를 동기 처리처럼 동작하도록 구현할 수 있는 async/await가 도입되었다. async/await는 프로미스를 기반으로 동작한다. 프로미스의 후속 처리 메서드 없이 마치 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현할 수 있다.
- async/await에서 에러 처리는 try...catch 문을 사용할 수 있다. 콜백 함수를 인수로 전달받는 비동기 함수와는 달리 프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기 때문에 호출자가 명확하다.

    ```jsx
    const fetch = require('node-fetch');

    const foo = async () => {
      try {
        const wrongUrl = 'https://wrong.url';
        
        const response = await fetch(wrongUrl);
        const data = await response.json();
        console.log(data);
      } catch (err) {
        console.error(err); // TypeError: Failed to fetch
      }
    };

    foo();
    ```
# 📚 47장 에러 처리
- 기본적으로 에러 처리를 구현하는 방법은 크게 두 가지가 있다. 첫 번째는 예외적으로 반환하는 값(null 또는 -1)을 if문이나 단축 평가 또는 옵셔널 체이닝 연산자를 통해 확인해서 처리하는 방법과 에러 처리 코드를 미리 등록해 두고 에러가 발생하면 에러 처리 코드로 점프하도록 하는 방법이 있다. try ... catch ... finally 문은 두 번째 방법이다. try ... catch ... finally문으로 에러를 처리하면 프로그램이 강제 종료되지 않는다.

    ```jsx
    try {
      // 실행할 코드(에러가 발생할 가능성이 있는 코드)
    } catch (err) {
      // try 코드 블록에서 에러가 발생하면 이 코드 블록의 코드가 실행된다.
      // err에는 try 코드 블록에서 발생한 Error 객체가 전달된다.
      // 생략 가능하지만 catch문이 없는 try문은 의미가 없다.
    } finally {
      // 에러 발생과 상관없이 반드시 한 번 실행된다. (생략가능)
    }
    ```

- 에러를 발생시키려면 try 코드 블록에서 throw 문으로 에러 객체를 던져야 한다. 에러를 던지면 catch 문의 에러 변수가 생성되고 던져진 에러 객체가 할당된다. 그리고 catch 코드 블록이 실행되기 시작한다.

    ```jsx
    try {
      // 에러 객체를 던지면 catch 코드 블록이 실행되기 시작한다.
      throw new Error('something wrong'); // Error 생성자 함수
    } catch (error) {
      console.log(error);
    }
    ```
# 📚 48장 모듈
- 모듈(module)이란 애플리케이션을 구성하는 개별적 요소로서 재사용 가능한 코드 조각을 말한다. 모듈은 공개가 필요한 자산에 한정하여 명시적으로 선택적 공개가 가능하고 이를 export라 한다. 모듈 사용자는 모듈이 공개(export)한 자산 중 일부 또는 전체를 선택해 자신의 스코프 내로 불러들여 재사용할 수 있는데 이를 import라 한다.

    ![https://blog.kakaocdn.net/dn/dNCuSO/btrdrmxo4GO/RkCT7nQUYkzj61m3P2OkD0/img.png](https://blog.kakaocdn.net/dn/dNCuSO/btrdrmxo4GO/RkCT7nQUYkzj61m3P2OkD0/img.png)

- 모듈에서 하나의 값만 export한다면 default 키워드를 사용할 수 있다. default 키워드를 사용하는 경우 기본적으로 이름 없이 하나의 값을 export하고 var, let, const 키워드는 사용할 수 없으며, {} 없이 임의의 이름으로 import한다.

    ```jsx
    // lib.mjs
    export default const foo = () => {};
    // => SyntaxError: Unexpected token 'const'
    // export default () => {};

    export default x => x * x;

    // app.mjs
    import square from './lib.mjs';
    console.log(square(3)); // 9
    ```
# 📚 49장 Babel과 Webpack을 이용한 ES6+/ES.NEXT 개발 환경 구축
- 매년 새롭게 도입되는 ES6 이상의 버전(ES6+)과 제안 단계에 있는 ES 제안 사양(ES.NEXT)은 브라우저에 따라 지원율이 제각각이다. 트랜스파일러(trainspiler)인 Babel과 모듈 번들러(module bundler)인 Webpack을 이용하여 ES6+/ES.NEXT 개발 환경을 구축한다.
- Babel은 ES6+/ES.NEXT로 구현된 최신 사양의 소스코드를 IE 같은 구형 브라우저에서도 동작하는 ES5 사양의 소스코드로 변환(트랜스파일링)할 수 있다.
- Webpack은 의존 관계에 있는 자바스크립트, CSS, 이미지 등의 리소스들을 하나(또는 여러 개)의 파일로 번들링하는 모듈 번들러다. Webpack을 사용하면 별도의 모듈 로더가 필요 없고, HTML 파일에서 script 태그로 여러 개의 자바스크립트 파일을 로드해야 하는 번거로움도 사라진다.

    ![https://blog.kakaocdn.net/dn/kHaUR/btrdBrqYW6J/4waGRt5kEqok53bx59HbOk/img.png](https://blog.kakaocdn.net/dn/kHaUR/btrdBrqYW6J/4waGRt5kEqok53bx59HbOk/img.png)

- Webpack이 모듈을 번들링할 때 Babel을 사용하여 ES6+/ES.NEXT 사양의 소스코드를 ES5 사양의 소스코드로 트랜스파일링하도록 babel-loader를 설치한다. 트랜스파일링은 Babel이 수행하고 번들링은 Webpack이 수행한다.
- Babel을 사용하여 ES5 사양의 소스코드로 트랜스파일링해도 브라우저가 지원하지 않아 ES5 사양으로 대체할 수 없는 기능은 polyfill을 이용한다.
