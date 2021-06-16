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
# 📚 11장 원시 값과 객체의 비교
