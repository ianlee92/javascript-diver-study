### 📅 2021년 8월 17일 화요일
# 📚 39장 DOM (p724~)
- DocumentFragment 노드는 자식 노드들의 부모 노드로서 별도의 서브 DOM을 구성하여 기존 DOM에 추가하기 위한 용도로 사용한다. 먼저 DocumentFragment 노드를 생성하고 DOM에 추가할 요소 노드를 생성하여 노드에 자식 노드를 추가한 다음 노드를 기존 DOM에 추가한다. 이때 실제로 DOM 변경이 발생하는 것은 한 번뿐이며 리플로우와 리페인트도 한 번만 실행된다. 따라서 여러 개 의 요소 노드를 DOM에 추가하는 경우 DocumentFragment 노드를 사용하는 것이 더 효율적이다.

    ```jsx
    <!DOCTYPE html>
    <html>
      <body>
        <ul id="fruits">
          <li>Apple</li>
        </ul>
      </body>
      <script>
        const $fruits = document.getElementById('fruits');
      
        // DocumentFragment 노드 생성
        const $fragment = document.createDocumentFragment();
      
        ['Apple', 'Banana', 'Orange'].forEach(text => {
          // 1. 요소 노드 생성
          const $li = document.createElement('li');
      
          // 2. 텍스트 노드 생성
          const $textNode = document.createTextNode('Banana');
      
          // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
          $li.appendChild(textNode);
      
          // 4. $li 요소 노드를 DocumentFragment 노드의 마지막 자식 노드로 추가
          $fragment.appendChild($li);
        });
      
        // 5. DocumentFragment 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
        $fruits.appendChild($fragment);
      </script>
    </html>
    ```

1. 노드 삽입(노드 이동)
    - Node.prototype.appendChild: 인수로 전달받은 노드를 자신을 호출한 노드의 마지막 자식 노드로 DOM에 추가
    - Node.prototype.insertBefore(newNode, childNode): 두 번째 인수로 전달받은 노드 앞에 첫 번째 인수로 전달받은 노드를 삽입
2. 노드 복사
    - Node.prototype.cloneNode([deep: true | false]): 노드의 사본을 생성하여 반환하고 기본 false를 인수로 전달하면 얕은 복사하여 노드 자신만의 사본을 생성하고 true를 인수로 전달하면 깊은 복사하여 모든 자손 노드가 포함된 사본을 생성
3.  노드 교체
    - Node.prototype.replaceChild(newChild, oldChild): 자신을 호출한 노드의 자식 노드(oldChild)를 다른 노드(newChild)로 교체
4.  노드 삭제
    - Node.prototype.removeChild(child): 매개변수에 인수로 전달한 노드를 DOM에서 삭제한다. removeChild 메서드를 호출한 노드의 자식 노드이어야 함

> 💡 어트리뷰트

- HTML 문서의 구성 요소인 HTML 요소는 여러 개의 어트리뷰트를 가질 수 있다. HTML 요소의 동작을 제어하기 위한 추가적인 정보를 제공하는 HTML 어트리뷰트는 HTML 요소의 시작 태그에 어트리뷰트 이름="어트리뷰트 값" 형식으로 정의한다.

    ```jsx
    <input id="user" type="text" value="ian">
    ```

- HTML 어트리뷰트 값을 참조하려면 Element.prototype.getAttribute(attributeName) 메서드를 사용하고, HTML 어트리뷰트 값을 변경하려면 Element.prototype.setAttribute(attributeName, attributeValue)메서드를 사용한다. setAttribute 메서드는 노드에서 관리하는 HTML 요소에 지정한 어트리뷰트 값, 즉 초기 상태값을 변경한다.

    ```jsx
    <!DOCTYPE html>
    <html>
    <body>
      <input id="user" type="text" value="ian">
      <script>
        const $input = document.getElementById('user');
        
        // value 어트리뷰트 값을 취득
        const inputValue = $input.getAttribute('value');
        console.log(inputValue); // ian
        
        // value 어트리뷰트 값을 변경
        $input.setAttribute('value', 'foo');
        console.log($input.getAttribute('value')); // foo
        
        // value 어트리뷰트의 존재 확인
        if ($input.hasAttribute('value')) {
          // value 어트리뷰트 삭제
          $input.removeAttribute('value');
        }
        
        // value 어트리뷰트가 삭제되었다.
        console.log($input.hasAttribute('value')); // false
      </script>
    </body>
    </html>
    ```

- HTML 어트리뷰트의 역할은 HTML 요소의 초기 상태를 지정하는 것이고 이는 변하지 않는다. 요소 노드는 2개의 상태, 초기 상태와 최신 상태를 관리해야 하는데 초기 상태는 어트리뷰트 노드가 관리하며, 최신 상태는 DOM 프로퍼티가 관리한다.

    ```jsx
    <!DOCTYPE html>
    <html>
    <body>
      <input id="user" type="text" value="ian">
      <script>
        const $input = document.getElementById('user');
        
        // DOM 프로퍼티에 값을 할당하여 HTML 요소의 최신 상태를 변경한다.
        $input.value = 'foo';
        console.log($input.value); // foo
        
        // getAttribute 메서드로 취득한 HTMl 어트리뷰트 값
        // 즉 초기 상태 값은 변하지 않고 유지된다.
        console.log($input.getATtribute('value')); // ian
      </script>
    </body>
    </html>
    ```

- id 어트리뷰트에 대응하는 id 프로퍼티는 사용자의 입력과 아무런 관계가 없으므로 id 어트리뷰트 값이 변하면 id 프로퍼티 값도 변하고 그 반대도 마찬가지다.
- data 어트리뷰트와 dataset 프로퍼티를 사용하면 HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트 간에 데이터를 교환할 수 있다. data 어트리뷰트는 data-user-id, data-role과 같이 data- 접두사 다음에 임의의 이름을 붙여 사용한다.

### 📅 2021년 8월 19일 목요일
# 📚 40장 이벤트
- 함수를 언제 호출할 지 알 수 없으므로 개발자가 명시적으로 함수를 호출하는 것이 아니라 브라우저에게 함수 호출을 위임하는 것이다.
- 이벤트 핸들러(event handler, event listener)는 이벤트가 발생했을 때 브라우저에 호출을 위임한 함수다. 다시 말해, 이벤트가 발생하면 브라우저에 의해 호출될 함수가 이벤트 핸들러다.

> 💡 이벤트 핸들러를 등록하는 3가지 방법

1. 어트리뷰트 방식
    - 이벤트 핸들러 어트리뷰트의 이름은 onclick과 같이 on 접두사와 이벤트의 종류를 나타내는 이벤트 타입으로 이루어져 있다. DOM 노드의 이벤트 핸들러 프로퍼티에 함수 참조를 할당하는 프로퍼티 방식과는 달리 함수 호출문 등의 문을 할당한다.

    ```jsx
    <!DOCTYPE html>
    <html>
    <body>
      <button onclick="sayHi('Lee')">Click me!</button>
      <script>
        function sayHi(name) {
          console.log(`Hi! ${name}.`);
        }
      </script>
    </body>
    </html>
    ```

2. 프로퍼티 방식
    - 이벤트 핸들러를 등록하기 위해 이벤트를 발생시킬 객체인 이벤트 타깃($button)과 이벤트의 종류를 나타내는 문자열인 이벤트 타입(onclick) 그리고 이벤트 핸들러(function)를 지정해야 한다. 어트리뷰트 방식의 HTML과 자바스크립트가 뒤섞이는 문제를 해결할 수 있지만 이벤트 핸들러 프로퍼티에 하나의 이벤트 핸들러만 바인딩할 수 있다는 단점이 있다.

    ```jsx
    <!DOCTYPE html>
    <html>
    <body>
      <button>Click me!</button>
      <script>
        const $button = document.querySelector('button');
        
        // 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩
        $button.onclick = function () {
          console.log('button click');
        };
      </script>
    </body>
    </html>
    ```

3. addEventListner 메서드 방식
    - 프로퍼티 방식과는 달리 이벤트 타입에 on 접두사를 붙이지 않는다. 마지막 매개변수에는 이벤트를 캐치할 이벤트 전파 단계를 지정하는데 생략하거나 false를 지정하면 버블링 단계에서 이벤트를 캐치하고, true를 지정하면 캡처링 단계에서 이벤트를 캐치한다.

    ```jsx
    <!DOCTYPE html>
    <html>
    <body>
      <button>Click me!</button>
      <script>
        const $button = document.querySelector('button');
        
        // addEventListener 메서드 방식
        $button.addEventListener('click', function() {
          console.log('button click');
        });
      </script>
    </body>
    </html>
    ```

    - addEventListener 메서드로 등록한 이벤트 핸들러를 제거하려면 EventTarget.prototype.removeEventListener 메서드를 사용한다. 이벤트 핸들러 프로퍼티는 null을 할당하여 이벤트 핸들러를 제거할 수 있다.

> 💡 이벤트 전파

- 생성된 이벤트 객체는 이벤트를 발생시킨 DOM 요소인 이벤트 타깃을 중심으로 DOM 트리를 통해 전파된다. 이벤트 전파는 이벤트 객체가 전파되는 방향에 따라 3단계로 구분할 수 있다.

    ![https://blog.kakaocdn.net/dn/VOwDy/btrcFV0AoGL/nSbBAIoQnNjFFznUXprTx0/img.png](https://blog.kakaocdn.net/dn/VOwDy/btrcFV0AoGL/nSbBAIoQnNjFFznUXprTx0/img.png)

- 타깃 단계: 이벤트가 이벤트 타깃에 도달
- 타깃 단계: 이벤트가 이벤트 타깃에 도달
- 버블링 단계: 이벤트가 하위 요소에서 상위 요소 방향으로 전파

    ![https://blog.kakaocdn.net/dn/OOpj3/btrcB3rinqR/s8q36jq1kOizfmgNHjeB2k/img.png](https://blog.kakaocdn.net/dn/OOpj3/btrcB3rinqR/s8q36jq1kOizfmgNHjeB2k/img.png)

> 💡 이벤트 위임(event delegation)

- 이벤트 위임은 여러 개의 하위 DOM 요소에 각각 이벤트 핸들러를 등록하는 대신 하나의 상위 DOM 요소에 이벤트 핸들러를 등록하는 방법을 말한다.

    ```jsx
    <body>
      <nav>
        <ul id="fruits">
          <li id="apple" class="active">Apple</li>
          <li id="banana">Banana</li>
          <li id="orange">Orange</li>
        </ul>
      </nav>
      <script>
        const $fruits = document.getElementById('fruits');
        ...
        // 모든 내비게이션 아이템(li 요소)에 이벤트 핸들러를 등록한다.
        document.getElementById('apple').onclick = activate;
        document.getElementById('banana').onclick = activate;
        document.getElementById('orange').onclick = activate;
       
        // 이벤트 위임: 상위 요소(ul#fruits)는 하위 요소의 이벤트를 캐치할 수 있다.
        $fruits.onclick = activate;
      </sciprt>
    </body>
    ```

- 이때 이벤트 객체의 currentTarget 프로퍼티는 언제나 변함없이 $fruits 요소를 가리키지만 이벤트 객체의 target 프로퍼티는 실제로 이벤트를 발생시킨 DOM 요소를 가리킨다.
- 이벤트 객체의 preventDefault 메서드는 DOM 요소의 기본 동작을 중단시키고, 이벤트 객체의 stopPropagation 메서드는 이벤트 전파를 중지시킨다.
- 생성된 커스텀 이벤트 객체는 버블링되지 않으며 preventDefault 메서드로 취소할 수도 없다. 즉, 커스텀 이벤트 객체는 bubbles와 cancelable 프로퍼티의 값이 false로 기본 설정된다.
- 기존 이벤트 타입이 아닌 임의의 이벤트 타입을 지정하여 커스텀 이벤트 객체를 생성한 경우 반드시 addEventListener 메서드 방식으로 이벤트 핸들러를 등록해야 한다. 이벤트 핸들러 어트리뷰트/프로퍼티 방식을 사용할 수 없는 이유는 'on + 이벤트 타입'으로 이루어진 이벤트 핸들러 어트리뷰트/프로퍼티가 요소 노드에 존재하지 않기 때문이다.
