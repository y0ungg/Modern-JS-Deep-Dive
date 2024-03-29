# Week 6 (8/8 ~ 8/14)
###### `JavaScript`

## 15. let, const 키워드와 블록 레벨 스코프
<br/>

### 📌 var 키워드의 특징

#### 1. 같은 스코프 내에서 변수의 중복 선언을 허용한다.
동일한 이름이 있는 변수를 선언할 경우, 값이 변경되는 부작용이 발생한다.

```jsx
var x = 1;
var y = 2;

var x = 100; //변수의 중복 선언을 허용한다.
var y; //초기화문이 없는 변수 선언문은 무시된다.

console.log(x); //100
console.log(y); //2
```

<br/>

#### 2. 함수 레벨 스코프만을 지역 스코프로 인정한다.
함수 외부에서 var로 선언한 변수는 모두 전역 변수가 된다.

<br/>

#### 3. 변수 호이스팅이 일어나서 변수 선언문 이전에 참조할 수 있다.
변수 선언 단계와 초기화 단계가 런타임 이전에 한꺼번에 발생한다.
단, 할당문 이전에 참조할시 undefined를 반환한다.

```jsx
console.log(x) //undefined

var x = 1;

console.log(x) //1
```

<br/>
<br/>

### 📌 let 키워드의 특징

ES6에서 도입된 변수 선언 키워드이다.

#### 1. 같은 스코프 내에서 변수 중복 선언을 금지한다.
변수를 중복 선언할시 문법 에러가 발생한다.
```jsx
let x = 1;
let x = 2; //SyntaxError: Identifier 'x' has already been declared
```
<br/>

#### 2. 블록 레벨 스코프를 따른다.
모든 코드 블록을 지역 스코프로 인정한다.
```jsx
let x = 0; //전역변수

{
	//지역변수
	let x = 1;
	let y = 2;
}

console.log(x) //0
consoel.log(y) //ReferenceError: y is not defined
```

>전역에서는 지역 변수를 참조할 수 없다.

<br/>


#### 3. 변수 호이스팅이 발생하지 않는 것처럼 동작한다.
let 키워드로 선언한 변수를 변수 선언문 이전에 참조하면 참조 에러가 발생한다.
```jsx
console.log(x) //ReferenceError: x is not defined
```
변수 선언 단계와 초기화 단계가 분리되어 진행된다.
런타임 이전에 변수 선언 단계가 이루어지지만, 초기화 단계는 변수 선언문에 도달했을 때 실행된다.

이처럼 스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 **일시적 사각지대**라고 한다.

>let, const 등 모든 선언 키워드로 선언한 변수는 여전히 호이스팅이 일어나지만, 호이스팅이 일어나지 않는 것처럼 동작할 뿐이다.

<br/>

#### 4. let 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 아니다. 
var 키워드로 선언한 전역 변수와 전역 함수는 window 객체의 프로퍼티다.


<br/>
<br/>

### 📌 const 키워드

ES6에서 도입된 변수 선언 키워드이다.

#### 1. 반드시 선언과 동시에 초기화해야 한다.
선언과 할당이 한꺼번에 이루어져야 한다.

```jsx
const foo; //SyntaxError: Missing initializer in const declaration
```

<br/>

#### 2. let 키워드와 마찬가지로 블록 레벨 스코프를 가지며, 변수 호이스팅이 일어나지 않는 것처럼 동작한다.

<br/>

#### 3. const 키워드로 선언한 변수는 재할당이 금지된다.

```jsx
const foo = 1;
foo = 2; //TypeError: Assignment to constant variable.
```
이러한 특징을 이용해 상수를 표현하는 데 사용하기도 한다.
일반적으로 상수는 대문자로 선언하며 스네이크 케이스를 사용한다.
```jsx
const SECRET_KEY = 'abc'
```

<br />

#### 4. const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다.
객체는 재할당 없이도 직접 변경 가능하기 때문이다.

즉, const 키워드로 선언한 변수는 재할당 금지를 의미할 뿐 불변을 의미하지 않는 것이다.

```jsx
const result = [];
result.push('a');

console.log(result) //['a']
```

이때 객체가 변경되더라도, 변수에 할당된 객체의 참조 값은 변하지 않는다.


<br/>
<br/>

### 📌 var, let, const

변수의 의도치 않은 재할당을 방지하여 안전하게 코드를 짜도록 한다.

- 변수 선언에 기본적으로 const 키워드를 사용한다.
읽기 전용으로 사용하는 원시 값과 객체에는 const 키워드를 사용한다.

- ES6을 사용한다면 var 키워드를 사용하지 않는다.

- 재할당이 필요한 경우에 한정해 let 키워드를 사용한다.


