# 27장 배열 

- 희소배열 (↔ 밀집배열) : 연속적이지 않음.

 - 배열의 동작을 흉내 낸 특수한 객체 

 - 값이 비어 있을 수 있음 

 - 인덱스로 요소에 접근하는 경우 느린 성능 / 검색, 삽입, 삭제할 경우 빠른 성능 

 - 인덱스 접근 시 일반 객체보다는 2배 빠름 

1. length 
    1. 배열 요소 추가 / 삭제 시 자동 갱신
    2. 큰 값 넣으면 변화 없음 
    3. 작은 값 넣으면 배열 작아짐

### 배열 생성

1. 리터럴 :  const arr = []
2. 생성자 함수 : (new 생략해도 동일)

    ```jsx
    const arr = new Array(10) // 인수가 1개이면 배열의 크기! 
    const arr = new Array() // const arr = [] 와 같음
    const arr = new Array(1,2,3) // [1,2,3] 
    ```

3.  Array.of : 인수가 1개라도 배열의 요소로 
4. Array.from : 유사배열, 이터러블을 배열로 **변환** 

### 배열 추가 / 갱신

```jsx
const arr = [0]
arr[1] = 1   // 추가 
arr[4] = 4   // 추가, 앞에 사이는 비어있음 
arr[1] = 3   // 갱신 
arr['숫자가아니면'] = 3 //  이것은 프로퍼티.. 요소 추가 아님 
```

### 배열 삭제

```jsx
const arr = [1,2,3]
delete arr[1]      // 2가 삭제되고 비어있음 -> 회소배열
arr.splice(1, 1)   // 요소 완전 삭제 
```

### 메서드

- 원본 변경
1. push : 마지막에 요소 추가, 길이 반환 (  arr[arr.length] = '값'  ⇒ 이게 더 빠름 ) 
2. pop : 마지막 요소 제거, 제거한 요소 반환 
3. unshift : 선두에 요소 추가, 길이 반환
4. shift : 첫 번 째 요소 제거, 제거한 요소 반환 
5. splice(start, deleteCount, items) :start 번째 인덱스에서 , deleteCount 만큼 지우고, items 를 삽입 , 제거한 요소 반환 
6. reverse : 배열 순서가 반대로 뒤집혀짐, 배열 반환 
7. fill(채울요소, 시작 인덱스, 끝 인덱스) : 시작 인덱스 (생략시 전부) 부터 끝 인덱스까지 채울 요소로 배열 변경 

- 새로운 배열 생성 후 반환
1. concat : 매개변수 1개 ⇒ 마지막에 요소 결합 , 배개변수 2개이상 ⇒ 매개변수 순서대로 결합, 생성된 배열 반환 

```jsx
let result = [1, 2].concat([3, 4])

result = [...[1,2], ... [3, 4]]     // 스레드 문법 사용 권장 
```

1. slice(start, end) : start 번째 인덱스에서, end 번째 인덱스 앞까지  까지 (얕은)복사하여 반환, 음수인 경우는 끝에서.. 생략하면 전부 (얕은)복사하여 반환 
2. flat : 중첩배열 평탄화, 매개변수는 평탄화할 깊이 

```jsx
const arr = [1, [2, [3, 4, 5, 6]]]

console.log(arr)           // [1, [2, [3, 4, 5, 6]]]
console.log(arr.flat())    // [1, 2, [3, 4, 5, 6]]
console.log(arr.flat(1))   // [1, 2, [3, 4, 5, 6]]
console.log(arr.flat(2))   // [1, 2, 3, 4, 5, 6]
console.log(arr)           // [1, [2, [3, 4, 5, 6]]]
```

- 정적 메서드
1. isArray : 배열 여부 판단 

- 일반 메서드
1. indexOf : 배열 인덱스 반환, NaN 포함 시 확인 불가 
2. includes : 요소 포함 여부 
3. join : 배열 요소를 문자열로 변환한 후, 인수로 전달 받은 구분자와 연결 

### 고차함수

1. sort : (문자열 기준) 정렬, 함수의 리턴값이 0보다 크면 오름차순, 0보다 작으면 내림차순

```jsx
arr.sort() // 문자열 기준 정렬
arr.sort((a, b) => a - b)   // 오름차순 정렬
arr.sort((a, b) => b - a)   // 내림차순 정렬
arr.sort(function() {
});
```

1. forEach : 조건문, 반복문, 변수 사용 억제, forEach(요소값, index, this), 희소배열은 제외 

```jsx
const number = [1,2,3]
const pows = []

for (let i = 0; i < numbers.length; i++) {
	pows.push(numbers[i] ** 2)
}
numbers.forEach(item => pows.push(item ** 2))
numbers.forEach((item, index, arr) => { arr[index] = item ** 2 })
```

1. map : forEach 반복 + 새로운 배열 반환 
2. filter : map 반복에서 반환 값이 true 요소의 새로운 배열 반환 
3. reduce : map 반복에서 반환값을 다음 순회의 첫번째 인자로 전달. 하나의 결과값 반환 
4. some : 배열 요소 확인. 하나라도 참이면 true / 모두 거짓이면 false (OR??)
5. every : 배열 요소 확인. 모두 참이면 true / 하나라도 거짓이면 false (AND ??) 
6. find : 콜백함수 반환값이 true 인 첫번째 요소 반환
7. findIndex : 콜백함수 반환값이 true 인 첫번째 요소의 인덱스 반환 
8. flatMap : map + flat 순차 실행 효과