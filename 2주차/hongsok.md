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
