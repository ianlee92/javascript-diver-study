- **08장: 제어문**
    - **블록문** : 코드 블록. 하나의 실행 단위.
    - **조건문** (if...else 문, switch 문)
    - **반복문** (for, while, do...while)
    - **break 문** : 레이블 문, 반복문, switch 문의 코드 블럭에서만 사용 가능.

        ```jsx
        // 레이블 문 : 식별자가 붙은 문
        foo : {
        	console.log(1);
        	break foo;
        	console.log(2);
        }
        console.log('Done');
        // 1
        // Done

        // outer가 식별자인 레이블 for문
        outer: for (var i = 0; i < 3; i++) {
        	for (var j = 0; j < 3; j++) {
        		// i + j === 3 이면 outer 레이블 문 탈출
        		if (i + j === 3) break outer;
        		console.log(`inner [${i}, ${j}]`);
        	}
        }
        console.log('Done');
        // inner [0, 0]
        // inner [0, 1]
        // inner [0, 2]
        // inner [1, 0]
        // inner [1, 1]
        // Done
        ```

        - break문 예제 : 문자열에서 특정 문자의 인덱스 검색

            ```jsx
            var string = "Hello World";
            var search = 'l';
            var index;

            // 기존에 문자를 차례대로 순회하며 찾는 방법
            for (var i = 0; i < string.length; i++) {
            	if (string[i] === search) {
            		index = i;
            		break;
            	}
            }
            console.log(index); // 2

            // String.prototype.indexOf 메서드 활용 방법
            console.log(string.indexOf(search)); // 2
            ```

    - continue문 예제 : 문자열에서 특정 문자의 개수를 세기

        ```jsx
        var string = "Hello World";
        var search = 'l';
        var count;

        // 기존에 문자를 차례대로 순회하며 카운트하는 방법
        for (var i = 0; i < string.length; i++) {
        	if (string[i] !== search) continue;
        	count++;
        }
        console.log(count); // 3

        // String.prototype.match 메서드 활용 방법
        const regexp = new RegExp(search, 'g'); // RegExp 객체 이용한 정규표현식
        console.log(string.match(regexp).length); // 3
        ```

        ([자바스크립트 정규식](https://beomy.tistory.com/21))

- **09장: 타입 변환과 단축 평가**

    ```jsx
    var x = 10;
    var str = x.toString();
    console.log(str); // "10"
    ```

    ```jsx
    // 암묵적 타입 변환
    // 문자열 (하나 이상이 문자열인 경우 '+'는 문자열 연결 연산자로 동작)
    0 + '' // "0"
    true + '' // "true"
    null + '' // "null"
    (Symbol()) + '' // TypeError (Symbol 타입은 문자열로 변환할 수 없다)
    ({}) + '' // "[object Object]" 
    Math + '' // "[object Math]"
    [] + ''  // "" (배열)
    [10, 20] + '' // "10,20"
    (function(){}) + '' // "function(){}" 
    Array + '' // "function Array() { [...] }"
    `1 + 1 = ${1 + 1}` // 템플릿 리터럴의 표현식 삽입은 결과를 문자열 타입으로 암묵적 타입 변환함.

    // 숫자
    1 - '1' // 0
    1 * '10' // 10
    1 / 'one' // NaN
    '1' > 0 // true (비교 연산자는 크기를 비교하므로 숫자 타입으로 암묵적 타입 변환)

    +'' // 0
    +'0' // 0
    +'string' // NaN
    +true // 1
    +false // 0
    +null // 0
    +undefined // NaN
    +[] // 0, 빈 문자열(''), 빈 배열([]), null, false는 0으로 변환된다.
    +[10, 20] // NaN
    +(function(){}) // 객체와 비어 있지 않은 배열, undefined는 변환되지 않아 NaN

    // 불리언
    if ('') console.log('1'); // false
    if (true) console.log('2'); // true
    if (0) console.log('3');  // false
    if ('str') console.log('4'); // true
    if (null) console.log('5'); // false
    // 2 4
    // false, undefined, null, 0, -0, NaN, '' 는 false로 평가

    // false로 평가되는 값들에 !을 붙이면 true로 평가되어 모두 내부 코드가 실행되는 조건문이 된다.
    if (!false)
    if (!undefined)
    if (!null)
    if (!0)
    if (!NaN)
    if (!'')
    ```

    ```jsx
    // 명시적 타입 변환
    // 문자열
    String(1); // "1", 숫자->문자열, new 없이 String 생성자 함수 호출하는 방법
    (1).toString(); // "1", Object.prototype.toString 메서드 사용하는 방법
    1 + '' // "1" // 문자열 연결 연산자를 이용하는 방법

    // 숫자
    Number('0') // 0, new 없이 Number 생성자 함수 호출하는 방법
    parseInt('0') // 0, parseInt 혹은 parseFloat 함수 사용하는 방법(문자열만 가능)
    +'0'; // 0, +연산자 이용하는 방법
    '0' * 1; // 0, *연산자 이용하는 방법

    // 불리언
    Boolean(''); // false, Boolean 생성자 함수 호출 방법
    Boolean('false'); // true
    Boolean(false); // false
    Boolean(0); // false
    Boolean(NaN); // false
    // Boolean('문자열') 같은 경우 문자열이 있는지 없는지를 판별
    // Boolean(숫자) 경우 숫자인지 아닌지 판별
    Boolean(null); // false
    Boolean(undefined); // false

    !!''; // false, !연산자 두 번 사용하는 방법
    !!0; // false
    !!null; // false
    ```

    ```jsx
    // 단축 평가 : 표현식을 평가하는 도중에 평가 결과가 확정되면 나머지 과정 생략하는 것

    // 논리 연산자, 논리합(||) 또는 논리곱(&&)의 평가 결과는 불리언 값이 아닐 수도 있다.
    'Cat' && 'Dog'; // "Dog", 논리곱(&&)은 모두 true일 때 true 반환, 첫 번째 피연산자 반환
    'Cat' || 'Dog'; // "Cat", 논리합(||)은 둘 중 하나만 true여도 true 반환, 두 번째 반환

    // ||연산자는 true인 값 반환, 첫 번째 값이 true이면 그대로 반환하고 나머지 과정 생략
    true || false // true
    false || true // true
    false || false // false

    // 단축 평가로 변수에 값이 null이거나 undefined인지 체크하는 방법
    var elem = null;
    var value = elem && elem.value; // null, false&&false이므로 첫번째 값 반환

    function getStringLength(str) {
    	str = str || ''; // str이 undefined이면 빈 문자열인 ''의 길이 0을 출력
    	return str.length;
    }
    getStringLength(); // 0
    getStringLength('hi'); // 2

    // 옵셔널 체이닝 연산자 ?, ES11(ECMAScript2020), null 또는 undefined이면 undefined 반환
    var elem = null;
    var value = elem?.value; 
    console.log(value); // undefined

    var elem = null;
    var value = elem && elem.value; 
    console.log(value); // null

    // null 병합 연산자 ??, ES11, 기본값 설정할 때 유용
    var foo = null ?? 'default'; // 좌항이 null 또는 undefined이면 우항 반환
    console.log(foo); // "default"

    var foo = '' || 'default'; // false||true 이므로 true인 'default' 값 반환
    console.log(foo); // "default"

    // ?? 연산자는 false로 평가되는 값이라도 null 또는 undefined가 아니면 좌항 그대로 반환
    var foo = '' ?? 'default';
    console.log(foo); // ''
    ```



- **10장: 객체 리터럴**
    - 자바스크립트는 객체 기반의 프로그래밍 언어. 원시 값을 제외한 값은 모두 객체.
    - 원시 값은 변경 불가, 객체 값은 변경 가능.

    - 자바스크립트는 프로토타입 기반 객체지향 언어. 클래스 기반과 달리 객체 생성 방법 다양함.
    - 객체 생성 방법 : 객체 리터럴, Object 생성자 함수, 생성자 함수, Object.create, 클래스(ES6)

        ```jsx
        // 객체 생성 : 객체 리터럴 사용 방법, {} 내에 프로퍼티 정의
        var person = {
        	name: 'Lee', // 객체의 각 프로퍼티는 키와 값으로 구성
        	sayHello: function() {},
        	firstName: 'hongsok', // 식별자 네이밍 규칙을 준수하는 프로퍼티 키
        	'last-name': 'an', // 식별자 네이밍 규칙을 준수하지 않는 프로퍼티 키는 따옴표를 붙여야 함
        	last-name: 'an' // SyntaxError, 자바스크립트 엔진이 '-'를 연산자로 인식하기 때문
        };
        ```

        → 코드 블록과 다르다. 객체 리터럴은 값으로 평가되는 표현식이므로, 뒤에 세미콜론을 붙인다.

    - 자바스크립트에서 함수는 일급 객체이므로 값으로 취급, 프로퍼티의 값으로도 사용할 수 있다.

        ```jsx
        var circle = {
        	radius : 5, // 프로퍼티

        	getDiameter: function() { // 메서드
        		return 2 * this.radius;
        	}
        }

        console.log(circle.radius); // 5
        console.log(circle['radius']); // 5
        console.log(circle[radius]); // ReferenceError: radius is not defined
        console.log(circle.name); // undefined, 존재하지 않는 프로퍼티에 접근해도 에러 X

        circle.radius = 6; // 프로퍼티 값 갱신
        circle.color = 'red'; // 프로퍼티 동적 생성
        delete circle.color; // 프로퍼티 삭제

        ```

    - ES6부터 사용 가능한 객체 리터럴 확장 기능

        ```jsx
        let x = 1, y = 2;
        const obj = { x, y }; // {x: 1, y: 2}, 프로퍼티 키는 변수 이름으로 자동 생성
        ```

        ```jsx
        // 계산된 프로퍼티 이름(문자열로 타입 변환 가능한 값의 표현식으로 동적 생성한 프로퍼티 키)
        // ES5
        var prefix = 'prop';
        var obj = {};

        for (var i = 0; i < 3; i++) {
            obj[prefix + '-' + i] = i;
        };
        console.log(obj); // {prop-0: 0, prop-1: 1, prop-2: 2}

        // ES6
        const prefix = 'prop';
        const obj = {};

        for (let i = 0; i < 3; i++) {
            obj[`${prefix}-${i}`] = i
        }
        console.log(obj); // {prop-0: 0, prop-1: 1, prop-2: 2}
        ```

- **11장: 원시 값과 객체의 비교**
    1. 원시 값은 변경 불가 ↔ 객체 값은 변경 가능
    2. 원시 값을 변수에 할당하면 실제 값 저장 ↔ 객체를 변수에 할당하면 참조 값 저장
    3. 원시 값을 갖는 변수를 다른 변수에 할당하면 원시 값이 복사되어 전달 (값에 의한 전달, 깊은 복사)

        객체를 가리키는 변수를 다른 변수에 할당하면 참조 값이 복사되어 전달 (참조에 의한 전달, 얕은 복사)

        ([자바스크립트 참조 복사와 값 복사](https://wanna-b.tistory.com/18))

    ```jsx
    // 자바스크립트에서 문자열은 유사 배열 객체(배열처럼 인덱스 접근 가능, length 프로퍼티 갖고 있음)
    var str = 'hello';
    console.log(str[0]); // h
    console.log(str.length); // 5
    console.log(str.toUpperCase()); // HELLO

    str[0] = 'H'; // 문자열은 원시 값이기 때문에 각 인덱스의 문자를 변경할 수는 없다.(에러 발생 X)
    console.log(str); // hello
    ```

    ```jsx
    // 값에 의한 전달
    var score = 80;
    var copy = score;
    console.log(score, copy); // 80 80, 값이 복사되어 할당(두 개의 80)
    console.log(score === copy); // true

    score = 100;
    console.log(score, copy); // 100 80, 값이 복사되어 전달되었기 때문에 score 값만 변경됨.
    console.log(score === copy); // false
    ```

    → 엄밀히 따지면 변수에는 값이 전달되는 것이 아닌 값이 저장된 메모리 주소가 전달된 것.

    - 자바스크립트는 클래스 없이 객체 생성, 동적으로 프로퍼티와 메서드 추가 가능

        ```jsx
        // 자바스크립트의 객체는 변경 가능한 값
        // 할당이 이뤄지는 시점에 객체 리터럴 해석, 그 결과 객체가 생성됨.
        var person = { // person에 접근하면 객체의 참조 값에 접근
        	name: 'Lee'
        };
        ```

        → 객체는 변경이 가능하므로 재할당 없이 객체를 직접 수정 가능(프로퍼티 추가, 값 갱신, 삭제 등)
