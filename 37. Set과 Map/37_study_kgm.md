# 📗챕터 정리

## Set ❔

 Set 객체 는 중복되지 않는 **유일한 값들의 집합**이며, 배열과 유사하지만 몇 가지 차이가 있다.

| 구분 | 배열 | Set 객체 |
| --- | --- | --- |
| 동일한 값을 중복하여 포함할 수 있다. | O | X |
| 요소 순서에 의미가 있다. | O | X |
| 인덱스로 요소에 접근할 수 있다. | O | X |

Set 객체는 수학적 집합을 구현하기 위한 자료구조이며, 교집합, 합집합, 차집합, 여집합 등을 구현할 수 있다.

Set 생성자 함수는 이터러블을 인수로 전달 받아 Set 객체를 생성한다.

이 때, 이터러블의 중복된 값은 Set 객체의 요소로 저장하지 않는다.

(중복을 허용하지 않는 Set 객체의 특성을 활용하여 배열에서 중복된 요소를 제거할 수 있다.)

```jsx
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}

const set2 = new Set('hello');
console.log(set2); // Set(4) {"h", "e", "l", "o"}
```

### Set 응용

1. `Set.prototype.size`
    
    Set 객체의 요소 개수를 확인한다.
    
    ```jsx
    const { size } = new Set([1, 2, 3, 3]);
    console.log(size); // 3
    ```
    
    `size` 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티이다.
    
    따라서 `size` 프로퍼티에는 값 할당이 불가능하고, Set 객체의 요소 개수 역시 변경할 수 없다.
    
    ```jsx
    const set = new Set([1, 2, 3]);
    
    console.log(Object.getOwnPropertyDescriptor(Set.prototype, 'size'));
    // {set: undefined, enumerable: false, configurable: true, get: ƒ}
    
    set.size = 10; // 무시된다.
    console.log(set.size); // 3
    ```
    
2. `Set.prototype.add`
    
    새로운 요소가 추가된 Set 객체를 반환한다.
    
    배열과 객체를 포함한 자바스크립트의 모든 값을 인수로 사용하여 Set 객체의 새로운 요소로 추가할 수 있다.
    
    ```jsx
    const set = new Set();
    console.log(set); // Set(0) {}
    
    set.add(1);
    console.log(set); // Set(1) {1}
    ```
    
    `add` 메서드를 연속으로 호출할 수 있고, 중복된 요소는 추가할 수 없다. (에러가 발생하지는 않는다.)
    
    중복의 기준이 일치 비교 연산자와 약간 다른데 `NaN`과 `NaN`, `+0`과 `-0` 중복을 허용하지 않는다.
    
    (일치 비교 연산자는 `NaN`과 `NaN`, `+0`과 `-0` 은 다르다고 평가한다.)
    
    ```jsx
    const set = new Set();
    
    console.log(NaN === NaN); // false
    console.log(0 === -0); // true
    
    set.add(NaN).add(NaN);
    console.log(set); // Set(1) {NaN}
    
    set.add(0).add(-0);
    console.log(set); // Set(2) {NaN, 0}
    ```
    
3. `Set.prototype.has`
    
    특정 요소가 존재하는지 확인하여 결과를 불리언 값을 반환한다.
    
    ```jsx
    const set = new Set([1, 2, 3]);
    
    console.log(set.has(2)); // true
    console.log(set.has(4)); // false
    ```
    
4. `Set.prototype.delete`
    
    특정 요소를 삭제하고 삭제 성공 여부를 불리언 값으로 반환한다.
    
    메서드를 사용할 때는 요소의 인덱스가 아닌 값을 인수로 전달해야 한다.
    
    존재하지 않는 요소를 삭제하면 에러 없이 무시된다.
    
    ```jsx
    const set = new Set([1, 2, 3]);
    
    set.delete(2);
    console.log(set); // Set(2) {1, 3}
    
    set.delete(1);
    console.log(set); // Set(1) {3}
    
    // 이미 삭제되어 존재하지 않는 1을 다시 삭제해도 에러가 발생하지 않는다.
    set.delete(1);
    console.log(set); // Set(1) {3}
    ```
    
5. `Set.prototype.clear`
    
    Set 객체의 요소를 일괄 삭제하고, 항상 `undefined`를 반환한다.
    
    ```jsx
    const set = new Set([1, 2, 3]);
    
    set.clear();
    console.log(set); // Set(0) {}
    ```
    
6. `Set.prototype.forEach`
    
    Set 객체의 요소를 순회할 때 사용한다.
    
    배열의 `forEach` 메서드와 유사하게 콜백 함수를 사용한다.
    
    이 때, 콜백 함수는 3개의 인수를 전달 받는다.
    
    > 첫 번째 인수 : 현재 순회 중인 요소 값
    두 번째 인수 : 현재 순회 중인 요소 값
    세 번째 인수 : 현재 순회 중인 Set 객체
    > 
    
    이처럼 동작하는 이유는 `Array.prototype.forEach` 메서드와 인터페이스를 통일하기 위함이다.
    
    ```jsx
    const set = new Set([1, 2, 3]);
    
    set.forEach((v, v2, set) => console.log(v, v2, set));
    /*
    1 1 Set(3) {1, 2, 3}
    2 2 Set(3) {1, 2, 3}
    3 3 Set(3) {1, 2, 3}
    */
    ```
    
    Set 객체는 이터러블이기 때문에 `forEach` 메서드 대신 `for … of` 문으로도 순회할 수 있다.
    
    또한, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수 있다.
    
    ```jsx
    const set = new Set([1, 2, 3]);
    
    // Set 객체는 Set.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
    console.log(Symbol.iterator in set); // true
    
    // 이터러블인 Set 객체는 for...of 문으로 순회할 수 있다.
    for (const value of set) {
      console.log(value); // 1 2 3
    }
    
    // 이터러블인 Set 객체는 스프레드 문법의 대상이 될 수 있다.
    console.log([...set]); // [1, 2, 3]
    
    // 이터러블인 Set 객체는 배열 디스트럭처링 할당의 대상이 될 수 있다.
    const [a, ...rest] = [...set];
    console.log(a, rest); // 1, [2, 3]
    ```
    

### Set 객체 집합 연산

Set 객체는 수학적 집합을 구현하기 위해 만들어진 자료 구조이다.

때문에 Set 객체를 통해 교집합, 합집합, 차집합, 여집합 등을 구현할 수 있다.

1. 교집합
    
    ```jsx
    Set.prototype.intersection = function (set) {
      const result = new Set();
    
      for (const value of set) {
        // 2개의 set의 요소가 공통되는 요소이면 교집합의 대상이다.
        if (this.has(value)) result.add(value);
      }
    
      return result;
    };
    
    const setA = new Set([1, 2, 3, 4]);
    const setB = new Set([2, 4]);
    
    // setA와 setB의 교집합
    console.log(setA.intersection(setB)); // Set(2) {2, 4}
    // setB와 setA의 교집합
    console.log(setB.intersection(setA)); // Set(2) {2, 4}
    ```
    
    ```jsx
    Set.prototype.intersection = function (set) {
      return new Set([...this].filter(v => set.has(v)));
    };
    
    const setA = new Set([1, 2, 3, 4]);
    const setB = new Set([2, 4]);
    
    // setA와 setB의 교집합
    console.log(setA.intersection(setB)); // Set(2) {2, 4}
    // setB와 setA의 교집합
    console.log(setB.intersection(setA)); // Set(2) {2, 4}
    ```
    
2. 합집합
    
    ```jsx
    Set.prototype.union = function (set) {
      // this(Set 객체)를 복사
      const result = new Set(this);
    
      for (const value of set) {
        // 합집합은 2개의 Set 객체의 모든 요소로 구성된 집합이다. 중복된 요소는 포함되지 않는다.
        result.add(value);
      }
    
      return result;
    };
    
    const setA = new Set([1, 2, 3, 4]);
    const setB = new Set([2, 4]);
    
    // setA와 setB의 합집합
    console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
    // setB와 setA의 합집합
    console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
    ```
    
    ```jsx
    Set.prototype.union = function (set) {
      return new Set([...this, ...set]);
    };
    
    const setA = new Set([1, 2, 3, 4]);
    const setB = new Set([2, 4]);
    
    // setA와 setB의 합집합
    console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
    // setB와 setA의 합집합
    console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
    ```
    
3. 차집합
    
    ```jsx
    Set.prototype.difference = function (set) {
      // this(Set 객체)를 복사
      const result = new Set(this);
    
      for (const value of set) {
        // 차집합은 어느 한쪽 집합에는 존재하지만 다른 한쪽 집합에는 존재하지 않는 요소로 구성된 집합이다.
        result.delete(value);
      }
    
      return result;
    };
    
    const setA = new Set([1, 2, 3, 4]);
    const setB = new Set([2, 4]);
    
    // setA에 대한 setB의 차집합
    console.log(setA.difference(setB)); // Set(2) {1, 3}
    // setB에 대한 setA의 차집합
    console.log(setB.difference(setA)); // Set(0) {}
    ```
    
    ```jsx
    Set.prototype.difference = function (set) {
      return new Set([...this].filter(v => !set.has(v)));
    };
    
    const setA = new Set([1, 2, 3, 4]);
    const setB = new Set([2, 4]);
    
    // setA에 대한 setB의 차집합
    console.log(setA.difference(setB)); // Set(2) {1, 3}
    // setB에 대한 setA의 차집합
    console.log(setB.difference(setA)); // Set(0) {}
    ```
    
4. 부분 집합과 상위 집합
    
    ```jsx
    // this가 subset의 상위 집합인지 확인한다.
    Set.prototype.isSuperset = function (subset) {
      for (const value of subset) {
        // superset의 모든 요소가 subset의 모든 요소를 포함하는지 확인
        if (!this.has(value)) return false;
      }
    
      return true;
    };
    
    const setA = new Set([1, 2, 3, 4]);
    const setB = new Set([2, 4]);
    
    // setA가 setB의 상위 집합인지 확인한다.
    console.log(setA.isSuperset(setB)); // true
    // setB가 setA의 상위 집합인지 확인한다.
    console.log(setB.isSuperset(setA)); // false
    ```
    
    ```jsx
    // this가 subset의 상위 집합인지 확인한다.
    Set.prototype.isSuperset = function (subset) {
      const supersetArr = [...this];
      return [...subset].every(v => supersetArr.includes(v));
    };
    
    const setA = new Set([1, 2, 3, 4]);
    const setB = new Set([2, 4]);
    
    // setA가 setB의 상위 집합인지 확인한다.
    console.log(setA.isSuperset(setB)); // true
    // setB가 setA의 상위 집합인지 확인한다.
    console.log(setB.isSuperset(setA)); // fals
    ```

## Map❔

 Map 객체 는 **키와 값의 쌍으로 이루어진 컬렉션**이며, 객체과 유사하지만 몇 가지 차이가 있다.

| 구분 | 배열 | Map 객체 |
| --- | --- | --- |
| 키로 사용할 수 있는 값 | 문자열 또는 심볼 | 객체를 포함한 모든 값 |
| 이터러블 | X | O |
| 요소 개수 확인 | Object.keys(obj).length | map.size |

Map 생성자 함수는 이터러블을 인수로 전달 받아 Map 객체를 생성한다.

이 때, 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 한다.

```jsx
const map1 = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(map1); // Map(2) {"key1" => "value1", "key2" => "value2"}

const map2 = new Map([1, 2]); // TypeError: Iterator value 1 is not an entry object
```

Map 생성자 함수의 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재하면 값을 덮어 쓴다.

따라서 Map 객체에는 중복된 키를 갖는 요소가 존재할 수 없다.

```jsx
const map = new Map([['key1', 'value1'], ['key1', 'value2']]);
console.log(map); // Map(1) {"key1" => "value2"}
```

### Map 응용

1. `Map.prototype.size`
    
    Map 객체의 요소 개수를 확인한다.
    
    ```jsx
    const { size } = new Map([['key1', 'value1'], ['key2', 'value2']]);
    console.log(size); // 2
    ```
    
    `size` 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티이다.
    
    따라서 `size` 프로퍼티에는 값 할당이 불가능하고, Map 객체의 요소 개수 역시 변경할 수 없다.
    
    ```jsx
    const map = new Map([['key1', 'value1'], ['key2', 'value2']]);
    
    console.log(Object.getOwnPropertyDescriptor(Map.prototype, 'size'));
    // {set: undefined, enumerable: false, configurable: true, get: ƒ}
    
    map.size = 10; // 무시된다.
    console.log(map.size); // 2
    ```
    
2. `Map.prototype.set`
    
    새로운 요소가 추가된 Map 객체를 반환한다.
    
    ```jsx
    const map = new Map();
    console.log(map); // Map(0) {}
    
    map.set('key1', 'value1');
    console.log(map); // Map(1) {"key1" => "value1"}
    ```
    
    `set` 메서드를 연속으로 호출할 수 있고, 중복된 키를 갖는 요소는 존재할 수 없다.
    
    중복된 키를 갖는 요소를 추가하면 갚을 덮어쓰게 된다.
    
    ```jsx
    const map = new Map();
    
    map
      .set('key1', 'value1')
      .set('key1', 'value2');
    
    console.log(map); // Map(1) {"key1" => "value2"}
    ```
    
    중복의 기준이 일치 비교 연산자와 약간 다른데 `NaN`과 `NaN`, `+0`과 `-0` 중복을 허용하지 않는다.
    
    (일치 비교 연산자는 `NaN`과 `NaN`, `+0`과 `-0` 은 다르다고 평가한다.)
    
    ```jsx
    const map = new Map();
    
    console.log(NaN === NaN); // false
    console.log(0 === -0); // true
    
    // NaN과 NaN을 같다고 평가하여 중복 추가를 허용하지 않는다.
    map.set(NaN, 'value1').set(NaN, 'value2');
    console.log(map); // Map(1) { NaN => 'value2' }
    
    // +0과 -0을 같다고 평가하여 중복 추가를 허용하지 않는다.
    map.set(0, 'value1').set(-0, 'value2');
    console.log(map); // Map(2) { NaN => 'value2', 0 => 'value2' }
    ```
    
    객체는 문자열, 심벌 값만 키로 사용할 수 있지만 Map 객체는 키 타입에 제한이 없다.
    
    따라서 Map 객체는 **객체의 키로 객체를 포함한 모든 유형의 값을 사용**할 수 있다.
    
    ```jsx
    const map = new Map();
    
    const lee = { name: 'Lee' };
    const kim = { name: 'Kim' };
    
    // 객체도 키로 사용할 수 있다.
    map
      .set(lee, 'developer')
      .set(kim, 'designer');
    
    console.log(map);
    // Map(2) { {name: "Lee"} => "developer", {name: "Kim"} => "designer" }
    ```
    
3. `Map.prototype.get`
    
    Map 객체가 특정 요소를 취득할 때 사용한다.
    
    `get` 메서드의 인수로 키를 전달하면 인수로 전달한 키를 갖는 값을 반환한다.
    
    인수로 전달한 키를 갖는 값이 존재하지 않으면 `undefined`를 반환한다.
    
    ```jsx
    const map = new Map();
    
    const lee = { name: 'Lee' };
    const kim = { name: 'Kim' };
    
    map
      .set(lee, 'developer')
      .set(kim, 'designer');
    
    console.log(map.get(lee)); // developer
    console.log(map.get('key')); // undefined
    ```
    
4. `Map.prototype.has`
    
    특정 키를 가진 요소가 존재하는지 확인하고 결과를 불리언 값으로 반환한다.
    
    ```jsx
    const lee = { name: 'Lee' };
    const kim = { name: 'Kim' };
    
    const map = new Map([[lee, 'developer'], [kim, 'designer']]);
    
    console.log(map.has(lee)); // true
    console.log(map.has('key')); // false
    ```
    
5. `Map.prototype.delete`
    
    특정 키를 가진 요소를 삭제하고 삭제 성공 여부를 불리언 값으로 반환한다.
    
    존재하지 않는 키로 요소를 삭제하려고 하면 에러 없이 무시된다.
    
    ```jsx
    const lee = { name: 'Lee' };
    const kim = { name: 'Kim' };
    
    const map = new Map([[lee, 'developer'], [kim, 'designer']]);
    
    map.delete(kim);
    console.log(map); // Map(1) { {name: "Lee"} => "developer" }
    
    // 이미 삭제되어 존재하지 않는 key로 다시 삭제해도 에러가 발생하지 않는다.
    map.delete(kim);
    console.log(map); // Map(1) { {name: "Lee"} => "developer" }
    ```
    
6. `Map.prototype.clear`
    
    Map 객체의 요소를 일괄 삭제하고, 항상 `undefined`를 반환한다.
    
    ```jsx
    const lee = { name: 'Lee' };
    const kim = { name: 'Kim' };
    
    const map = new Map([[lee, 'developer'], [kim, 'designer']]);
    
    map.clear();
    console.log(map); // Map(0) {}
    ```
    
7. `Map.prototype.forEach`
    
    Map 객체의 요소를 순회할 때 사용한다.
    
    배열의 `forEach` 메서드와 유사하게 콜백 함수를 사용한다.
    
    이 때, 콜백 함수는 3개의 인수를 전달 받는다.
    
    > 첫 번째 인수 : 현재 순회 중인 요소 값
    두 번째 인수 : 현재 순회 중인 요소 키
    세 번째 인수 : 현재 순회 중인 Map 객체
    > 
    
    이처럼 동작하는 이유는 `Array.prototype.forEach` 메서드와 인터페이스를 통일하기 위함이다.
    
    ```jsx
    const lee = { name: 'Lee' };
    const kim = { name: 'Kim' };
    
    const map = new Map([[lee, 'developer'], [kim, 'designer']]);
    
    map.forEach((value, key, map) => console.log(value, key, map));
    /*
    developer {name: "Lee"} Map(2) {
      {name: "Lee"} => "developer",
      {name: "Kim"} => "designer"
    }
    designer {name: "Kim"} Map(2) {
      {name: "Lee"} => "developer",
      {name: "Kim"} => "designer"
    }
    */
    ```
    
    Map 객체는 이터러블이기 때문에 `forEach` 메서드 대신 `for … of` 문으로도 순회할 수 있다.
    
    또한, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수 있다.
    
    ```jsx
    const lee = { name: 'Lee' };
    const kim = { name: 'Kim' };
    
    const map = new Map([[lee, 'developer'], [kim, 'designer']]);
    
    // Map 객체는 Map.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
    console.log(Symbol.iterator in map); // true
    
    // 이터러블인 Map 객체는 for...of 문으로 순회할 수 있다.
    for (const entry of map) {
      console.log(entry); // [{name: "Lee"}, "developer"]  [{name: "Kim"}, "designer"]
    }
    
    // 이터러블인 Map 객체는 스프레드 문법의 대상이 될 수 있다.
    console.log([...map]);
    // [[{name: "Lee"}, "developer"], [{name: "Kim"}, "designer"]]
    
    // 이터러블인 Map 객체는 배열 디스트럭처링 할당의 대상이 될 수 있다.
    const [a, b] = map;
    console.log(a, b); // [{name: "Lee"}, "developer"]  [{name: "Kim"}, "designer"]
    ```
    
8. Map 객체 요소 순회 (`keys` / `values` / `entries`)
    
    Map 객체는 이터러블이며 동시에 이터레이터인 객체를 반환하는 메서드를 제공한다.
    
    ```jsx
    const lee = { name: 'Lee' };
    const kim = { name: 'Kim' };
    
    const map = new Map([[lee, 'developer'], [kim, 'designer']]);
    
    // Map.prototype.keys는 Map 객체에서 요소키를 값으로 갖는 이터레이터를 반환한다.
    for (const key of map.keys()) {
      console.log(key); // {name: "Lee"} {name: "Kim"}
    }
    
    // Map.prototype.values는 Map 객체에서 요소값을 값으로 갖는 이터레이터를 반환한다.
    for (const value of map.values()) {
      console.log(value); // developer designer
    }
    
    // Map.prototype.entries는 Map 객체에서 요소키와 요소값을 값으로 갖는 이터레이터를 반환한다.
    for (const entry of map.entries()) {
      console.log(entry); // [{name: "Lee"}, "developer"]  [{name: "Kim"}, "designer"]
    }
    ```

<br/>

## 🤔궁금한 점

## 📌중요한 점
