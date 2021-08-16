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
    <input id="user" type="text" value="ungmo2">
    ```

- HTML 어트리뷰트 값을 참조하려면 Element.prototype.getAttribute(attributeName) 메서드를 사용하고, HTML 어트리뷰트 값을 변경하려면 Element.prototype.setAttribute(attributeName, attributeValue)메서드를 사용한다. setAttribute 메서드는 노드에서 관리하는 HTML 요소에 지정한 어트리뷰트 값, 즉 초기 상태값을 변경한다.

    ```jsx
    <!DOCTYPE html>
    <html>
    <body>
      <input id="user" type="text" value="ungmo2">
      <script>
        const $input = document.getElementById('user');
        
        // value 어트리뷰트 값을 취득
        const inputValue = $input.getAttribute('value');
        console.log(inputValue); // ungmo2
        
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
      <input id="user" type="text" value="ungmo2">
      <script>
        const $input = document.getElementById('user');
        
        // DOM 프로퍼티에 값을 할당하여 HTML 요소의 최신 상태를 변경한다.
        $input.value = 'foo';
        console.log($input.value); // foo
        
        // getAttribute 메서드로 취득한 HTMl 어트리뷰트 값
        // 즉 초기 상태 값은 변하지 않고 유지된다.
        console.log($input.getATtribute('value')); // ungmo2
      </script>
    </body>
    </html>
    ```

- id 어트리뷰트에 대응하는 id 프로퍼티는 사용자의 입력과 아무런 관계가 없으므로 id 어트리뷰트 값이 변하면 id 프로퍼티 값도 변하고 그 반대도 마찬가지다.
- data 어트리뷰트와 dataset 프로퍼티를 사용하면 HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트 간에 데이터를 교환할 수 있다. data 어트리뷰트는 data-user-id, data-role과 같이 data- 접두사 다음에 임의의 이름을 붙여 사용한다.

### 📅 2021년 8월 19일 목요일
# 📚 40장 이벤트
