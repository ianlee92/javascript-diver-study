### 📅 2021년 6월 15일 화요일
# 📚 8장 제어문
> 💡  indexOf 메서드
- 문자열에서 특정 문자의 인덱스(위치)를 검색

    ```jsx
    var string = 'Hello World.';
    var search = 'l';
    var index;

    // 문자열은 유사 배열이므로 for 문으로 순회할 수 있다.
    for(var i=0; i<string.length; i++) {
      // 문자열의 개별 문자가 'l'이면
    	if(string[i] === search) {
    		index = i;
    		break // 반복문 탈출
    	}
    }

    console.log(index); // 2

    //참고로 String.prototype.indexOf 메서드를 사용해도 같은 동작을 한다.
    console.log(string.indexOf(search)); // 2
    ```

- indexOf 메서드를 이용한 중복문자 제거 (코딩테스트)

    ```jsx
    <script>
      function solution(s){
        let answer="";
        for(let i=0; i<s.length; i++){
          console.log(s[i], i, s.indexOf(s[i]));
          // k 0 0, s 1 1, e 2 2, k 3 0, k 4 0, s 5 1, e 6 2, t 7 7
          if(s.indexOf(s[i])===i) answer+=s[i];
          // 최초의 문자 인덱스번호와 본래 위치가 일치할 때 첫번째로 발견된 것
        }
        return answer;
      }
      console.log(solution("ksekkset")); // kset
    </script>
    ```

- continue 문은 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 이동한다.

    ```jsx
    var string = 'Hello World.';
    var count = 0;

    // 문자열은 유사배열이므로 for 문으로 순회할 수 있다.
    for (var i = 0; i < string.length; i++) {
      // 'l'이 아니면 현 지점에서 실행을 중단하고 반복문의 증감식으로 이동한다.
      if (string[i] !== 'l') continue;
      count++; // continue 문이 실행되면 이 문은 실행되지 않는다.
    }

    console.log(count); // 3

    // 참고로 String.prototype.match 메소드를 사용해도 같은 동작을 한다.
    console.log(string.match(/l/g).length); // 3
    ```

- if 문 내에서 실행해야 할 코드가 한 줄이라면 continue 문을 사용했을 때보다 간편하며 가독성도 좋다. 하지만 if 문 내에서 실행해야 할 코드가 길다면 들여쓰기가 한 단계 더 깊어지므로 continue 문을 사용하는 것이 가독성이 더 좋다.

    ```jsx
    // continue 문을 사용하지 않으면 if 문 내에 코드를 작성해야 한다.
    for (var i = 0; i < string.length; i++) {
      // 'l'이면 카운트를 증가시킨다.
      if (string[i] === 'l') {
        count++;
        // code
        // code
        // code
      }
    }

    // continue 문을 사용하면 if 문 밖에 코드를 작성할 수 있다.
    for (var i = 0; i < string.length; i++) {
      // 'l'이 아니면 카운트를 증가시키지 않는다.
      if (string[i] !== 'l') continue;

      count++;
      // code
      // code
      // code
    }
    ```
# 📚 9장 타입 변환과 단축 평가
- 자바스크립트의 모든 값은 타입이 있다. 값의 타입은 다른 타입으로 개발자에 의해 의도적으로 변환할 수 있다. 또는 자바스크립트 엔진에 의해 암묵적으로 자동 변환될 수 있다. 개발자에 의해 의도적으로 값의 타입을 변환하는 것을 명시적 타입 변환(Explicit coercion) 또는 타입 캐스팅(Type casting)이라 한다.

    ```jsx
    var x = 10;

    // 명시적 타입 변환
    var str = x.toString(); // 숫자를 문자열로 타입 캐스팅한다.
    console.log(typeof str); // string

    // 변수 x의 값이 변경된 것은 아니다.
    console.log(typeof x, x); // number 10
    ```

- 동적 타입 언어인 자바스크립트는 개발자의 의도와는 상관없이 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되기도 한다. 이를 암묵적 타입 변환(Implicit coercion) 또는 타입 강제 변환(Type coercion)이라고 한다.

    ```jsx
    var x = 10;

    // 암묵적 타입 변환
    // 숫자 타입 x의 값을 바탕으로 새로운 문자열 타입의 값을 생성해 표현식을 평가한다.
    var str = x + '';

    console.log(typeof str, str); // string 10

    // 변수 x의 값이 변경된 것은 아니다.
    console.log(typeof x, x); // number 10
    ```

- 단축 평가를 사용하면 if문을 대체할 수 있다. 어떤 조건이 Truthy 값(참으로 평가되는 값)일 때 무언가를 해야 한다면 논리곱(&&) 연산자 표현식으로 if문을 대체할 수 있다.

    ```jsx
    // 배송료 조건
    if(total&&total<40000){
        setFinalTotal(total+3000)
    } else {
        setFinalTotal(total)
    }
    ```

* * *

### 📅 2021년 6월 18일 금요일
# 📚 10장 객체 리터럴
- 자바스크립트는 객체(object) 기반의 프로그래밍 언어이며 자바스크립트를 이루고 있는 거의 “모든 것”이 객체이다. 원시타입(Primitives)을 제외한 나머지 값들(함수, 배열, 정규표현식 등)은 모두 객체이다. 단 하나의 값만 나타내는 원시 타입은 변경 불가능 한 값(immutable value)이지만 다양한 타입의 값을 하나의 단위로 구성한 복합적인 자료구조인 객체 타입의 값은 변경 가능한 값(mutable value)이다.
- 자바스크립트는 프로토타입 기반 객체지향 언어로서 클래스 기반 객체지향 언어와는 달리 다양한 개체 생성방법을 지원한다. (객체 리터럴, Object 생성자 함수, 생성자 함수, Object.create 메서드, 클래스(ES6))
- 객체 리터럴의 중괄호는 코드 블록을 의미하지 않는다는 데 주의하자. 코드 블록의 닫는 중괄호 뒤에는 세미콜론을 붙이지 않는다. 하지만 객체 리터럴은 값으로 평가되는 표현이다. 따라서 객체 리터럴의 닫는 중괄호 뒤에는 세미콜론을 붙인다.
- 객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성된다. 프로퍼티 키는 프로퍼티 값에 접근할 수 있는 이름으로서 식별자 역할을 한다. 식별자 네이밍 규칙을 준수하는 이름인 경우 따옴표를 생략할 수 있지만 식별자 네이밍 규칙을 따르지 않는 이름에는 반드시 따옴표를 사용해야 한다.

    ```jsx
    var person = {
      // 프로퍼티 키는 name, 프로퍼티 값은 'Ian'
      firstName: 'Ian',
      // 식별자 네이밍 규칙을 준수하지 않은 경우 따옴표 생략시 에러 발생
      'last-name': 'Lee',
      // 프로퍼티 키는 age, 프로퍼티 값은 20
      age: 20
    };
    ```

- 프로퍼티 키에 문자열이나 심벌 값 외의 값을 사용하면 암묵적 타입 변환을 통해 문자열이 된다.
- 이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어쓰는데 이때 에러가 발생하지 않으니 주의해야 한다.
- 프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메서드(method)라 부른다. 즉, 메서드는 객체에 묶여 있는 함수를 의미한다.
- 프로퍼티에 접근한 방법은 마침표(dot) 프로퍼티와 대괄호(bracket) 프로퍼티가 있다.
- 대괄호 프로퍼티 접근 연산자 내부에 지정하는 프로퍼티 키는 반드시 따옴표로 감싼 문자열이어야 한다.
- 객체에 존재하지 않는 프로퍼티에 접근하면 RefrenceError가 발생하지 않고 undefined를 반환한다.

    ```jsx
    var person = {
      name: 'Lee',
    };

    console.log(person.name); // Lee
    console.log(person.age); // undefined
    console.log(person[name]); // ReferenceError: name is not defined
    console.log(person['name']); // Lee
    console.log(person[age]); // ReferenceError: name is not defined
    console.log(person['age']); // undefined
    ```

- 프로퍼티 키가 식별자 네이밍 규칙을 준수하지 않는 이름, 즉 자바스크립트에서 사용 가능한 유효한 이름이 아니면 반드시 대괄호 표기법을 사용해야 한다.
- 프로퍼티 키가 숫자로 이뤄진 문자열인 경우 따옴표를 생략할 수 있다.

    ```jsx
    var person = {
      'last-name': 'Lee',
      1: 10
    };

    console.log(person); // {1: 10, last-name: "Lee"}

    console.log(person.last-name);  // 브라우저 환경 NaN: undefined-undefined
                                  // Node.js 환경 ReferenceError: last is not defined
    console.log(person.'last-name');  // SyntaxError: Unexpected string
    console.log(person[last-name]);   // ReferenceError: last is not defined
    console.log(person['last-name']); // 'Lee'

    console.log(person['1']); // 10
    console.log(person[1]);   // 10 : person[1] -> person['1']
    console.log(person.1);    // SyntaxError
    ```

- 존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고 프로퍼티 값이 할당되며 delete 연산자를 이용하여 프로퍼티를 삭제할 수 있다.

    ```jsx
    var person = {
      name : 'Lee',
    };

    // person 객체에는 age 프로퍼티가 존재하지 않는다.
    // person 객체에 age 프로퍼티가 동적으로 생성되고 값이 할당된다.
    person.age = 20;

    console.log(person); // {name: "Lee", age: 20}

    // delete 연산자로 프로퍼티를 삭제할 수 있다.
    delete person.name;
    // 존재하지 않는 address 프로퍼티 삭제 불가능하고 에러도 발생하지 않는다.
    delete person.address;

    console.log(person); // {age: 20}
    ```
# 📚 11장 원시 값과 객체의 비교
- 원시 타입의 값은 변경 불가능한 값(immutable value)으로 값을 변수에 할당하면 실제 값이 저장되고 다른 변수에 할당하면 원본의 원시 값이 복사되어 전달, 값에 의한 전달(pass by value)된다. 이에 비해 객체(참조) 타입의 값은 변경 가능한 값(mutable value)으로 참조 값이 저장되고 다른 변수에 할당하면 원본의 참조 값이 복사, 참조에 의한 전달(pass by reference)된다.
- 변수에 원시 값을 갖는 변수를 할당하면 할당받는 변수(copy)에는 할당되는 변수(score)의 원시 값이 복사되어 전달되는데 이를 값에 의한 전달이라 한다. score 변수와 copy 변수의 값 80은 다른 메모리 공간에 저장된 별개의 값이므로 score 변수의 값을 변경해도 copy 변수의 값에는 어떠한 영향도 주지 않는다.

    ```jsx
    var score = 80;

    // copy 변수에는 score 변수의 값 80이 복사되어 할당된다.
    var copy = score;

    console.log(score, copy);    // 80 80
    console.log(score === copy); // true

    // score 변수와 copy 변수의 값은 다른 메모리 공간에 저장된 별개의 값이다.
    // 따라서 score 변수의 값을 변경해도 copy 변수의 값에는 어떠한 영향도 주지 않는다.
    score = 100;

    console.log(score, copy);    // 100 80
    console.log(score === copy); // false
    ```

- 변수에는 값이 전달되는 것이 아니라 메모리 주소가 전달된다. 변수와 같은 식별자는 값이 아니라 메모리 주소를 기억하고 있기 때문이다. "값에 의한 전달"도 사실은 값을 전달하는 것이 아니라 메모리 주소를 전달한다. 전달된 메모리는 주소를 통해 메모리 공간에 접근하면 값을 참조할 수 있다.
- 자바스크립트는 클래스 없이 객체를 생성할 수 있으며 객체가 생성된 이후라도 동적으로 프로퍼티와 메서드를 추가할 수 있다. 이는 사용하기 매우 편리하지만 성능 면에서는 이론적으로 클래스 기반 객체지향 프로그래밍 언어(자바, C++)의 객체보다 생성과 프로퍼티 접근에 비용이 더 많이 드는 비효율적인 방식이다. 따라서 V8 자바스크립트 엔진에서는 프로퍼티에 접근하기 위해 동적 탐색대신 히든 클래스라는 방식을 사용해 C++ 객체의 프로퍼티에 접근하는 정도의 성능을 보장한다.
- 객체(참조) 타입의 값, 즉 객체는 변경 가능한 값이다. 원시 값을 할당한 변수는 원시 값 자체를 값으로 갖지만 객체를 할당한 변수가 기억하는 메모리 주소를 통해 메모리 공간에 접근하면 참조 값에 접근할 수 있고 참조 값은 생성된 객체가 저장된 메모리 공간의 주소이다.
    ```jsx
    var person = {
      name: 'Lee'
    };
    
    // 참조 값을 복사(얕은 복사). copy와 person은 동일한 참조 값을 갖는다.
    var copy = person;
    
    // copy와 person은 동일한 객체를 참조한다.
    console.log(copy === person); // true
    
    // copy를 통해 객체를 변경한다.
    copy.name = 'Kim';
    
    // person을 통해 객체를 변경한다.
    person.address = 'Seoul';
    
    // copy와 person은 동일한 객체를 가리킨다.
    // 따라서 어느 한쪽에서 객체를 변경하면 서로 영향을 주고받는다.
    
    console.log(person); // {name : "Kim", address: "Seoul"}
    console.log(copy); // {name : "Kim", address: "Seoul"}
    ```

> 💡 값에 의한 전달 vs 참조에 의한 전달

    1. 값에 의한 전달: 원시 값을 갖는 변수를 할당하면 할당받는 변수에는 할당되는 변수의 원시 값이 복사되어 전달되는 것
    2. 참조에 의한 전달: 객체를 가리키는 변수를 다른 변수에 할당하면 원본의 참조 값이 복사되어 전달되는 것

![https://blog.kakaocdn.net/dn/NTTNA/btq7xS9dznw/HnDLWKqG5XX5lDk7DCE2nK/img.png](https://blog.kakaocdn.net/dn/NTTNA/btq7xS9dznw/HnDLWKqG5XX5lDk7DCE2nK/img.png)
![https://blog.kakaocdn.net/dn/lNaot/btq7wxRWCgH/kMdPzgAdKb0tkFzE2bUwM1/img.png](https://blog.kakaocdn.net/dn/lNaot/btq7wxRWCgH/kMdPzgAdKb0tkFzE2bUwM1/img.png)

- "값에 의한 전달"과 "참조에 의한 전달"은 식별자가 기억하는 메모리 공간에 저장되어 있는 값을 복사해서 전달한다는 면에서 동일하다. 다만 식별자가 기억하는 메모리 공간, 즉 변수에 저장되어 있는 값이 원시 값이냐 참조 값이냐의 차이만 있을 뿐이다. 따라서 자바스크립트에는 "참조에 의한 전달"은 존재하지 않고 "값에 의한 전달"만이 존재한다고 말할 수 있다. 이런 이유로 "공유에 의한 전달"이라고 표현하는 경우도 있다.
