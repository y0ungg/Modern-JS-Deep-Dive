# Week 3 (7/18~ 7/24)

##### `JavaScript`

## 8. 제어문

일반적으로 코드는 위에 아래 방향으로 순차적으로 실행된다.  <br/>
제어문을 사용하면 코드의 실행 흐름을 인위적으로 제어할 수 있다.

### 📌 블록문

0개 이상의 문을 `{중괄호}`로 묶은 코드 블록<br/>
자바스크립트는 블록문을 하나의 실행 단위로 취급한다.

블록문은 자체 종결성을 갖기 때문에 문의 끝에 세미콜론을 붙이지 않는다.

<br/>
<br/>

### 📌 조건문

`boolean` 값으로 평가될 수 있는 표현식을 `조건식`으로 갖는다.<br/>
`조건식`의 평가 결과에 따라 코드 블록의 실행이 결정된다.

### if...else

if문의 `조건식`은 `boolean` 값으로 평가되어야 한다.<br/>
만약 `boolean`이 아닌 값으로 평가되면 자바스크립트 엔진에 의해 `boolean` 값으로 `암묵적 타입 변환`된다.<br/>
만약 코드 블록 안에 문이 하나뿐이라면 중괄호를 생략할 수 있다.

```jsx
if (x > 0) return y;
else return x;
```

<br/>

### switch

```jsx
switch(1 + 1) {
	case 2:
		//실행문
		break;
	case 3:
		//실행문
		break;
	default:
		//실행문 //일치하는 case문이 없을 때 실행된다.
}
```

switch문의 표현식은 `boolean` 값보다 `문자열`이나 `숫자` 값인 경우가 많다.<br/>
`boolean` 값에 따른 실행 결정보다 다양한 상황(case)에 따라 실행할 코드 블록을 결정할 때 사용한다.<br/>

> case문 실행 후 switch문을 탈출하지 않으면 이후의 모든 case문을 거치게 되는데 이를 fall through라고 한다.

break문은 코드 블록에서 탈출하는 역할을 하기 위해 필요한 존재다.

default문은 맨 마지막에 위치하므로 break문을 생략하는 것이 일반적이다. 

같은 실행문을 갖는 여러 개의 case문이 있는 경우에는  fall through를 활용해 하나의 조건으로 사용할 수 있다.

```jsx
const month = 2;
const days = 0;
switch (month) {
	case 1: case 3: case 5: case 7: case 8: case 10: case 12:
	days = 31;
	break;
	case 4: case 6: case 9: case 11:
	days = 30;
	break;
	case 2:
	days = 28;
	break;
	default:
		console.log('error');
};
console.log(days) //28
```

<br/>
<br/>

## 📌 반복문

for문, while문, do…while문 

### for

`조건식`이 false로 평가될 때까지 코드 블록을 반복 실행한다.

변수 선언문, 조건식, 증감식을 반드시 모두 사용할 필요는 없다.

단, 어떤 식도 선언하지 않으면 false로 평가되지 못함으로 해당 반복문은 무한루프가 된다.

<br/>

### while

`조건식`이 true로 평가되는 동안 코드 블록을 반복 실행한다.

⇒ 언제나 true라면 무한루프가 된다.  코드블록 내에 if문으로 탈출 조건을 만들고 break문을 작성해 탈출한다.

for문이 반복 횟수가 명확할 때 사용된다면, while문은 반복 횟수가 불명확할 때 주로 사용한다.

만약 `조건식` 의 평가 결과가 boolean 값이 아니면 boolean 값으로 `암묵적 타입 변환`하여 truthy, falsy한 값을 구별한다.

<br/>

### do…while

코드 블록을 먼저 실행하고 `조건식`을 평가한다.

```jsx
do {
	실행문 //조건식이 true인 동안 코드 블록을 반복 실행한다.
}while(조건식)
```

<br/>

### break

레이블 문, 반복문 또는 switch문의 코드 블록을 탈출한다.

그 외 SyntaxError 발생

중첩된 for문의 내부 for문에서 break 문을 실행하면 외부 for문으로 진입한다.

for문 내부의 if문에서 실행할 시 반복문을 탈출한다.

<br/>

### Continue

반복문의 실행문을 중단하고, 반복문의 증감식으로 실행 흐름을 이동한다.

가독성에 따라 if문과 둘 중 선택해서 사용한다.