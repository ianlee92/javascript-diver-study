# 8장 제어문

1. 종류 
: 블록 , 조건, 반복, break, continue
- 팁 : swift 에서는 if 블록 내의 문이 하나 뿐이라도 중괄호를 생략할 수 없음
- label 문 (C의 goto 문과 유사함. 사용권장하지 않음)

# 9장 타입 변환과 단축 평가
- 명시적 타입 변환 (타입 캐스팅) / 암묵적 타입 변환 (타입 강제 변환)
- 암묵적 타입 변환 시 주의할 예제 

```jsx
+'string' : NaN
+true : 1
+false : 0
+null : 0
+undefined : NaN 
+Symbol() : TypeError
+{} : NaN
+[] : 0
+[10, 20] : NaN 
+(function() {}) : NaN 
''? : false
0? : false
null ? : false
'str'? : true
!0 : true
!NaN : true
!'' : true
```

- 문자열 타입 변환
    1. String(값)
    2. Object.prototype.toString() 
    3. 값 + '문자열'
- 숫자 타입 변환
    1. Number(값)
    2. parseInt('문자열'), parseFloat('문자열')
    3. +'문자열이나Boolean'
    4. 1 * '문자열이나 Boolean' 
- Boolean 타입 변환
    1. Boolean(값)
    2. !!값 

- 단축평가
    - 표현식을 평가하는 도중 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것
    - if 문 대체하여 사용 가능

    ```jsx
    // done 값이 true 이면 '완료' 반환, done 값이 false 이면 false 반환 
    var done = true
    var message = done && '완료'
    ```

    - 논리 연산자는 좌항에서 우항으로 평가가 진행됨.

        두 개의 피연산자가 모두 true 인 경우

        - &&
            - 첫번째가 참인 경우 두번째를 평가 한 후 결과를 반환하여 두번째 값이 반환
            - 첫번째가 거짓인 경우 두번째를 평가할 필요가 없기 때문에 첫번째 값 반환
        - ||
            - 첫번째가 참인 경우 두번째를 평가할 필요가 없기 때문에 첫번째 값이 반환
            - 첫번째 값이 거짓인 경우 두번째 값을 평가하여 두 번째 값을 반환
        - 객체를 가리키기 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때

        >> ?? 사용하면 됨. 

        - 함수 매개변수에 기본값을 설정할 때

        ```jsx
        var elem = null
        console.log(elem.value) // error
        console.log(elem?.value) // undefined

        var done = 1
        var retValue = done ?? "값없음"
        console.log(retValue)  // "값없음"
        ```

# 10장 객체 리터럴 
- 객체 생성 var obj = {}
- 키 : 변수의 이름 규칙 (따옴표 없음 obj.key), 변수 이름 규칙이 아님 (따옴표와 함께 대괄호를 이용 obj["123"])
- 키 중복선언시 덮어씌어짐 m
- 존재 하지 않는 프로퍼티 호출 시 undefined 반환 

# 11장 원시값과 객체의 비교 

|원시값 | 객체 |
|:---- |:------|
|변수에 할당하면 실제 값 저장 | 변수에 할당하면 참조 값 저장 | 
|변수 재할당 시 다른 메모리 공간에 저장| 재할당 없이 객체를 직접 변경 가능|
|원시 값 자체 변경은 불가 var str = 'string'; str[0] = 'S'; (불가) | |
|const 할당된 값 변경 불가 | const 재할당은 불가하나 객체의 프로퍼티 변경 가능|

------------
|얕은 복사 | 깊은 복사 | 
|:------ | :------- | 
|참조값 복사 | 완전한 복사본 생성|
|객체를 재할당 | 객체 내부를 모두 직접 복사| 