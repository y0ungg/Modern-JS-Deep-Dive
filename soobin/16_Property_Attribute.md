# ☑️ 16. 프로퍼티 어트리뷰트

p.219~233

✍️ 2022.12.12(Mon)

## 📎 16.1_내부 슬롯과 내부 메서드

- 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 사용하는 pseudo property & method<br/>(이중 대괄호로 감싼 이름들이 이에 해당!)

- 모든 객체는 `[[Prototype]]` 내부 슬롯을 가짐
- 내부 슬롯은 자스엔진 내부 로직이므로 직접 접근할 순 없지만, <br/> `[[Prototype]]` 내부 슬롯의 경우, `__proto__`를 통해 간접적으로 접근 가능

```js
const o = {};
o.[[Prototype]]; // 이렇게 직접 접근 불가 => SyntaxError
o.__proto__; // Object.prototype
```

<br/>

## 📎 16.2_프로퍼티 어트리뷰트 & 프로퍼티 descriptor 객체

- 자스엔진은 프로퍼티 생성시 프로퍼티 상태를 나타내는 `프로퍼티 어트리뷰트`를 기본값으로 자동 정의한다.

  ✚ `프로퍼티 상태`: 프로퍼티 value, 값의 갱신 가능 여부, 열거 가능 여부, 재정의 가능 여부

- `Property Attribute`: 자스엔진이 관리하는 내부 상태 값인 내부 슬롯 `[[Value]]`, `[[Writable]]`, `[[Enumerable]]`, `[[Configurable]]`

  - 이는 직접 접근 불가 <br/> But, `Object.getOwnPropertyDescriptor` 메서드를 사용하여 간접적으로 확인 가능

```js
const person = { name: 'Choi' };
person.age = 24; // 프로퍼티 동적 생성

// 매개변수 1번째: 객체의 참조, 2번째: 프로퍼티 키를 문자열로 전달
console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// => Property Descriptor 객체 반환 | undefined
// {value: 'Choi', writable: true, enumerable: true, configurable: true}

// ES8부턴 모든 Property Descriptor 객체 반환
console.log(Object.getOwnPropertyDescriptor(person);
// {
//   name: {value: 'Choi', writable: true, enumerable: true, configurable: true},
//   age: {value: 24, writable: true, enumerable: true, configurable: true}
// }
```

<br/>

## 📎 16.3_데이터 프로퍼티 & 접근자 프로퍼티


### 💫 16.3.1_데이터 프로퍼티

- `데이터 프로퍼티`: 키와 값으로 구성된 일반적인 프로퍼티


| 프로퍼티 attribute | 프로퍼티 descriptor 객체의 프로퍼티 | description |
|:----------------:|:-----------------------------:|:-----------|
| [[Value]] | value | - 프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값 <br/> - 값 변경시 [[Value]]에 값 재할당 |
| [[writable]] | writable | - 프로퍼티 값의 변경 가능 여부 (Boolean) <br/> - 값이 false인 경우 [[Value]] 값을 변경할 수 없는 읽기 전용 프로퍼티가 됨 |
| [[Enumerable]] | enumerable | - 프로퍼티의 열거 가능 여부 (Boolean) <br/> - 값이 false인 경우 for...in문, Object.keys 메서드 등으로 열거 불가능  |
| [[Configurable]] | configurable | - 프로퍼티의 재정의 가능 여부 (Boolean) <br/> - 값이 false인 경우 해당 프로퍼티 어트리뷰트 값 변경이 금지됨 |


### 💫 16.3.2_접근자 프로퍼티

- `접근자(accessor) Property`: 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티


| 프로퍼티 attribute | 프로퍼티 descriptor 객체의 프로퍼티 | description |
|:----------------:|:-----------------------------:|:-----------|
| [[Get]] | get | - 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수 <br/> - [[Get]]의 값인 getter 함수가 호출되고, 그 결과가 프로퍼티 값으로 반환 |
| [[Set]] | set | - 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수 <br/> - [[Set]]의 값인 setter 함수가 호출되고, 그 결과가 프로퍼티 값으로 반환 |
| [[Enumerable]] | enumerable | - 데이터 프로퍼티와 같음  |
| [[Configurable]] | configurable | - 데이터 프로퍼티와 같음 |

✋ 접근자 함수는 `getter/setter` 함수라고도 부름

```js
const person = {
  firstName: 'Soobhin',
  lastName: 'Choi',

  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`
  },

  // setter 함수
  set fullName(name) {
    [this.firstName, this.lastName] = name.split(' ');
  }
};

// ** 접근자 프로퍼티를 통한 프로퍼티 값 참조 **
// setter 함수 호출
person.fullName = 'Zzanggu Shin';
console.log(person); // {firstName: 'Zzanggu', lastName: 'Shin'}

// getter 함수 호출
console.log(person.fullName); // Zzanggu Shin

// firstName, lastName: 데이터 프로퍼티
// fullName: 접근자 프로퍼티
```

- `Prototype`: 어떤 객체의 상위(부모) 객체 역할을 하는 객체
  - 하위(자식) 객체에게 자신의 프로퍼티와 메서드를 상속함

- `Prototype Chain`: 프로토타입이 단방향 Linked List 형태로 연결되어 있는 상속 구조

```js
// 일반 객체의 __proto__: 접근자 프로퍼티
Object.getOwnPropertyDescriptor(Object.prototype, '__proto__');
// {get: f, set: f, enumerable: false, configurable: true}

// 함수 객체의 prototype: 데이터 프로퍼티
Object.getOwnPropertyDescriptor(function() {}, 'prototype');
// {value: {...}, writable: true, enumerable: false, configurable: false}
```

<br/>

## 📎 16.4_프로퍼티 정의

- `Object.defineProperty` 메서드를 사용하여 프로퍼티 어트리뷰트 정의 가능

```js
const person = {};

// arguments: (객체의 참조, 데이터 프로퍼티의 키인 문자열, 프로퍼티 Descrtipor 객체)
Object.defineProperty(person, 'lastName', {
  value: 'Choi'
});

// Descriptor 객체의 프로퍼티를 누락시키면 undefined, false가 기본값
// [[Enumerable]]: false => 열거 불가능
// [[writable]]: false => [[Value]] 값 변경 불가능 (에러 발생 X, 무시됨)
// [[Configurable]]: false => 해당 프로퍼티 삭제 불가능 (에러 발생 X, 무시됨)
// [[Configurable]]: false => 해당 프로퍼티 재정의 불가능 (TypeError)
```

- `value`, `get`, `set` => 생략시 기본값 `undefined`
- `writable`, `enumerable`, `configurable` => 생략시 기본값 `false`

<br/>

- `Object.defineProperty`: 한번에 하나의 프로퍼티만 정의 가능
- `Object.defineProperties`: 여러 개의 프로퍼티를 한 번에 정의 가능


<br/>

## 📎 16.5_객체 변경 방지

- JS는 객체 변경을 방지하는 다양한 메서드 제공 (각 객체 변경 금지 강도가 다름)

| 구분 | 메서드 | 프로퍼티 추가 | 프로퍼티 삭제 | 프로퍼티 값 읽기 | 프로퍼티 값 쓰기 | 프로퍼티 어트리뷰트 재정의|
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|객체 확장 금지|`Object.preventExtensions`|❌|⭕️|⭕️|⭕️|⭕️|
|객체 밀봉|`Object.seal`|❌|❌|⭕️|⭕️|❌|
|객체 동결|`Object.freeze`|❌|❌|⭕️|❌|❌|

### 💫 16.5.1_객체 확장 금지

- `객체 확장 금지`: 프로퍼티 추가 금지 의미 <br/> 👉 확장이 금지된 객체는 프로퍼티 추가가 금지됨
  - 프로퍼티 동적 추가, `Object.defineProperty` 메서드 추가 방법 모두 금지됨

- `Object.isExtensible`: 확장 가능한 객체 여부 확인

```js
const person = {name: 'Choi'};

// 확장 가능 객체!
console.log(Object.isExtensible(person)); // true

Object.preventExtensions(person); // 객체 확장 금지 => 프로퍼티 추가 금지
console.log(Object.isExtensible(person)); // false

// 프로퍼티 추가는 금지되지만 삭제는 가능!
delete person.name; // person: {}
```

### 💫 16.5.2_객체 밀봉

- `객체 밀봉`: 프로퍼티 추가, 삭제 & 프로퍼티 어트리뷰트 재정의 금지 의미 <br/> 👉 밀봉된 객체는 읽기 & 쓰기만 가능

```js
const person = {name: 'Choi'};

// 밀봉된 객체가 아님!
console.log(Object.isSealed(person)); // false

Object.seal(person); // 객체 밀봉 => 프로퍼티 추가, 삭제, 재정의 금지
console.log(Object.isSealed(person)); // true
// 밀봉된 객체는 configurable: false
person.name = 'Kim'; // 프로퍼티 값 갱신은 가능!

// 프로퍼티 어트리뷰트 재정의 금지
Object.defineProperty(person, 'name', {configurable: true}); // TypeError
```

### 💫 16.5.3_객체 동결

- `객체 동결`: 프로퍼티 추가, 삭제, 프로퍼티 어트리뷰 재정의 금지, 프로퍼티 값 갱신 금지 의미 <br/> 👉 동결된 객체는 읽기만 가능!

- `Object.isFrozen`: 동결된 객체 여부 확인

```js
const person = {name: 'Choi'};

// 동결된 객체가 아님!
console.log(Object.isFrozen(person)); // false

Object.freeze(person); // 객체 동결 => 프로퍼티 추가, 삭제, 재정의, 쓰기 금지
console.log(Object.isSealed(person)); // true

// 동결된 객체는 writable, configurable: false
```

### 💫 16.5.4_불변 객체

- 위의 객체 변경 방지 메서드들은 얕은 변경 방지로 직속 프로퍼티만 변경이 방지됨!
  - `Object.freeze` 메서드로 객체를 동결해도 중첩 객체까지 동결할 수는 없음


👀 객체의 중첩 객체까지 동결하여 변경 불가능한 읽기 전용 불변 객체를 구현하려면 객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로 `Object.freeze` 메서드를 호출해야 함

```js
const person = {
  name: 'Choi',
  address: { city: 'Seoul' }
};

Object.freeze(person); // 얕은 객체 동결

console.log(Object.isFrozen(person)); // true
console.log(Object.isFrozen(person.address)); // false
// 중첩 객체까지는 동결하지 못함

```