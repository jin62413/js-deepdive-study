# 목차

[📗배운 점](#📗배운-점)

[📌중요한 점](#📌중요한-점)

[🤔궁금한 점](#🤔궁금한-점)

## 📗배운 점

## 33.1 심벌이란? 

변경 불가 원시타입값 - 문자열, 숫자, 불리언, undefined, null, 객체 타입 거기에 심벌symbol은 ES6에서 도입된 7번째 데이터 타입 심벌
원시타입의 값은 다른 값 과 중복되지 않는 유일무이한 값이다. 따라서 주로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용한다. 프로퍼티 키로 사용할 수 있는 값은 빈 문자열을 포함하는 모든 문자열 또는 심벌 값이다.

## 33.2 심벌 값의 생성


### 33.2.1 Symbole 함수

심벌 값은 Symbol 함수를 호출하여 생성한다. 

문자열, 숫자, 불리언, undefined, null 타입 의 값 - 리터럴 표기법을 통해 값을 생성
심벌 값은 Symbol 함수를 호출하여 생성

 이 때 생성된 심벌 값은 외부로 노출되지 않아 확인할 수 없으며, 다른 값과 절대 중복되지 않는 유일무이한 값 이다.

 ```javascript
// Symbol 함수를 호출하여 유일무이한 심벌 값을 생성한다.
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol

// 심벌 값은 외부로 노출되지 않아 확인할 수 없다.
console.log(mySymbol);        // Symbol()
```
Symbol 함수는 String, Number, Boolean 생성자 함수와는 달리 new 연산자와 함께 호출하지 않는다. 

new 연산자와 함께 생성자 함수 또는 클래스를 호출하면 객체(인스턴스)가 생성되지만 `심벌 값은 변경 불가능한 원시 값`이다.


```javascript
new Symbol(); // TypeError: Symbol is not a constructor
```

 Symbol 함수에는 선택적으로 문자열을 인수로 전달할 수 있다. 이 문자열은 생성된 심벌 값에 대한 설명de- scription 으로 `디버깅 용도`로만 사용되며, 심벌 값 생성에 어떠한 영향도 주지 않는다. 즉, 심벌 값에 대한 설명 이 같더라도 생성된` 심벌 값은 유일무이한 값`이다. 

```javascript
// 심벌 값에 대한 설명이 같더라도 유일무이한 심벌 값을 생성한다.
const mySymbol1 = Symbol('mySymbol');
const mySymbol2 = Symbol('mySymbol');

console.log(mySymbol1 === mySymbol2); // false
```

심벌 값도 문자열, 숫자, 불리언과 같이 `객체처럼 접근`하면 암묵적으로` 래퍼 객체'를 생성`한다. 다음 예제의 description 프로퍼티와 toString 메서드는 Symbol.prototype의 프로퍼티다.

```javascript
const mySymbol = Symbol('mySymbol');

// 심벌도 레퍼 객체를 생성한다
console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString());  // Symbol(mySymbol)
```

심벌 값은 암묵적으로 문자열이나 숫자 타입으로 `변환되지 않는다`. 


```javascript
const mySymbol = Symbol();

// 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.
console.log(mySymbol + ''); // TypeError: Cannot convert a Symbol value to a string
console.log(+mySymbol);     // TypeError: Cannot convert a Symbol value to a number
```

단, `불리언 타입으로`는 암묵적으로 타입 변환된다.



```javascript
const mySymbol = Symbol();

// 불리언 타입으로는 암묵적으로 타입 변환된다.
console.log(!!mySymbol); // true

// if 문 등에서 존재 확인을 위해 사용할 수 있다.
if (mySymbol) console.log('mySymbol is not empty.');
```



### 33.2.2 Symbole.for / Symbole.keyFor 메서드


Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리global symbol registry에서 해당 키와 일치하는 심벌 값을 검색한다. 
• 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환한다. 
• 검색에 실패하면 새로운 심벌 값을 생성하여 Symbol. for 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환한다.

```javascript
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol');
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값을 반환
const s2 = Symbol.for('mySymbol');

console.log(s1 === s2); // true
```

Symbol 함수는 호출될 때마다 `유일무이한 심벌 값을 생성`한다. 이때 자바스크립트 엔진이 관리하는 심벌 값 저장소인 전역 심벌 레지스트리에서 심벌 값을 검색할 수 있는 키를 지정할 수 없으므로 전역 심벌 레지스트 리에 등록되어 관리되지 않는다. 하지만 `Symbol. for 메서드`를 사용하면 애플리케이션 전역에서 중복되지 않는 유일무이한 `상수인 심벌 값을 단 하나만 생성`하여 `전역 심벌 레지스트리를 통해 공유`할 수 있다.

Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 `저장된 심벌 값의 키를 추출`할 수 있다.

```javascript
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol');
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s1); // -> mySymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s2 = Symbol('foo');
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s2); // -> undefined
```
## 33.3 심벌과 상수


```javascript
// 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의한다.
// 이때 값 1, 2, 3, 4에는 특별한 의미가 없고 상수 이름에 의미가 있다.
const Direction = {
  UP: 1,
  DOWN: 2,
  LEFT: 3,
  RIGHT: 4
};

// 변수에 상수를 할당
const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log('You are going UP.');
}
```

값에는 특별한 의미가 없고 상수 이름 자체에 의미가 있는 경우가 있다. 이때 문제는 상수 값 1,2,3,4가 변경될 수 있으며, 다른 변수 값과 중복될 수 있다는 것이다.이 경우 변경/중복될 가능성이 있는 무의미한 상수 대신 중복될 가능성이 없는 유일무이한 심벌 값을 사용할 수 있다.

```javascript
// 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의한다.
// 중복될 가능성이 없는 심벌 값으로 상수 값을 생성한다.
const Direction = {
  UP: Symbol('up'),
  DOWN: Symbol('down'),
  LEFT: Symbol('left'),
  RIGHT: Symbol('right')
};

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log('You are going UP.');
}
```

> enum은 명명된 숫자 상수named numeric constant의 집합으로 열거형enumerated type이라고 부른다. 자바스크립트는 enum을 지원하 지 않지만 C. 자바, 파이썬 등 여러 프로그래밍 언어와 자바스크립트의 상위 확장인 타입스크립트에서는 enum을 지원한다. 자바스크립트에서 enum을 흉내 내어 사용하려면 다음과 같이 객체의 변경을 방지하기 위해 객체를 동결하는 Object.freeze 메서드와 심벌 값을 사용한다.




## 33.4 심벌과 프로퍼티 키 

객체의 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 만들 수 있으며, 동적으로 생성 할 수도 있다.심벌 값을 프로퍼티 키로 사용하려면 프 로퍼티 키로 사용할 심벌 값에 대괄호를 사용해야 한다. 프로퍼티에 접근할 때도 마찬가지로 대괄호를 사용 해야 한다.


```javascript
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol.for('mySymbol')]: 1
};

obj[Symbol.for('mySymbol')]; // -> 1
```
심벌 값은 유일무이한 값이므로 `심벌 값으로 프로퍼티 키를 만들면` `다른 프로퍼티 키와 절대 충돌하지 않는다`. 기존 프로퍼티 키와 충돌하지 않는 것은 물론, 미래에 추가될 어떤 프로퍼티 키와도 충돌할 위험이 없다. 

## 33.5 심벌과 프로퍼티 은닉

심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 for in 문이나 Object.keys, Object. getOwnPropertyNames 메서드로 찾을 수 없다. 이처럼 심벌 값을 프로퍼티 키로 사용하여 프로퍼티를 생성하 면 외부에 노출할 필요가 없는 프로퍼티를 은닉할 수 있다.

```javascript
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol('mySymbol')]: 1
};

for (const key in obj) {
  console.log(key); // 아무것도 출력되지 않는다.
}

console.log(Object.keys(obj)); // []
console.log(Object.getOwnPropertyNames(obj)); // []
```

**Object.getOwnPropertySymbols 메서드:** 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티를 찾을 수 있다.


```javascript
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol('mySymbol')]: 1
};

// getOwnPropertySymbols 메서드는 인수로 전달한 객체의 심벌 프로퍼티 키를 배열로 반환한다.
console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(mySymbol)]

// getOwnPropertySymbols 메서드로 심벌 값도 찾을 수 있다.
const symbolKey1 = Object.getOwnPropertySymbols(obj)[0];
console.log(obj[symbolKey1]); // 1
```






## 33.6 심벌과 표준 빌트인 객체 확장 

일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장하지 않는다. 표준 빌트인 객체는 `읽기 전용`으로 사용하는 것이 좋다.

```javascript
// 표준 빌트인 객체를 확장하는 것은 권장하지 않는다.
Array.prototype.sum = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2].sum(); // -> 3
```

why ? 개발자가 직접 추가한 메서드와 미래에 표준 사양으로 추가될 메서드의 이름이 중복될 수 있기 때문. 

예를 들어, Array.prototype. find 메서드가 ES6에서 도입되기 이전에 Array.prototype에 사용자 정의 find 메서드를 직접 추가했다면 새롭게 도입된 ES6의 Array.prototype.find 메서드와 이름이 중복되어 ES6의 Array.prototype.find 메서드를 이전에 추가했던 사용자 정의 find 메서드가 덮어쓴다. 표준 빌트인 메서드를 사용자 정의 메서드가 덮어쓴다면 문제가 된다. 

하지만 중복될 가능성이 없는 심벌 값으로 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장하면 표준 빌트 인 객체의 기존 프로퍼티 키와 충돌하지 않는 것은 물론, 표준 사양의 버전이 올라감에 따라 추가될지 모르는 어떤 프로퍼티 키와도 충돌할 위험이 없어 안전하게 표준 빌트인 객체를 확장할 수 있다. 

```javascript
// 심벌 값으로 프로퍼티 키를 동적 생성하면 다른 프로퍼티 키와 절대 충돌하지 않아 안전하다.
Array.prototype[Symbol.for('sum')] = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2][Symbol.for('sum')](); // -> 3
```

## 33.7 Well-Known Symbol

자바스크립트가 기본 제공하는 빌트인 심벌 값이 있다. 빌트인 심벌 값은 Symbol 함수의 프로퍼티에 할당되어 있다.

자바스크립트가 기본 제공하는 빌트인 심벌 값을 ECMAScript 사양에서는 Well-known Symbol 이라 부 른다. Well-known Symbol은 자바스크립트 엔진의 내부 알고리즘에 사용된다. 예를 들어, Array, String, Map, Set, TypedArray, arguments, NodeList, HTMLCollection과 같이 for... of 문으로 순회 가능한 빌트인 이터러블은 Well-known Symbol인 Symbol.iterator를 키로 갖는 메서드를 가지며 Symbol.iterator c메서드를 호출하면 이터레이터'를 반환하도록 ECMAScript 사양에 규정되어 있 다. 빌트인 이터러블은 이 규정 즉, 이터레이션 프로토콜을 준수한다. 만약 빌트인 이터러블이 아닌 일반 객체를 이터러블처럼 동작하도록 구현하고 싶다면 이터레이션 프로토콜 을 따르면 된다.

 즉, ECMAScript 사양에 규정되어 있는 대로 Well-known Symbol인 Symbol. iterator를 키로 갖는 메서드를 객체에 추가하고 이터레이터를 반환하도록 구현하면 그 객체는 이터러블이 된다. 


```javascript
// 1 ~ 5 범위의 정수로 이루어진 이터러블
const iterable = {
  // Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수
  [Symbol.iterator]() {
    let cur = 1;
    const max = 5;
    // Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환
    return {
      next() {
        return { value: cur++, done: cur > max + 1 };
      }
    };
  }
};

for (const num of iterable) {
  console.log(num); // 1 2 3 4 5
}
```

이때 이터레이션 프로토콜을 준수하기 위해 일반 객체에 추가해야 하는 메서드의 키 Symbol.iterator는 기존 프로퍼티 키 또는 미래에 추가될 프로퍼티 키와 절대로 중복되지 않을 것이다. 이처럼 심벌은 중복되지 않는 상수 값을 생성하는 것은 물론 기존에 작성된 코드에 영향을 주지 않고 새로운 프로퍼티를 추가하기 위해, 즉 하위 호환성을 보장하기 위해 도입되었다. 



## 📌중요한 점


## 🤔궁금한 점