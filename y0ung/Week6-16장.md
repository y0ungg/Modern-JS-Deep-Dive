# Week 6 (8/8~8/14)
###### `JavaScript`

## 16. 프로퍼티 어트리뷰트

>내부 슬롯과 내부 메서드란 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 사용하는 의사 프로퍼티와 의사 메서드다.
ECMAScript 사양에 등장하는 이중 대괄호로 감싼 이름들이 내부 슬롯과 내부 메서드다.
이에 직접 접근하거나 호출할 수 없으나, 일부에 한해 간접적으로 접근할 수 있다.

<br/>

### 📌 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체
자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.
프로퍼티 디스크립터 객체는 프로퍼티 어트리뷰트 정보를 담고 있다.
다음 메서드를 통해 프로퍼티 디스크립터 객체를 조회할 수 있다.

```jsx!
const a = {person: 1, sayHi: "hi"}

//프로퍼티 1개에 대한 프로퍼티 디스크립터 객체를 반환한다.
//bject.getOwnPropertyDescriptor(object, property key)

Object.getOwnPropertyDescriptor(a, "person")
> {value: 1, writable: true, enumerable: true, configurable: true}


//객체의 모든 프로퍼티에 대한 프로퍼티 디스크립터 객체들을 반환한다.
//Object.getOwnPropertyDescriptors(object)

Object.getOwnPropertyDescriptors(a)
{
    "person": {
        "value": 1,
        "writable": true,
        "enumerable": true,
        "configurable": true
    },
    "sayHi": {
        "value": "hi",
        "writable": true,
        "enumerable": true,
        "configurable": true
    }
}
```


<br/>
<br/>

### 📌 데이터 프로퍼티와 접근자 프로퍼티


#### 1. 데이터 프로퍼티

키와 값을 갖는 일반적인 프로퍼티를 말한다.
위에서 알아본 프로퍼티 어트리뷰트를 갖는다.

```jsx!
{
	value: 1, //프로퍼티 값
		
	//아래 세 어트리뷰트는 true로 초기화된다.
	writable: true, //프로퍼티 값의 변경 가능 여부
	enumerable: true, //프로퍼티의 열거 가능 여부
	configurable: true //프로퍼티의 재정의 가능 여부
}
```
<br/>

#### 2. 접근자 프로퍼티
자체적으로 값을 갖지 않고, 다른 데이터 프로퍼티에 대한 **접근자 함수**로 구성된 프로퍼티다.
접근자 함수는 `get`/`set` 메서드를 통해 생성하며, `getter`/`setter` 함수라고도 한다.

```jsx!
const person = {
	firstName: "Lee",
	lastName: "Ho",
	
	//getter 함수
	get fullName() {
		return `${this.firstName} ${this.lastName}`
	},
	
	//setter 함수
	set fullName(name) {
		[this.firstName, this.lastName] = name.split(' ');
	}	
}
```
<br/>

`fullName` 프로퍼티에 값을 **저장**하면 `setter` 함수가 호출된다. 
`setter` 함수의 결과값이 프로퍼티 값으로 저장된다.
```jsx
person.fullName = "Kim Jo";

console.log(person); //{firstName: 'Kim', lastName: 'Jo'}
```

<br/>

`fullName` 프로퍼티를 **참조**하면 `getter` 함수가 호출된다.

```jsx
console.log(person.fullName); //Kim Jo
```
<br/>


접근자 프로퍼티는 다음과 같은 프로퍼티 어트리뷰트를 갖는다.

```jsx
Object.getOwnPropertyDescriptor(person, 'fullName');
{enumerable: true, configurable: true, get: ƒ, set: ƒ}
```

<br/>
<br/>

### 📌 프로퍼티 정의
다음 메서드를 통해 프로퍼티 어트리뷰트를 정의할 수 있다.

```jsx!
//프로퍼티 1개에 대한 정의
Object.defineProperty(object, property key, {property descriptor})

//여러 프로퍼티에 대한 정의
Object.defineProperties(object, {
	property key1: {property descriptor},
	property key2: {property descriptor},
})
```

프로퍼티 디스크립터 객체를 인자로 전달하지 않을 경우, 프로퍼티 어트리뷰트가 `undefined`, `false`로 초기화된다.

<br/>
<br/>

### 📌 객체 변경 방지

객체는 프로퍼티 추가, 삭제, 갱신, 프로퍼티 재정의가 가능하지만, 객체 변경을 방지하는 메서드들이 있다.


- `Object.preventExtensions(object)`: 프로퍼티 추가를 금지한다.
`Object.isExtensible(object)` 메서드로 확장 가능 여부를 확인할 수 있다.


- `Object.seal(object)`: 프로퍼티 추가, 삭제, 프로퍼티 어트리뷰트 재정의를 금지한다.
`Object.isSealed(object)` 메서드로 밀봉된 객체인지 여부를 확인할 수 있다.

- `Object.freeze(object)`: 프로퍼티 값 읽기를 제외하고 모두 금지한다.
`Object.isFrozen(object)` 메서드로 객체 동결 여부를 확인할 수 있다.


중첩된 객체까지 변경을 방지하려면 모든 프로퍼티에 대해 재귀적으로 메서드를 호출해야 한다.