# 19장 프로토타입 (~p280)


![Screenshot_20210706-131359](https://user-images.githubusercontent.com/8978613/124681009-98b72480-df02-11eb-8465-f98c8f83eaa1.jpg)

- 프로토타입 체인 : 객체 접근 프로퍼티를 부모 프로토타입 프로퍼티를 순차적으로 검색  (변수 스코프랑 비슷?)

```jsx
function Person(name) {	
	this.name = name
}
Person.prototype.sayHello = function () {	      // 프로토타입 프로퍼티 
	console.log(`Hi!! My name is ${this.name}`);
}
const me = new Person("Jeong")

console.log(Object.prototype)            // {...}  end of chain (체인의 종점) 
console.log(Object.getPrototypeOf(Person.prototype)) // {...}
console.log(Person.prototype)             // {  sayHello: function () {    console.log(`Hi!! My name is ${this.name}`);}}
console.log(Object.getPrototypeOf(me))    // {  sayHello: function () {    console.log(`Hi!! My name is ${this.name}`);}}
```

- 오버라이딩 : [https://jsfiddle.net/yuchi79/tg1cze4h/](https://jsfiddle.net/yuchi79/tg1cze4h/2/)

```jsx
me.sayHello()    // "Hi!! My name is Jeong"

me.sayHello = function () {	                        // 인스턴스 프로퍼티 
	console.log(`안녕하세요. 저는 ${this.name} 입니다.`)
}

me.sayHello()     // "안녕하세요. 저는 Jeong 입니다." (섀도잉 되었음)

delete me.sayHello 

me.sayHello()    // "Hi!! My name is Jeong"

delete Person.prototype.sayHello

me.sayHello()     // me.sayHello is not a function
```

- 오버로딩 : 자바스크립트에서 지원하지 않음!!!!!! 매개변수의 개수와 인수가 일치 하지 않기 때문에 당연!
- 프로퍼티 섀도잉 : 오버라이딩 되면서 프로퍼티가 가려지는 현상 

- 프로토타입 교체  >> **동적으로 직접 교체하지 않는 것이 좋다!!!!!!!!!!!**
    - 생성자 함수에 의해 교체 : prototype에 객체 리터럴 할당

           : 객체 리터럴에 생성자가 없어서 상위 프로토타입인 Object가 생성자로 나오는 듯.... ? 

           : 프로토타입 교체하면 생성자간 연결 파괴 

    ```jsx
    Person.prototype.sayHello = function () {  
    console.log(`Hello!! My name is ${this.name}`);
    } 
    const me = new Person("Jeong")
    console.log(me.constructor)         // function Person(name) {    this.name = name}

    /////////////////////////////////////////////////
    Person.prototype = {
    	sayHello() {
    		console.log(`Hello! My name is ${this.name}`)
    	}
    }
    const me = new Person("Jeong")
    console.log(me.constructor)          // function Object() { [native code] }

    ```

    ```jsx
    Person.prototype = {
    	constructor : Person, 
    	sayHello() {
    		console.log(`Hello! My name is ${this.name}`)
    	}
    }
    ```

    - 인스턴스에 의해 교체 가능

    ```jsx
    const change = { 	
    	sayHello() {  	
    	console.log(`안녕하세요. 저는 ${this.name} 입니다. `);  
    	} 
    }
    Object.setPrototypeOf(me, parent)
    console.log(me.constructor)           // function Object() { [native code] }
    me.sayHello()                         // "안녕하세요. 저는 Jeong 입니다. "

    /////////////////////////////////////////
    const change = { 	
    	constructor: Person,     // 생성자에 의한 교체와 동일하게 constructor 추가!!!!
    	sayHello() {  	
    	console.log(`안녕하세요. 저는 ${this.name} 입니다. `);  
    	} 
    }
    Object.setPrototypeOf(me, parent)
    ```

- instanceof
    - 생성자 함수의 prototype 에 바인딩된 객체가 프로토타입 체인 상에 존재하는 지 확인

    ```jsx
    console.log(me instanceof Object)     // true
    console.log(me instanceof Person)     // true
    ```

- ~~직접 상속  : Object.create  ⇒ 권장하지 않음!~~
    - new 연산자가 없이도 객체 생성
    - 프로토타입을 지정하면서 객체 생성
    - 객체 리터럴에 의해 생성된 객체도 상속 받을 수 있음.
    - 프로토타입 체인 종점 객체는 생성할 수 있어서 Object.prototype 빌트인 메서드 사용할 수 없음. ⇒ 권장 하지 않음

- 정적 프로퍼티  : 자바의 static  함수. 그냥 객체의 프로퍼티 추가하는 것과 비슷??
    - 생성자 함수로 인스턴스를 생성하지 않아도 참조 / 호출 할 수 있는 프로퍼티 / 메서드
    - [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array#정적_속성](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array#%EC%A0%95%EC%A0%81_%EC%86%8D%EC%84%B1)
- in : 프로퍼티 존재 확인
    - 객체가 상속받는 모든 프로토타입 프로퍼티를 확인

    ```jsx
    const obj= {}
    console.log('toString' in obj)              // true
    console.log(Reflect.has(obj, 'toString'))   // true
    ```

    - 객체 고유 프로퍼티만 확인 하고 싶은 경우???

    ```jsx
    console.log(obj.hasOwnProperty('toString'))   // false
    ```

    - 열거  : Object.prototype 의 프로퍼티는 열거되지 않음
        - `[[Enumerable]]` 이 true 인 것만 열거됨.
        - 열거 시 순서는 보장되지 않음 (정렬됨)
        - [https://jsfiddle.net/yuchi79/tg1cze4h/24/](https://jsfiddle.net/yuchi79/tg1cze4h/24/)

    ```jsx
    const obj= {	
    	key1 : 'value1',  
    	key2 : 'value2',   
    	key3 : 'value3'
    }
    for (const key in obj) {	
    	console.log(key + ':' + obj[key])
    }
    // "key1:value1""key2:value2""key3:value3"
    ```

    ```jsx
    Object.defineProperty(obj, 'key2', {	
    	value : 'value2',   
    	enumerable : false
    })

    for (const key in obj) {	
    	console.log(key + ':' + obj[key])
    }
    // "key1:value1""key3:value3"
    ```