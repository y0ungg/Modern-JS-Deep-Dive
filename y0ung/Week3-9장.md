# Week 3 (7/18~ 7/24)
###### `JavaScript`

## 9. 타입 변환과 단축 평가

**명시적 타입 변환** = 타입 캐스팅, 의도적으로 값의 타입을 변환하는 것
**암묵적 타입 변환** = 타입 강제 변환, 자바스크립트 엔진에 의해 값의 타입이 변경되는 것

원시 값은 immutable value로, 다른 타입의 새로운 원시 값을 생성한다.
암묵적 타입 변환은 연산 과정에서 일회성으로 적용된다.

>개발자는 자신이 작성한 코드에서 암묵적 타입 변환이 발생할지, 그렇다면 어떤 타입으로 변환될지, 나아가 표현식이 어떻게 평가될지 예측하고 코드를 짜야 한다.

<br/>

### 📌 암묵적 타입 변환
자바스크립트 엔진이 코드의 문맥을 고려해 `문자열`, `숫자`, `불리언` 중 하나로 타입을 자동 변환하는 것


#### 1. 문자열 타입으로 변환
문자열 연결 연산자(`+`) 표현식에서 피연산자를 `문자열`로 암묵적 타입 변환한다.

이때 `symbol`은 TypeError와 함께 문자열로 형변환될 수 없음을 알린다.
```jsx
//symbol
(Symbol()) + '' //TypeError: Cannot convert a Symbol value to a string

//object
({}) + '' //'[object Object]'
Math + '' //'[object Math]'
[] + '' // ''
Array + '' //'function Array() { [native code] }'
(function(){}) + '' //'function(){}'
```

<br/>

#### 2. 숫자 타입으로 변환

산술 연산자, 비교 연산자 표현식에서 피연산자를 `숫자`로 암묵적 타입 변환한다.

이때, 피연산자를 `숫자`로 형변환 할 수 없는 경우 `NaN`을 반환한다.

빈 문자열, 빈 배열, `null`, `false`는 0으로, `true`는 1로 변환된다.
객체와 빈 배열이 아닌 배열, `undefined`는 변환되지 않아 `NaN`이 된다.
`symbol`은 TypeError와 함께 숫자로 형변환될 수 없음을 알린다.

<br/>

#### 3. 불리언 타입으로 변환

조건식의 평가 결과를 불리언으로 암묵적 타입 변환한다.
`true`와 `false`가 아닌 표현식을 **truthy값과 falsy값**으로 구분한다.

- false
- undefined
- null
- 0, -0
- NaN
- 빈 문자열

위의 falsy값 외의 모든 값은 true로 평가되는 truthy값이다.



<br/>
<br/>

### 📌 명시적 타입 변환
빌트인 생성자 함수(String, Number, Boolean)을 new 연산자 없이 호출하거나, 빌트인 메서드를 사용하거나, 암묵적 타입 변환을 이용할 수 있다.


#### 1. 문자열 타입으로 변환
```jsx
String(1) //'1'
true.toString() //'true'
NaN + '' // 'NaN'
```
<br/>

#### 2. 숫자 타입으로 변환
`parseInt`, `parseFloat` 함수는 문자열에만 적용할수 있다.
단항 산술 연산자 `+`, 산술 연산자`*` 를 이용할 수도 있다.
```jsx
Number(true) //10
parseInt('0') //0
+false //0
'0' * 1 //0
'5.7' * 1 // 5.7
```
<br/>


#### 3. 불리언 타입으로 변환
부정 논리 연산자`!`를 두 번 사용하는 것으로 형변환할 수도 있다.
```jsx
Boolean('x') //true
Boolean('') //false
Boolean(undefined) //false
Boolean({}) //true

!!'x' //true
!!NaN //false
!!null //false
!![] //true
```
<br/>
<br/>

### 📌 단축 평가

#### 1. 논리 연산자를 사용한 단축 평가

논리곱 연산자`&&`는 좌항에서 우항으로 평가가 진행된다.
따라서 **우항의 피연산자가 평가 결과를 결정**한다.

논리곱 연산자는 연산 결과를 결정하는 **두 번째 피연산자를 그대로 반환한다**.

좌항이 truthy값일시, 우항의 평가 결과에 따라서 연산 결과가 달라진다.
->연산 결과에 상관 없이 두 번째 피연산자를 반환

좌항이 falsy값일시, 우항의 값과 상관 없이 논리 연산 결과가 false다.
->좌항을 반환함.


```jsx
'Cat' && 'Dog' //'Dog'
```

<br/>

논리합 연산자`||`는 좌항과 우항 중 하나만 true로 평가되어도 true를 반환한다.
따라서 논리 연산 결과를 결정한 **첫 번째 피연산자를 반환**한다.

좌항이 falsy값일시, 우항까지 살펴야 하므로 우항이 논리 연산 결과를 결정한다.
-> 우항을 반환함.

좌항이 truthy값일시, 우항을 살필 필요가 없으므로 좌항이 논리 연산 결과를 결정한다.
-> 좌항을 반환함.


```jsx
'Cat' || 'Dog' //'Cat'
```

`&&`과 `||`는 논리 연산의 결과를 결정하는 피연산자를 **타입변환하지 않고 그대로 반환**한다.
->이를 **단축평가**라고 한다.

단축 평가는 표현식을 평가하는 도중에 평가 결과가 확정된 경우, 나머지 평가 과정을 생략하는 것을 말한다.

<br/>

단축 평가를 사용하면 if문을 대체할 수 있다.
- 조건이 truthy 값일 때 무언가를 해야 한다면 `&&` 를 사용한다.
```jsx
const done = true;
message = done && '완료' //done이 truthy 값일시 '완료'를 할당

//위와 같은 동작을 하는 if문
if(done) message = '완료'
```

- 조건이 falsy 값일 때 무언가를 해야 한다면 `||`를 사용한다.
```jsx
const done = false;
message = done || '미완료' //done이 false기 때문에 '미완료' 할당

//위와 같은 동작을 하는 if문
if(!done) message = '미완료'
```


>객체 프로퍼티를 참조할 때 단축 평가가 유용하다.
```jsx
const obj = null;
const value = obj && obj.name; //obj.name 값이 있을 시 그것을 할당하고, 없으면 null을 할당
```
falsy한 변수로 객체 참조를 잘못해도 에러를 발생시키지 않는다.


>함수를 호출할 때 매개변수의 기본값을 설정함으로 undefined로 인한 에러를 방지할 수 있다.
```jsx
function myFunc(str) {
str = str || '' //undefined일시 기본값으로 빈 문자열 할당
return str.length;
}

//ES6 문법
function myFunc(str = '') {
return str.length;
}
```
<br/>

#### 2. 옵셔널 체이닝 연산자
ES11에 도입된 연산자 `?.`
객체 프로퍼티를 참조할 때 사용한다.

좌항의 피연산자(객체)가 **null또는 undefined인 경우 undefined를 반환**하고, 아닌 경우 우항의 프로퍼티 참조를 이어간다.

```
const obj = null;
const value = obj?.name; //obj가 null이므로 undefined 반환
```
좌항이 falsy한 값이어도 null 또는 undefined가 아니면 우항의 프로퍼티 참조를 진행한다.

<br/>

#### 3. null 병합 연산자
ES11에 도입된 연산자 `??`
좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.

`??`는 null 또는 undefined가 아니라면 falsy한 값이어도 할당하므로 논리합 연산자와 조금 다른 역할을 한다.

빈 문자열 등의 의도적으로 할당하려는 falsy한 값을 변수에 할당하고자 할 때 유용하다.

```jsx
const str = '' ?? 'default value' //str에 ''이 할당된다.


const udf = undefined;
const value = udf ?? 'default value' //value에 'default value'가 할당된다.
```
