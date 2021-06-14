### 📅 2021년 6월 8일 화요일
# 📚 4장 변수
- 변수(variable)는 하나의 값을 저장하기 위해 확보한 메모리 공간 자체 또는 그 메모리 공간을 식별하기 위해 붙인 이름을 말한다.
- 변수에 값을 저장하는 것을 할당(assignment)이라 하고, 변수에 저장된 값을 읽어 들이는 것을 참조(reference)라 한다.
- 변수 이름을 식별자(identifier) 라고도 한다. 식별자는 어떤 값을 구별해서 식별할 수 있는 고유한 이름을 말한다.
- 변수 선언이란 값을 저장하기 위한 메모리 공간을 확보(allocate)하고 변수 이름과 확보된 메모리 공간의 주소를 연결(name biding)해서 값을 저장할 수 있게 준비하는 것이다.
- var 키워드로 선언한 변수는 어떠한 값도 할당하지 않아도 undefined라는 값을 갖는다.
- 변수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 공유의 특징을 변수 호이스팅(variable hoisting)이라 한다. 변수 선언뿐 아니라 var, let, const, function, class 키워드를 사용해서 선언하는 모든 식별자(변수, 함수, 클래스 등)는 호이스팅된다. 모든 선언문은 런타임 이전 단계에서 먼저 실행되기 때문이다.
- 변수 선언은 소스코드가 순차적으로 실행되는 시점인 런타임 이전에 먼저 실행되지만 값의 할당은 소스코드가 순차적으로 실행되는 시점인 런타임에 실행된다.
- 변수에 값을 재할당하면 이전 값의 메모리 공간을 지우고 그 자리에 재할당 값을 저장하는 것이 아니라 새로운 메모리 공간을 확보하고 그 메모리 공간에 저장한다.
- 불필요한 값들은 가비지 콜렉터(garbage collector)에 의해 메모리에서 자동 해제된다. 단, 메모리에서 언제 해제될지는 예측할 수 없다.
- 일반적으로 변수나 함수의 이름에는 카멜 케이스(firstName)를 사용하고, 생성자 함수, 클래스의 이름에는 파스칼 케이스(FirstName)를 사용한다.
- var 키워드의 여러 단점 중에서 가장 대표적인 것은 함수 레벨 스코프(function-level scope)를 지원한다는 것이다. 이로 인해 의도치 않게 전역 변수가 선언되어 심각한 부작용이 발생하기도 한다.

> 💡 함수 레벨 스코프(function-level scope) VS 블록 레벨 스코프(block-level scope)

- 함수 레벨 스코프(function-level scope) : 함수 내에서 선언된 변수는 함수 내에서만 유효하며 함수 외부에서는 참조할 수 없다. 즉, 함수 내부에서 선언한 변수는 지역 변수이며 함수 외부에서 선언한 변수는 모두 전역 변수이다.
- 블록 레벨 스코프(block-level scope) : 모든 코드 블록(함수, if문, for문, while문, try/catch 문 등)내에서 선언된 변수는 코드 블록 내에서만 유효하며 코드 블록 외부에서는 참조할 수 없다. 즉, 코드 블록 내부에서 선언한 변수는 지역 변수이다.

    ```jsx
    // 함수 레벨 스코프 (var)
    var x = 0;
    {
      var x = 1;
      console.log(x); // 1
    }
    console.log(x); // 1

    // 블록 레벨 스코프 (let, const)
    let y = 0;
    {
      let y = 1;
      console.log(y); // 1
    }
    console.log(y); // 0

    const z = 0;
    {
      const z = 1;
      console.log(z); // 1
    }
    console.log(z); // 0
    ```
    
# 📚 5장 표현식과 문
- 리터럴(literal)은 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용해 값을 생성하는 표기법(notation)을 말한다. 자바스크립트 엔진은 코드가 실행되는 시점인 런타임(runtime)에 리터럴을 평가해 값을 생성한다. 즉 리터럴은 값을 생성하기 위해 미리 약속한 표기법이라고 할 수 있다.
- 표현식(expression)은 값으로 평가될 수 있는 문(statement)이다. 즉, 표현식이 평가되면 새로운 값을 생성하거나 기존값을 참조한다. 표현식은 리터럴, 식별자(변수, 함수 등의 이름), 연산자, 함수 호출 등의 조합으로 이뤄질 수 있다. 값으로 평가될 수 있는 문은 모두 표현식이다.
- 문(statement)은 프로그램을 구성하는 기본 단위이자 최소 실행 단위다. 문의 집합으로 이뤄진 것이 바로 프로그램이며, 문을 작성하고 순서에 맞게 나열하는 것이 프로그래밍이다.
- 문은 여러 토큰(token)으로 구성된다. 토큰이란 문법적인 의미를 가지며, 문법적으로 더 이상 나눌 수 없는 코드의 기본 요소를 의미한다.
- 문을 명령문이라고 부른다. 문이 실행되면 명령이 실행되고 무슨 일인가가 일어나게 된다. 문은 선언문, 할당문, 조건문, 반복문 등으로 구분할 수 있다.
- 세미콜론(;)은 문의 종료를 나타낸다. 문을 끝낼 때는 세미콜론을 붙여야 한다. 단, 0개 이상의 문을 중괄호로 묶은 코드 블록({ ... }) 뒤에는 세미콜론을 붙이지 않는다. (if문, for문, 함수 등) 이러한 코드 블록은 언제나 문의 종료를 의미하는 자체 종결성(self closing)을 갖기 때문이다.
- 세미콜론은 옵션이고 생략이 가능하다. 자바스크립트 엔진이 소스코드를 해석할 때 문의 끝이라고 예측되는 지점에 세미콜론을 자동으로 붙여주는 세미콜론 자동 삽입 기능(ASI: automatic semicolon insertion)이 암묵적으로 수행되기 때문이다.
- 문에는 표현식인 문과 표현식이 아닌 문이 있다. 표현식인 문은 값으로 평가되므로 변수에 할당할 수 있지만 표현식이 아닌 문은 값으로 평가할 수 없으므로 변수에 할당하면 에러가 발생한다.
- 변수 선언문은 표현식이 아닌 문이고 할당문은 그 자체가 표현식이지만 완전한 문이기도 하다. 즉, 할당문은 표현식인 문이다.

    ```jsx
    // 변수 선언문은 표현식이 아닌 문이다.
    var x;

    // 할당문은 표현식인 문이다.
    x = 100;
    ```

- 크롬 개발자 도구에서 표현식이 아닌 문을 실행하면 언제나 undefined를 출력한다. 이를 완료 값이라 한다. 개발자 도구에서 표현식이 아닌 문을 실행하면 완료 값 undefined를 출력한다. 표현식인 문은 평가된 값을 반환한다.

    ```jsx
    // 변수 선언문
    var foo = 10;
    undefined

    // 조건문
    if (true) {}
    undefined
    ```

    ```jsx
    var num = 10;
    undefined

    // 표현식은 평가된 값을 반환한다.
    // 표현식 문
    100 + num;
    110

    // 할당문
    num = 100;
    100
    ```

* * *

### 📅 2021년 6월 10일 목요일

# 📚 6장 데이터 타입
- 자바스크립트(ES6)는 7개의 데이터 타입을 제공한다. 원시 타입(숫자, 문자열, 불리언, undefined, null, 심벌) 6개와 객체 타입(객체, 함수, 배열 등)이다. 원시 타입의 값은 변경 불가능한 값(immutable value)이며 pass-by-value(값에 의한 전달)이다.
- C나 자바와 달리 자바스크립트는 독특하게 하나의 숫자 타입만 존재한다. 모든 수를 실수로 처리하며, 정수만 표기하기 위한 데이터 타입이 별도로 존재하지 않는다.

    ```jsx
    // 숫자 타입은 모두 실수로 처리된다.
    console.log(1 === 1.0); // true
    console.log(4 / 2); // 2
    console.log(3 / 2); // 1.5
    ```
> 💡 NaN(not-a-number)과 빌트인 함수 isNaN()
- NaN(not-a-number)은 산술 연산 불가로 자바스크립트는 대소문자를 구별하므로 NaN을 NAN, Nan, nan과 같이 표현하면 에러가 발생하니 주의해야 한다.
- 자바스크립트 코딩 테스트 숫자만 추출하는 문제(빌트인 함수 isNaN() 활용)

    ```jsx
    <script>
      function solution(str){
          let answer="";
          // isNaN() 숫자면 거짓, 숫자가 아니면 참
          for(let x of str){
              if(!isNaN(x)) answer+=x; // !isNaN() 숫자면 참
          }
          return parseInt(answer); // 0208 -> 208
      }
      let str="g0en2T0s8eSoft";
      console.log(solution(str)); // 208
    </script>
    ```
- 자바스크립트의 문자열은 원시 타입이며, 변경 불가능한 값(immutable value)이다. 아래의 예제에서 문자열 ‘Hello’와 ‘world’는 모두 메모리에 존재하고 있다. 변수 str은 문자열 ‘Hello’를 가리키고 있다가 문자열 ‘world’를 가리키도록 변경되었을 뿐이다.

    ```jsx
    var str = 'Hello';
    str = 'world';
    console.log(str); // 'world'
    ```

    아래 예제의 2행에서 String 객체의 slice() 메소드는 statement 변수에 저장된 문자열을 변경하는 것이 아니라 사실은 새로운 문자열을 생성하여 반환하고 있다. 그 이유는 문자열은 변경할 수 없는 immutable value이기 때문이다.

    ```jsx
    var statement = 'I am an immutable value'; // string은 immutable value
    var otherStr = statement.slice(8, 17);
    console.log(otherStr);   // 'immutable'
    console.log(statement);  // 'I am an immutable value'
    ```

    원시 타입 이외의 모든 값은 객체(Object) 타입이며 객체 타입은 변경 가능한 값(mutable value)이다. 즉, 객체는 새로운 값을 다시 만들 필요없이 직접 변경이 가능하다는 것이다.

> 💡 템플릿 리터럴(template literal)
- ES6부터 템플릿 리터럴(template literal)이라고 하는 새로운 문자열 표기법이 도입되었다. 런타임에 일반 문자열로 변환되어 처리되고 일반적인 따옴표 대신 백틱(``)을 사용해 표현한다.
1. 멀티라인 문자열(multi-line string) : 일반 문자열 내에서는 줄바꿈(개행)을 \n을 통해서 하지만 템플릿 리터럴에서는 이스케이프 시퀀스를 사용하지 않고도 줄바꿈이 허용되며, 모든 공백도 있는 그대로 적용된다.

    ```jsx
    var template = '<ul>\n\t<li><a href="#">Home</a></li>\n<ul>';

    var template = `<ul>
      <li><a href="#">Home</a></li>
    </ul>`;
    ```

2. 표현식 삽입(expression interpolation) : 문자열은 문자열 연산자 +를 사용해 연결할 수 있다. + 연산자는 피연산자 중 하나 이상이 문자열인 경우 문자열 연결 연산자로 동작하고 그 외의 경우는 덧셈 연산자로 동작한다. 표현식을 삽입하려면 ${ }으로 표현식을 감싼다. 이때 표현식의 평가 결과가 문자열이 아니더라도 문자열로 타입이 강제로 변환되어 삽입된다.

    ```jsx
    var first = 'Ian';
    var last = 'Lee';

    // ES5 문자열 연결
    console.log('My name is ' + first + ' ' + last + '.'); // My name is Ian Lee.

    // ES6 표현식 삽입
    console.log(`My name is ${first} ${last}.`); // My name is Ian Lee.
    ```

    표현식 삽입은 반드시 템플릿 리터럴 내에서 사용해야 한다. 템플릿 리터럴이 아닌 일반 문자열에서의 표현식 삽입은 문자열로 취급된다.

    ```jsx
    console.log(`1 + 2 = ${1 + 2}`); // 1 + 2 = 3
    console.log('1 + 2 = ${1 + 2}'); // 1 + 2 = ${1 + 2}
    ```

    ```jsx
    import React from "react";
    import BannerImage from "../assets/pizza.jpeg";

    function Home() {
      return (
        <div className="home" style={{ backgroundImage: `url(${BannerImage})` }}>
          ...
        </div>
      );
    }

    export default Home;
    ```

3. 태그드 템플릿(tagged template) : React styled-component에서 사용된다. 템플릿 리터럴 앞에 styled.h1이라는 객체의 속성이 붙어 있고 h1은 메서드(함수)다. 태그 함수라고도 부르고 함수 뒤에 템플릿 리터럴을 붙여 같이 사용한다.

    ```jsx
    const Title = styled.h1`
      font-size: 1.5em;
      text-align: center;
      color: palevioletred;
    `;
    ```

- 데이터 타입이 필요한 이유는 값을 저장할 때 확보해야 하는 메모리 공간의 크기, 값을 참조할 때 한 번에 읽어 들여야 할 메모리 공간의 크기, 메모리에서 읽어 들인 2진수를 어떻게 해석할지 결정하기 위해서이다.

> 💡 정적 타입 언어 vs 동적 타입 언어

1. 정적 타입 언어는 변수의 타입을 변경할 수 없으며, 변수에 선언한 타입에 맞는 값만 할당할 수 있다. 컴파일 시점에 타입 체크를 수행하고 통과하지 못했다면 에러를 발생시킴으로써 타입의 일관성을 강제하고 더욱 안정적인 코드의 구현을 통해 런타임에 발생하는 에러를 줄인다. 대표적으로 C, C++, 자바, 코틀린, 고 등이 있다.
2. 동적 타입 언어는 선언이 아닌 할당에 의해 타입이 결정(타입 추론)되고 재할당에 의해 변수의 타입은 언제든지 동적으로 변할 수 있다. 이러한 특징을 동적 타이핑이라 한다. 대표적으로 자바스크립트, 파이썬, PHP, 루비 등이 있다. 동적 타입 언어는 유연성은 높지만 신뢰성은 떨어진다.

# 📚 7장 연산자
- 단항 산술 연산자의 증가/감소(++/--) 연산자는 위치에 의미가 있다.

    ```jsx
    var x = 5, result;

    // 선대입 후증가 (Postfix increment operator)
    result = x++;
    console.log(result, x); // 5 6

    // 선증가 후대입 (Prefix increment operator)
    result = ++x;
    console.log(result, x); // 7 7

    // 선대입 후감소 (Postfix decrement operator)
    result = x--;
    console.log(result, x); // 7 6

    // 선감소 후대입 (Prefix decrement operator)
    result = --x;
    console.log(result, x); // 5 5
    ```

- 개발자의 의도와는 상관없이 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되기도 한다. 이를 암묵적 타입 변환(implicit coercion) 또는 타입 강제 변환(type coercion)이라고 한다.

    ```jsx
    // 단항 연산자
    +10 // 10
    +'10' // 10
    +true // 1
    +false // 0

    // 문자열 연결 연산자
    '1' + '2'      // '12'
    '1' + 2       // '12'

    // 산술 연산자
    1 + 2          // 3
    1 + true       // 2 (true → 1)
    1 + false      // 1 (false → 0)
    true + false    // 1 (true → 1 / false → 0)
    1 + null       // 1 (null → 0)
    1 + undefined // NaN (undefined → NaN)
    ```

- 동등 비교 연산자는 편리한 경우도 있지만 결과를 예측하기 어렵고 실수하기 쉽다. 동등 비교(==) 연산자 대신 일치 비교(===) 연산자를 사용하는 편이 좋다. 일치 비교(===) 연산자는 좌항과 우항의 피연산자가 타입도 같고 값도 같은 경우에 한하여 true를 반환한다. 다시 말해, 암묵적 타입 변환을 하지 않고 값을 비교한다. 따라서 일치 비교 연산자는 예측하기 쉽다.

```jsx
// NaN은 자신과 일치하지 않는 유일한 값이다.
NaN === NaN // false

// 숫자가 NaN인지 조사하려면 빌트인 함수 isNaN을 사용한다.
isNaN(NaN) // true

// 숫자 0도 주의하도록 하자.
0 === -0     // true
```

- ES6에서 도입된 Object.is 메서드는 예측 가능한 정확한 비교 결과를 반환한다. 그 외에는 일치 비교 연산자(===)와 동일하게 동작한다.

    ```jsx
    -0 === +0 // true
    Object.is(-0, +0); // false

    NaN === NaN; // false
    Object.is(NaN, NaN); // true
    ```
> 💡 삼항 조건 연산자 condition ? exprIfTrue : exprIfFalse
- 삼항 조건 연산자의 첫 번째 피연산자는 조건식이므로 삼항 조건 연산자 표현식은 조건문이다.

    ```jsx
    var x = 2;

    // 2 % 2는 0이고 0은 false로 암묵적 타입 변환된다.
    var result = x % 2 ? '홀수' : '짝수';

    console.log(result); // 짝수
    ```

- 삼항 조건 연산자 표현식은 값으로 평가할 수 있는 표현식인 문이다. 따라서 삼항 조건 연산자 표현식은 값처럼 다른 표현식의 일부가 될 수 있어 매우 유용하다. 조건에 따라 어떤 값을 결정해야 한다면 if ... else 문보다 삼항 조건 연산자 표현식을 사용하는 편이 유리하지만 조건에 따라 수행해야 할 문이 하나가 아니라 여러 개라면 if ... else 문의 가독성이 더 좋다.
    ```jsx
    function Navbar() {
      const [openLinks, setOpenLinks] = useState(false);
      const toggleNavbar = () => {
        setOpenLinks(!openLinks);
      };
      return (
        <div className="navbar">
          <div className="leftSide" id={openLinks ? "open" : "close"}>
          ...
        <button onClick={toggleNavbar} />
      );
    }
    ```
- typeof 연산자는 피연산자의 데이터 타입을 문자열로 반환한다. typeof 연산자는 7가지 문자열 "string", "number", "boolean", "undefined", "symbol", "object", "function" 중 하나를 반환한다. "null"을 반환하는 경우는 없다. 값이 null 타입인지 확인할 때는 typeof 연산자를 사용하지 말고 일치 연산자(===)를 사용하자. 선언하지 않은 식별자를 typeof 연산자로 연산해 보면 ReferenceError가 발생하지 않고 undefined를 반환한다.
- 부수 효과(다른 코드에 영향을 주는 효과)가 있는 연산자는 할당 연산자(=), 증가/감소 연산자(++,--), delete 연산자다.    
