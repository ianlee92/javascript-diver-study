- **12장: 함수**
    - 함수 : 일련의 과정을 문으로 구현하고, 코드 블록으로 감싸서, 하나의 실행 단위로 정의한 것

        ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a95a60cc-524c-43ac-a70e-19883649e047/.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a95a60cc-524c-43ac-a70e-19883649e047/.png)

        ([이미지 출처](https://user-images.githubusercontent.com/31315644/66568713-f6d79c00-eba4-11e9-8280-018cc5ef6b77.jpeg))

    ```jsx
    // 함수는 객체 타입의 값. 함수 리터럴로 생성 가능
    var f = function add(x, y) {
    	return x + y;
    };
    console.log(f);
    // ƒ add(x, y) {
    // 	return x + y;
    // }
    ```

    → 함수는 객체지만 일반 객체와 다르다. 일반 객체는 호출할 수 없지만, 함수는 호출 가능.

    → 자바스크립트에서 함수가 객체라는 사실은 다른 언어와 구별되는 중요한 특징.

    - 함수 정의 4가지 방식

        ```jsx
        // 함수 선언문 (함수 선언문은 식별자가 생성되고 함수 객체가 할당된다)
        function add(x, y) { return x + y; }

        // 함수 표현식
        var add = function (x,y) { return x + y; };

        // Function 생성자 함수
        var add = new Function('x', 'y', 'return x+y');

        // 화살표 함수(ES6)
        var add = (x,y) => x + y;
        ```

        → 정의된 함수는 자바스크립트 엔진에 의해 평가되어 함수 객체가 된다. (일급 객체)

    - 함수 선언문 : 함수 리터럴과 같은 형태지만 함수 이름 생략 불가. (표현식이 아닌 문)

        ```jsx
        // 함수 선언 및 참조
        function add(x, y) { 
        	return x + y; 
        }
        // console.dir은 브라우저 환경에서 함수 객체의 프로퍼티까지 출력
        console.dir(add); // f add(x, y)

        // 함수 호출
        console.log(add(2, 5)); // 7
        ```

        ```jsx
        // 함수 선언문은 표현식이 아닌 문
        // 그러나 변수에 할당하거나 피연산자로 사용하면 함수 리터럴 표현식으로 해석
        var add = function add(x, y) { // 함수 add()는 함수 리터럴 표현식으로 해석됨.
        	return x + y;
        };
        console.log(add(2, 5)); // 7

        // 기명 함수 리터럴을 단독으로 사용하면 함수 선언문으로 해석. (이름 생략 불가)
        function foo() { console.log('foo'); }

        // 함수 리터럴을 피연산자로 사용하면 함수 리터럴 표현식으로 해석. (이름 생략 가능)
        // 그룹 연산자 () 안에 있으므로 함수 bar()는 피연산자가 된 것
        (function bar() { console.log('bar'); });
        ```

        → 함수 이름은 함수 몸체 내에서만 참조할 수 있는 식별자이므로 bar 함수는 호출 불가.

        → 그러나 foo 함수는 자바스크립트 엔진이 암묵적으로 생성한 식별자이므로 호출 가능.

        (자바스크립트 엔진은 함수 선언문을 해석해 함수 객체를 생성한다.)

    - 함수 표현식

        ```jsx
        // 함수 선언문으로 해석되는 foo 함수를 의사 코드로 표현하면 아래와 같다.
        // 자바스크립트 함수는 일급 객체이므로 변수 할당 가능
        // 이런 함수 정의 방식이 함수 표현식이다.
        var add = function add(x, y) { // 자바스크립트 엔진이 암묵적으로 add 식별자 생성
        	return x + y;
        }
        console.log(add(2, 5)); // 7
        ```

        → 결론적으로 자바스크립트 엔진은 함수 선언문을 함수 표현식으로 변환해 함수 객체 생성.

        → 함수 선언문은 "표현식이 아닌 문", 함수 표현식은 "표현식인 문"

    - 함수 생성 시점과 함수 호이스팅

        ```jsx
        // 함수 참조
        console.dir(add); // f add(x, y)
        console.dir(sub); // undefined

        // 함수 호출
        console.log(add(2, 5)); // 7
        console.log(sub(2, 5)); // TypeError: sub is not a function

        // 함수 선언문
        function add(x, y) { return x + y; }

        // 함수 표현식
        var sub = function (x, y) { return x - y; };
        ```

        → 함수 선언문으로 정의한 함수와 함수 표현식으로 정의한 함수는 생성 시점이 다름

        → 함수 선언문은 모든 선언문이 그렇듯 호이스팅 된다. (런타임 이전에 함수 객체 생성됨)

        (var 변수는 undefined로 초기화, 함수 선언문은 함수 객체로 초기화되어 호출 가능)

        → 함수 표현식은 변수 선언문 + 변수 할당문 축약 표현과 동일하게 동작.

        → 함수 표현식으로 함수 정의하면 함수 호이스팅이 아닌 변수 호이스팅 발생.

        (변수 할당문의 값은 런타임에 평가됨, 함수 리터럴도 런타임에 평가되어 함수 객체가 됨)

        → 함수 표현식 이전에 함수 참조하면 undefined를 호출하는 것과 마찬가지 (TypeError)

    - Function 생성자 함수

        ```jsx
        var add = new Function('x', 'y', 'return x + y');
        console.log(add(2, 5)); // 7
        ```

        → 주의할 점은 Function 생성자 함수는 함수 선언문이나 함수 표현식 함수와 다르게 동작.

    - 화살표 함수 (ES6) : 항상 익명 함수로 정의

        ```jsx
        // 화살표 함수
        const add = (x, y) => x + y;
        console.log(add(2, 5)); // 7
        ```

        → 표현 뿐만 아니라 내부 동작도 간략화.

    - 함수 호출 : 함수 가리키는 식별자와 함수 호출 연산자로 호출

        ```jsx
        // 함수 선언문
        function add(x, y) { return x + y; }

        // 함수 호출
        var result = add(1, 2); // 인수 (1, 2)가 매개변수 (x, y)에 할당되고 실행

        // 함수 호출할 때 인수 개수가 달라도 에러 발생 X
        console.log(add(2)); // NaN (2 + undefined)
        console.log(add(2, 5, 10)); // 7, 초과된 인수는 무시된다(프로퍼티로 보관 상태)
        ```

        → 매개변수는 함수 몸체 내부에서만 참조 가능. (매개변수의 스코프는 함수 내부)

        - 인수 확인 : 자바스크립트는 동적 타입 언어이므로 매개변수의 타입 지정 불가

        ```jsx
        // 예제 : 적절한 인수가 전달되었는지 체크
        function add(x, y) {
        	if (typeof x !== 'number' || typeof y !== 'number') {
        		throw new TypeError('인수는 모두 숫자 값이어야 합니다.');
        	}
        	return x + y;
        }

        // 예제 : 인수 개수가 매개변수 개수와 일치하는지 체크 (단축 평가 활용)
        function add(a, b, c) {
        	a = a || 0;
        	b = b || 0;
        	c = c || 0;
        	return a + b + c;
        }

        // 예제 : 매개변수 기본값(ES6)
        function add(a = 0, b = 0, c = 0) {
        	return a + b + c;
        }
        ```

    - 참조에 의한 전달과 외부 상태의 변경
        - 원시 값은 값에 의한 전달, 객체는 참조에 의한 전달. 매개변수도 방식은 동일.

    - 다양한 함수의 형태

        ```jsx
        // 익명 즉시 실행 함수 (단 한번만 호출됨)
        (function () {
        	var a = 3;
        	var b = 5;
        	return a * b;
        }());

        // 기명 즉시 실행 함수 (그룹 연산자 (...) 내부에 있어 함수 리터럴로 평가됨)
        (function foo() {
        	var a = 3;
        	var b = 5;
        	return a * b;
        }());
        foo(); // ReferenceError

        // 즉시 실행 함수도 일반 함수처럼 값 반환 가능
        var res = (function () {
        	var a = 3;
        	var b = 5;
        	return a * b;
        }());
        console.log(res); // 15
        ```

    - 재귀 함수

        ```jsx
        function countdown(n) {
        	if (n < 0) return;
        	console.log(n);
        	countdown(n - 1); // 재귀 호출
        }
        countdown(10);

        function factorial(n) {
        	if (n <= 1) return 1;
        	return n * factorial(n - 1);
        } // 함수 이름은 함수 몸체 내부에서만 유효.
        ```

    - 콜백 함수 : 함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수

        (콜백 함수를 전달받은 함수를 고차 함수라고 한다)

        ```jsx
        // 익명 함수 리터럴을 콜백 함수로 전달
        repeat(5, function (i) {
        	if (i % 2) console.log(i);
        }); // 1 3
        ```

        → 고차 함수 repeat이 호출될 때마다 익명 함수는 평가되어 함수 객체를 생성

    ```jsx
    // 콜백 함수를 사용한 이벤트 처리
    document.getElementById('button').addEventListener('click', function() {
    	console.log('button clicked!');
    });

    // 콜백 함수를 사용한 비동기 처리
    setTimeout(function() {
    	console.log('1초 경과');
    }, 1000);

    // 콜백 함수를 사용하는 고차 함수
    var mapped = [1, 2, 3].map(function (item) { return item * 2; }); // [2, 4, 6]
    var filtered = [1, 2, 3].filter(function (item) { return item % 2; }); // [1, 3]
    var reduced = [1, 2, 3].reduce(function (acc, cur) { return acc + cur; }, 0); // 6
    ```

    - 순수 함수와 비순수 함수
        - 순수 함수 : 어떤 외부 상태에 의존하지도 않고 변경되지 않는, 부수 효과가 없는 함수

            (비순수 함수는 반대)

        ```jsx
        // 순수 함수, increase()는 외부 상태(count)에 의존하지 않고 변경하지도 않음
        var count = 0;
        function increase(n) { return ++n; }
        count = increase(count);
        console.log(count); // 1

        // 비순수 함수, increase()는 외부 상태(count)에 의존하고 값을 변경
        var count = 0;
        function increase() { return ++count; }
        increase();
        console.log(count); // 1
        ```

        → 함수가 외부 상태를 변경하면 상태 변화를 추적하기 어려워진다.

        → 함수형 프로그래밍은 외부 상태를 변경하는 부수 효과를 최소화하여 불변성 지향

- **13장: 스코프**
    - 스코프 : 모든 식별자는 선언된 위치에 따라 참조할 수 있는 유효 범위가 결정된다.

        → 스코프 내에서 식별자는 유일해야 하지만, 다른 스코프에는 같은 이름 사용 가능.

        ```jsx
        // var 변수는 같은 스코프 내 중복 가능(변수값 재할당)
        function foo() {
        	var x = 1;
        	var x = 2;
        	console.log(x); // 2
        }
        foo();

        // let이나 const 변수는 같은 스코프 내 중복 불가
        function bar() {
        	let x = 1;
        	let x = 2; // SyntaxError
        }
        bar();
        ```

    - 스코프 종류
        - 전역: 코드의 가장 바깥 영역 (전역 변수는 어디서든 참조 가능)
        - 지역: 함수 몸체 내부 (지역 변수는 자신의 스코프와 하위 스코프에서 유효)

    ![https://publizm.github.io/static/6187a030810f694489ecc9b10c5de786/aa03c/scope.jpg](https://publizm.github.io/static/6187a030810f694489ecc9b10c5de786/aa03c/scope.jpg)

    - 스코프 체인 : 함수는 중첩되면 스코프는 계층적 구조를 갖게 된다.

        → 위 예제에서 inner 함수 스코프의 상위는 outer 함수의 스코프, 더 상위는 전역 스코프.

        → 자바스크립트 엔진은 스코프 체인을 통해 상위로 이동하며(단방향) 선언된 변수를 검색.

    - 함수 레벨 스코프 : 지역 스코프는 코드 블록이 아닌 함수에서만 생성.

        → C, 자바 등 대부분은 함수 외에도 모든 코드 블록이 지역 스코프를 만듦 (블록 레벨 스코프)

        → var 변수는 오직 함수 몸체만을 지역 스코프로 인정 (함수 레벨 스코프)

        ```jsx
        var x = 1;
        if (true) { 
        	var x = 10; // var 변수는 코드 블록 내에 있어도 모두 전역 변수
        }
        console.log(x); // 10 (1에서 10으로 재할당)

        // 블록 레벨 스코프를 지원하는 언어에서는 for문의 i가 코드 블록에서만 유효
        // 자바스크립트에서 var로 선언된 변수는 함수의 코드 블록만 지역으로 인정
        // 그러므로 for문의 var i 는 전역 변수
        var i = 10;
        for (var i = 0; i < 5; i++) { // i 역시 전역 변수
        	console.log(i); // 0 1 2 3 4
        }
        console.log(i); // 5
        ```

    - 렉시컬 스코프 : 함수를 어디서 정의했는지에 따라 함수의 상위 스코프 결정

        (자바스크립트를 비롯한 대부분 언어는 렉시컬 스코프를 따름)

    - 동적 스코프 : 함수를 어디서 호출했는지에 따라 함수의 상위 스코프 결정

        ```jsx
        var x = 1;

        function foo() {
        	var x = 10;
        	bar(); // 1
        }

        // 자바스크립트는 렉시컬 스코프를 따름
        // bar 함수는 전역에서 정의. 어디서 호출되어도 항상 전역 스코프가 상위
        function bar() {
        	console.log(x); // 1
        }
        ```

- **14장: 전역 변수의 문제점**
    - 변수의 생명 주기 : 변수는 선언에 의해 생성되고, 할당을 통해 값을 갖는다.

        ```jsx
        function foo() {
        	var x = 'local';
        	console.log(x); // local
        	return x;
        }
        foo();
        console.log(x); // ReferenceError, 함수 호출 전에는 지역 변수 x는 생성되지 않음.
        ```

        → 지역 변수의 생명 주기는 함수의 생명 주기와 일치 (클로저 예외)

        ```jsx
        var x = 'global';

        function foo() {
        	console.log(x); // undefined, 함수 내부에서 x는 호이스팅됨
        	var x = 'local';
        }
        foo();
        console.log(x); // global
        ```

        → 예제를 통해 변수 선언이 끌어 올려지는 호이스팅이 스코프 단위로 동작함을 알 수 있다.

    - 전역 변수의 사용을 억제하는 방법 : 변수의 스코프는 좁을수록 좋다

        ```jsx
        // 즉시 실행 함수 : 단 한번만 호출
        (function() { 
        	var foo = 10; // 즉시 실행 함수의 지역 변수
        	// ...
        }());

        // 네임스페이스 객체
        var MYAPP = {}; // 전역 네임스페이스 객체
        MYAPP.name = 'Lee'; // 전역 변수처럼 사용하고 싶은 변수를 MYAPP 프로퍼티로 추가

        // 모듈 패턴 : 클래스 모방. 클로저 기반 동작. 캡슐화 구현 가능.
        var Counter = (function() {
        	var num = 0; // private
        	return { // 외부로 공개할 데이터나 메서드를 프로퍼티로 추가한 객체를 반환
        		increase() {
        			return ++num;
        		},
        		decrease() {
        			return --num;
        		}
        	};
        }());
        console.log(Counter.num); // undefined
        console.log(Counter.increase()); // 1
        console.log(Counter.decrease()); // 0

        // ES6 모듈 : ES6 모듈을 사용하면 더는 전역 변수를 사용할 수 없다. (독자적 모듈 스코프)
        <script type="module" src="lib.mjs"></script> // IE 포함 구형에서 동작 X
        ```

- **15장: let, const 키워드와 블록 레벨 스코프**
    - var 키워드로 선언한 변수의 문제점
        - 변수 중복 선언 허용
        - var 변수는 오직 함수의 코드 블록만을 지역 스코프로 인정
        - var 변수의 호이스팅은 프로그램 흐름에 맞지 않음

    - let 키워드
        - 변수 중복 선언 금지
        - 블록 레벨 스코드
        - let 변수 호이스팅은 선언과 초기화가 분리됨. (TDZ)

            (모든 선언은 호이스팅되지만 let, const, class 선언문은 호이스팅되지 않는 것처럼 동작)

    - const 키워드
        - 선언과 초기화 : 반드시 선언과 동시에 초기화해야 한다.
        - 재할당 금지
        - 상수
        - const 키워드와 객체 : const 변수에 할당된 객체는 값 변경 가능
