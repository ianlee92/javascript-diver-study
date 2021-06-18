> 📖 9장 - 타입 변환

```jsx
// 문자열 타입으로 변환
-0 + ''             //
1 + ''              //
-1 + ''             //
null + ''           //
undefined + ''      // 
[] + ''             // 
[10, 20] + ''       // 

// 숫자 타입으로 변환
1 - '1'         // 
1 / 'one'       // 
'1' > 0         // 
+''             // 
+'string'       // 
+true           // 
+false          // 
+null           // 
+undefined      // 
+[]             // 
+[10, 20]       // 

// 불리언 타입으로 변환
!!''        // 
!!true      // 
!!'true'    // 
!!false     // 
!!'false'   // 
!!1         // 
!!0         // 
!!NaN       // 
!!null      // 
!!undefined // 
!!{}        // 
!![]        // 
```

<img src="../answer.jpeg">

⇒

```jsx
// 문자열 타입으로 변환
-0 + ''             // "0"
1 + ''              // "1"
-1 + ''             // "-1"
null + ''           // "null"
undefined + ''      // "undefined"
[] + ''             // ""
[10, 20] + ''       // "10,20"

// 숫자 타입으로 변환
1 - '1'         // 0
1 / 'one'       // NaN
'1' > 0         // true
+''             // 0
+'string'       // NaN
+true           // 1
+false          // 0
+null           // 0
+undefined      // NaN
+[]             // 0
+[10, 20]       // NaN

// 불리언 타입으로 변환
!!''        // false
!!true      // true
!!'true'    // true
!!false     // false
!!'false'   // true
!!1         // true
!!0         // false
!!NaN       // false
!!null      // false
!!undefined // false
!!{}        // true
!![]        // true
```

* * *

> 📖 10장 - 프로퍼티 접근방법

```jsx
var person = {
  lastName: 'Lee',
};

console.log(person.lastName); // (1)
console.log(person.age); // (2)
console.log(person[lastName]); // (3)
console.log(person['lastName']); // (4)
console.log(person[age]); // (5)
console.log(person['age']); // (6)

// 보기 1) Lee 2) undefined 3) ReferenceError: lastName is not defined
```

<img src="../answer.jpeg">

⇒ (1) 1 (2) 2 (3) 3 (4) 1 (5) 3 (6) 2

- 프로퍼티에 접근한 방법은 마침표(dot) 프로퍼티와 대괄호(bracket) 프로퍼티가 있다. 대괄호 프로퍼티 접근 연산자 내부에 지정하는 프로퍼티 키는 반드시 따옴표로 감싼 문자열이어야 한다.
- 객체에 존재하지 않는 프로퍼티에 접근하면 RefrenceError가 발생하지 않고 undefined를 반환한다.
