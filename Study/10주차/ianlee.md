### 📅 2021년 8월 10일 화요일
# 📚 38장 브라우저의 렌더링 과정
> 💡 브라우저의 렌더링

![https://blog.kakaocdn.net/dn/cBFVi9/btrbJcog0kh/u48oBgzm32AU8k7dqsM9xK/img.png](https://blog.kakaocdn.net/dn/cBFVi9/btrbJcog0kh/u48oBgzm32AU8k7dqsM9xK/img.png)

1. HTML, CSS, 자바스크립트, 이미지, 폰트 파일 등 렌더링에 필요한 리소스를 요청하고 서버로부터 응답을 받음
2. 렌더링 엔진은 서버로부터 응답된 HTML, CSS를 파싱하여 DOM과 CSSOM을 생성하고 이들을 결합하여 렌더 트리 생성

    ![https://blog.kakaocdn.net/dn/mpd2T/btrbCssOVcd/znj5dZXHnkhaTKKRNcHo3k/img.png](https://blog.kakaocdn.net/dn/mpd2T/btrbCssOVcd/znj5dZXHnkhaTKKRNcHo3k/img.png)

3. 자바스크립트 엔진은 서버로부터 응답된 자바스크립트를 파싱하여 AST(Abstract Syntax Tree: 추상 구문 트리)를 생성하고 바이트코드로 변환하여 실행 (이때 자바스크립트는 DOM API를 통해 DOM이나 CSSOM을 변경할 수 있고 변경된 DOM과 CSSOM은 다시 렌더 트리로 결합)

    ![https://blog.kakaocdn.net/dn/ch2zbM/btrbEOvn0M0/2gzOKI1y3B1KMDLiQPP5q0/img.png](https://blog.kakaocdn.net/dn/ch2zbM/btrbEOvn0M0/2gzOKI1y3B1KMDLiQPP5q0/img.png)

4. 렌더 트리를 기반으로 HTML 요소의 레이아웃(위치와 크기)를 계산하고 브라우저 화면에 HTML 요소를 페인팅
- 렌더링에 필요한 리소스는 모두 서버에 존재하므로 필요한 리소스를 서버에 요청하고 서버가 응답한 리소스를 파싱하여 렌더링하는 것이다.
- 서버는 루트 요청에 대해 서버의 루트 폴더에 존재하는 정적 파일 [index.html을](http://index.xn--html-jy1s/) 클라이언트로 응답하고 다른 정적 파일을 서버에 요청하려면 정적 파일의 경로와 파일 이름을 URI의 호스트 뒤의 패스(path)에 기술하여 서버에 요청한다. (ex: …/assets/data/data.json 자바스크립트를 통해 동적으로 서버에 정적/동적 데이터를 요청할 수도 있다. (ajax, REST API)
- HTTP(HyperText Transfer Protocol)는 웹에서 브라우저와 서버가 통신하기 위한 프로토콜(규약)이다. HTTP/1.1은 기본적으로 커넥션당 하나의 요청과 응답만 처리한다. 리소스의 동시 전송이 불가능한 구조이므로 요청할 리소스의 개수에 비례하여 응답 시간도 증가하는 단점이 있다. HTTP/2는 커넥션당 다중 요청/응답이 가능해 여러 리소스의 동시 전송이 가능하고 페이지 로드 속도가 약 50% 정도 빠르다.
- HTML 문서를 파싱하여 브라우저가 이해할 수 있는 노드들로 구성된 트리 자료구조인 DOM(Document Object Model)을 생성한다. 즉, DOM은 HTML 문서를 파싱한 결과물이다. (바이트 -> 문자 -> 토큰 -> 노드 -> DOM)
- 렌더링 엔진이 HTML과 CSS를 파싱하여 DOM과 CSSOM을 생성하듯이 자바스크립트 엔진은 자바스크립트를 해석하여 AST(Abstract Syntax Tree:추상적 구문 트리)를 생성한다. 그리고 AST를 기반으로 인터프리터가 실행할 수 있는 중간 코드인 바이트코드를 생성하여 실행한다.

    ![https://blog.kakaocdn.net/dn/TsUmB/btrbOPGdglj/4eOhmMY4jxKzOmDQyf6KNK/img.png](https://blog.kakaocdn.net/dn/TsUmB/btrbOPGdglj/4eOhmMY4jxKzOmDQyf6KNK/img.png)

- 렌더링 엔진과 자바스크립트 엔진은 직렬적으로 파싱을 수행한다. 브라우저는 동기적(synchronous)으로, 즉 위에서 아래 방향으로 순차적으로 HTML, CSS, 자바스크립트를 파싱하고 실행한다. script 태그의 위치에 따라 HTML 파싱이 블로킹되어 DOM 생성이 지연될 수 있다는 것을 의미한다.
- 자바스크립트를 head가 아닌 body 요소의 가장 아래 닫는 태그 바로 위에 위치시키면 자바스크립트가 실행될 시점에 이미 렌더링 엔진이 HTML 요소를 모두 파싱하여 DOM 생성이 완료되어 렌더링되므로 페이지 로딩 시간이 단축되고 에러 발생할 우려도 없다.
# 📚 39장 DOM (~p684)
- DOM(Document Object Model: 문서 객체 모델)은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조다.

![https://blog.kakaocdn.net/dn/XMBNt/btrbGA5bkQX/ft3j4aXD79oMTTJLYZUl30/img.png](https://blog.kakaocdn.net/dn/XMBNt/btrbGA5bkQX/ft3j4aXD79oMTTJLYZUl30/img.png)

![https://blog.kakaocdn.net/dn/bCIkYT/btrbIcJIdJZ/IKk70KA3T6CVL3l8k5S5kk/img.png](https://blog.kakaocdn.net/dn/bCIkYT/btrbIcJIdJZ/IKk70KA3T6CVL3l8k5S5kk/img.png)

![https://blog.kakaocdn.net/dn/blpDeU/btrbCt6BbcI/W8mUzTj0cVcxP1ZSNBeXoK/img.png](https://blog.kakaocdn.net/dn/blpDeU/btrbCt6BbcI/W8mUzTj0cVcxP1ZSNBeXoK/img.png)

- 모든 노드 객체는 Object, EventTarget, Node 인터페이스를 상속받는다. 추가적으로 문서 노드는 Document, HTMLDocument 인터페이스를 상속받고 어트리뷰트 노드는 Attr, 텍스트 노드는 CharacterData 인터페이스를 각각 상속받는다. 요소 노드는 Element 인터페이스를 상속받는다. 또한 요소 노드는 추가적으로 HTMLElement와 태그의 종류별로 세분화된 HTMLHtmlElement, HTMLHeadElement, HTMLBodyElement, HTMLUListElement 등의 인터페이스를 상속받는다.
- DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류, 즉 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API(Application Programming Interface)로 제공한다. 이 DOM API를 통해 HTML의 구조나 내용 또는 스타일 등을 동적으로 조작할 수 있다.

> 💡 요소 노드 취득

- HTML의 구조나 내용 또는 스타일 등을 동적으로 조작하려면 먼저 요소 노드를 취득해야 한다. 텍스트 노드나 어트리뷰트 노드를 조작하고자 할 때도 마찬가지다. 요소 노드의 취득은 HTML 요소를 조작하는 시작점이다. DOM은 요소 노드를 취득할 수 있는 다양한 메서드를 제공한다.
1. id를 이용한 요소 노드 취득
    - Document.prototype.getElementById 메서드는 인수로 전달한 id 어트리뷰트 값(이하 id 값)을 갖는 하나의 요소 노드를 탐색하여 반환한다. getElementById 메서드는 Document.prototype의 프로퍼티다. 따라서 반드시 문서 노드인 document를 통해 호출해야 한다.

    ```html
    <!DOCTYPE html>
    <html>
      <body>
        <ul>
          <li id="apple">Apple</li>
          <li id="banana">Banana</li>
          <li id="banana">Orange</li>
        </ul>
        <script>
          // id값이 banana인 두 번째 li 요소가 파싱되어 생성된 요소 노드 반환
          // 인수로 전달된 id 값을 갖는 첫 번째 요소 노드만 반환(단 하나의 요소 노드 반환)
          const $elem = document.getElementById('banana');
          // 취득한 요소 노드의 style.color 프로퍼티 값 변경
          $elem.style.color = 'red';
          
          // id 값 없으면 null 반환
          const $elem2 = document.getElementById('grape');
        </script>
      </body>
    </html>
    ```

2. 태그 이름을 이용한 요소 노드 취득
    - Document.prototype/Element.prototype.getElementsByTagName 메서드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색하여 반환한다. 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환한다. HTML 문서의 모든 요소 노드를 취득하려면 메서드의 인수로 '*'를 전달한다.
    - Document.prototype에 정의된 메서드는 document를 통해 호출하며 DOM 전체에서 요소 노드를 탐색하여 반환하고 Element.prototype에 정의된 메서드는 특정 요소 노드를 통해 호출하며, 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환한다.

    ```html
    <!DOCTYPE html>
    <html>
      <body>
        <ul id="fruits">
          <li>Apple</li>
          <li>Banana</li>
          <li>Orange</li>
        </ul>
        <ul>
          <li>HTML</li>
        </ul>
        <script>
          // Dom 전체에서 태그 이름이 li인 요소 노드를 모두 탐색하여 반환
          const $lisFromDocument = document.getElementsByTagName('li');
          console.log($lisFromDocument); // HTMLCollection(4) [li, li, li, li]
          
          // ul#fruits 요소의 자손 노드 중에서 태그 이름이 li인 요소 노드를 모두 탐색하여 반환
          const $fruits = document.getElementById('fruits');
          const $lisFromFruits = $fruits.getElementsByTagName('li');
          console.log($lisFromFruits); // HTMLCollection(3) [li, li, li]
        </script>
      </body>
    </html>
    ```

3. class를 이용한 요소 노드 취득
    - Document.prototype/Element.prototype.getElementsByClassName 메서드는 인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색하여 반환한다. getElementsByTagName 메서드와 마찬가지로 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환한다.

    ```html
    <!DOCTYPE html>
    <html>
      <body>
        <ul id="fruits">
          <li class="apple">Apple</li>
          <li class="banana">Banana</li>
          <li class="orange">Orange</li>
        </ul>
        <div class="banana">Banana</div>
        <script>
          // Dom 전체에서 class 값이 'banana'인 요소 노드를 모두 탐색하여 반환
          const $bananasFromDocu = document.getElementsByClassName('banana');
          console.log($bananasFromDocu);
          // HTMLCollection(2) [li.banana, div.banana]
          
          // #fruits 요소의 자손 노드 중 class 값이 'banana'인 요소 노드 모두 탐색해 반환
          const $fruits = document.getElementById('fruits');
          const $bananasFromFruit = $fruits.getElementsByClassName('banana');
          console.log($bananasFromFruit); // HTMLCollection [li.banana]
        </script>
      </body>
    </html>
    ```

4.  CSS 선택자를 이용한 요소 노드 취득
    - Document.prototype/Element.prototype.querySelector 메서드는 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색하여 반환한다. (요소 노드가 여러 개인 경우 첫 번째 요소 노드만 반환, 존재하지 않는 경우 null 반환, 문법에 맞지 않는 경우 DOMException 에러 발생)
    - Document.prototype/Element.prototype.querySelectorAll 메서드는 인수로 전달한 CSS 선택자를 만족시키는 모든 요소 노드를 탐색하여 DOM 컬렉션 객체인 NodeList 객체를 반환한다. HTML 문서의 모든 요소 노드를 취득하려면 querySelectorAll 메서드의 인수로 전체 선택자 '*'를 전달한다.

    ```html
    <!DOCTYPE html>
    <html>
      <body>
        <ul>
          <li class="apple">Apple</li>
          <li class="banana">Banana</li>
          <li class="orange">Orange</li>
        </ul>
        <script>
          // ul 요소의 자식 요소인 li 요소를 모두 탐색하여 반환
          const $elems = document.querySelectorAll('ul > li');
          console.log($elems);
          // NodeList(3) [li.apple, li.banana, li.orange]
        </script>
      </body>
    </html>
    ```

5.  특정 요소 노드를 취득할 수 있는지 확인
    - Element.prototype.matches 메서드는 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는 지 확인한다. (이벤트 위임 사용 시 유용)

    ```html
    <!DOCTYPE html>
    <html>
      <body>
        <ul id="fruits">
          <li class="apple">Apple</li>
          <li class="banana">Banana</li>
          <li class="orange">Orange</li>
        </ul>
        <script>
          const $apple = document.querySelector('.apple');
          console.log($apple.matches('#fruits > li.apple')); // true
          console.log($apple.matches('#fruits > li.banana')); // false 취득 X
        </script>
      </body>
    </html>
    ```
### 📅 2021년 8월 12일 목요일
# 📚 39장 DOM (p685~713)
- DOM 컬렉션 객체인 HTMLCollection과 NodeList는 DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬렉션 객체다. 스프레드 문법을 사용하여 간단히 배열로 변환할 수 있고 노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 사용하려면 배열로 변환하여 사용하는 것을 권장한다.

> 💡 노드 정보 취득

- Node.prototype.nodeType: 노드 타입을 나타내는 상수로 반환. 1: 요소 노드 타입, 3: 텍스트 노드 타입, 9: 문서 노드 타입
- Node.prototype.nodeName: 노드의 이름을 문자열로 반환. 요소 노드, 텍스트 노드, 문서 노드

    ```html
    <!DOCTYPE html>
    <html>
      <body>
        <div id="foo">Hello</div>
      </body>
      <script>
        // 문서 노드의 노드 정보를 취득한다.
        console.log(document.nodeType); // 9
        console.log(document.nodeName); // #document
        
        // 요소 노드의 노드 정보를 취득한다.
        const $foo = document.getElementById('foo');
        console.log($foo.nodeType); // 1
        console.log($foo.nodeName); // DIV
        
        // 텍스트 노드의 노드 정보를 취득한다.
        const $testNode = $foo.firstChild;
        console.log($testNode.nodeType); // 3
        console.log($testNode.nodeName); // #text
      </script>
    </html>
    ```


> 💡 DOM 조작

- Element.prototype.innerHTML 프로퍼티는 요소 노드의 콘텐츠 영역 내에 포함된 모든 HTML 마크업을 문자열로 반환한다.
- innerHTML 프로퍼티를 사용한 DOM 조작은 구현이 간단하고 직관적이라는 장점이 있지만 크로스 사이트 스크립팅 공격(XSS)에 취약한 단점도 있다. 그리고 기존의 자식 노드까지 모두 제거하고 다시 처음부터 새롭게 자식 노드를 생성하여 DOM에 반영하므로 효율적이지 않고 새로운 요소를 삽입할 때 삽입될 위치를 지정할 수 없다는 단점도 있다.

    ```html
    <!DOCTYPE html>
    <html>
      <body>
        <ul id="fruits">
          <li class="apple">Apple</li>
        </ul>
      </body>
      <script>
        const $fruits = document.getElementById('fruits');
        
        // 노드 추가
        $fruits.innerHTML += '<li class="banana">Banana</li>';
        
        // 노드 교체
        $fruits.innerHTML = '<li class="orange">Orange</li>';
        
        // 노드 삭제
        $fruits.innerHTML = '';
      </script>
    </html>
    ```

- Element.prototype.insertAdjacentHTML(position, DOMString) 메서드는 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입한다. 기존 요소에는 영향을 주지 않고 새롭게 삽입될 요소만을 파싱하여 자식 요소로 추가하므로 innerHTML 프로퍼티보다 효율적이고 빠르지만 마찬가지로 HTML 마크업 문자열을 파싱하므로 크로스 사이트 스크립팅 공격에 취약하다는 점은 동일하다.
- innerHTML 프로퍼티와 insertAdjacentHTML 메서드는 HTML 마크업 문자열을 파싱하여 노드를 생성하고 DOM에 반영한다. DOM은 노드를 직접 생성/삽입/삭제/치환하는 메서드도 제공한다.

    ```html
    <!DOCTYPE html>
    <html>
      <body>
        <ul id="fruits">
          <li>Apple</li>
        </ul>
      </body>
      <script>
      const $fruits = document.getElementById('fruits');
      
      // 1. 요소 노드 생성
      const $li = document.createElement('li');
      
      // 2. 텍스트 노드 생성
      const $textNode = document.createTextNode('Banana');
      
      // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
      $li.appendChild(textNode);
      
      // 4. $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
      $fruits.appendChild($li);
      </script>
    </html>
    ```

    - Document.prototype.createElement(tagName) 메서드는 요소 노드를 생성하여 반환한다. 요소 노드를 생성할 뿐 DOM에 추가되지 않고 홀로 존재하고 자식 노드인 텍스트 노드도 없는 상태다.
    - Document.prototype.createTextNode(text) 메서드는 텍스트 노드를 생성하여 반환한다. 매개변수 text에는 텍스트 노드의 값으로 사용할 문자열을 인수로 전달한다. createElement 메서드와 마찬가지로 텍스트 노드를 생성할 뿐 요소 노드에 추가하지 않는다.
    - Node.prototype.appendChild(childNode) 메서드는 매개변수 childNode에게 인수로 전달한 노드를 메서드를 호출한 노드의 마지막 자식 노드로 추가한다. appendChild 메서드를 통해 요소 노드와 텍스트 노드는 부자 관계로 연결되었지만 아직 기존 DOM에 추가되지 않은 상태다.

        ```jsx
        // 텍스트 노드를 생성하여 요소 노드의 자식 노드로 추가
        $li.appendChild(document.createTextNode('Banana'));

        // $li 요소 노드에 자식 노드가 하나도 없는 위 코드와 동일하게 동작한다.
        $li.textContent = 'Banana';
        ```

    - 요소 노드를 DOM에 추가할 때도 appendChild 메서드를 사용하여 텍스트 노드와 부자 관계로 연결한 요소 노드를 $fruits 요소 노드의 마지막 자식 요소로 추가한다. 비로소 새롭게 생성한 요소 노드가 DOM에 추가된다. 단 하나의 요소 노드를 생성하여 DOM에 한번 추가하므로 DOM은 한 번 변경된다. 이때 리플로우와 리페인트가 실행된다.
