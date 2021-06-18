8장부터 11장까지

# 💭 여전히, 자바스크립트의 정신

에러 발생을 최대한 줄이는 것.

## 에러를 뱉을 것 같은데 그렇지 않은 경우와 동작

| 케이스                                         | 동작                    |
| :--------------------------------------------- | :---------------------- |
| if문의 조건식의 값이 boolean 타입이 아닐 때    | implicit type change    |
| 객체 프로퍼티의 키를 중복 선언했을 때          | 마지막에 선언한 값 저장 |
| 객체에 존재하지 않는 키로 프로퍼티에 접근할 때 | `undefined` 반환        |
| 객체에 존재하지 않는 키로 프로퍼티를 삭제할 때 | 아무 일도 일어나지 않음 |
| `string`의 1 글자만 수정할 때                  | 아무 일도 일어나지 않음 |

---

## 8장 제어문

### Label statement

식별자가 붙은 문. switch문 안에 쓰이는 `case ~~` 역시 label statement이다. 이외의 사용을 권장하지 않는다.

break 키워드로 실행을 중단할 수 있다.

```javascript
foo: {
  console.log(1);
  break foo;
  console.log(2); // 실행되지 않음
}
```

## 9장 타입 변환과 단축 평가

### Type Casting vs. Type Coercion

Type Casting은 explicit 타입 변환이다. 개발자의 의도에 따라 타입이 변환되는 경우이다.
Type Coercion은 implicit 타입 변환이다. 개발자의 의도와 무관하게 오로지 자바스크립트 엔진의 의도에 의해서만 실행된다. 자바스크립트는 최대한 에러를 발생시키지 않는다는 정신을 갖고 있기 때문에, 꼭 필요한 연산을 위해서는 값의 타입까지 바꿔버린다. 이 때 암묵적으로(혹은 강제로) 변환된 새로운 값은 일회성 즉, 단 1번만 계산에 사용된 뒤 버려진다.

### Truthy / Falsy value

boolean 타입을 위한 암묵적 타입 변환이 일어났을 때 True로 계산되는 값과 False로 계산되는 값이 있다. 이를 각각 truthy, falsy라고 한다. [한국어로 번역하면 "참 같은 값", "거짓 같은 값"](https://developer.mozilla.org/ko/docs/Glossary/Truthy)이라고 한다.

- truthy: falsy 값 이외의 모든 값
- falsy: `false`, `undefined`, `null`, `0`, `-0`, `NaN`, `'' (빈 스트링)`

## 10장 객체 리터럴

### 객체

property로 구성된 집합이다. 공집합도 가능하다. 생성 방법이 여러가지다.

- 객체 리터럴 사용 (가장 흔하게 쓴다.), Object 생성자 사용, 생성자 사용, Object.create 함수 사용, 클래스 사용

### 객체 리터럴

일반적인 코드 블록과 다르다. 객체 리터럴은 결과값을 갖기 때문에 표현식의 일종이다.

```javascript
const person = {
  name: "Broccoli",
  age: 10,
};
```

### Property

property는 key, value를 갖는다.

- 프로퍼티의 값으로 사용된 함수를 특별히 *method*라고 이른다.
- `delete` 키워드를 사용해서 원하는 속성을 삭제할 수 있다.
  - e.g) `delete person.name;`
- 변수 이름을 키, 변수 값을 값으로 사용하고 싶을 때는 축약 표현을 사용할 수 있다.
  - e.g) `const obj = { x, y };`

## 11장 Primitive Value vs. Object

|        비교        | Primitive value |    Object     |
| :----------------: | :-------------: | :-----------: |
|     Mutability     |    수정 불가    |   수정 가능   |
| 변수에 저장되는 값 |       값        |    주소값     |
|    복사되는 값     |    원본의 값    | 원본의 주소값 |

자바스크립트에서 Object를 메모리에 저장하는 방식은 C/C++에서 포인터 방식과 매우 유사하지만 완전히 같지는 않다고 한다. 따라서 JS에서 call by value, call by referrence라는 표현을 쓰는 것은 적절하지 않다고 말한다.
