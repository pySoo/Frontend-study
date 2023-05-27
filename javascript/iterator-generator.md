# 이터레이터, 제너레이터

## 이터러블(iterable)

- `이터러블 프로토콜을 준수`한 객체.
- ‘이터러블 프로토콜을 준수’했다는 것은 `Symbol.iterator`를 프로퍼티로 사용한 메서드를 직접 구현하거나, 프로토타입 체인을 통해 상속받은 객체를 뜻한다.

- 쉽게 말하면, 반복이 가능하며 `[Symbol.iterator]()`를 가진 객체이다.
- 배열, 문자열, Map, Set 모두 이터러블이다.

#### 특징 1. 반복 가능한 객체

```jsx
// 반복이 가능한 객체, for of 반복문은 iterable 요소들만 수행할 수 있다.
const obj = { a: 1, b: 2 };
for (item of obj) {
  console.log(item);
}

// TypeError: obj is not iterable
```

#### 특징 2. iterator를 리턴하는 `[Symbol.iterator]()` 속성을 가진 객체

```jsx
const arr = [1, 2, 3, 4];
const set = new Set([1, 2, 3, 3, 5, 2]);
const map = new Map([
  ["a", "A"],
  ["b", "B"],
]);
const obj = { a: 1, b: 2 };

console.log(arr[Symbol.iterator]());
console.log(set[Symbol.iterator]());
console.log(map[Symbol.iterator]());
console.log(obj[Symbol.iterator]());
```

- 실행 결과: 일반 객체( obj = { a: 1, b: 2 } )는 iterable이 아님

```java
Object [Array Iterator] {}
[Set Iterator] { 1, 2, 3, 5 }
[Map Entries] { [ 'a', 'A' ], [ 'b', 'B' ] }
console.log(obj[Symbol.iterator]());
                                ^

TypeError: obj[Symbol.iterator] is not a function
```

## 이터레이터(iterator)

- 이터러블에 Symbol.iterator 메서드를 호출했을 때 반환되는 값

```jsx
const arr = [1, 2, 3];
const iterator = arr[Symbol.iterator()];
```

- 이터레이터는 next라는 메서드를 가지고 있는데, 이걸 이용해서 각 요소를 순회할 수 있다.
- next 메서드를 호출하면 Iterator result object가 반환된다. 이 객체는 value와 done이라는 프로퍼티를 가지고 있다.

```jsx
const arr = [1, 2, 3];
const iterator = arr[Symbol.iterator()];

const iteratorResultObject = iterator.next();
console.log(iteratorResultObject); // { value: 1, done: false }
```

## 제너레이터

- 이터레이터를 발생시키는 함수

```java
// function 뒤에 *을 붙이는 것이 규칙이다.
function* generator () {}
```

- 제너레이터 함수 내부에 `yield` 키워드를 이용해서 이터러블의 요소를 표현식으로 나타낼 수 있다.
- `yield` 옆의 표현식이 배열의 요소가 된 것처럼 동작하고 이것을 순회할 수 있다.

```java
function* generator() {
	yield 1;
	yield 2;
	yield 3;
	return -1;
}

const iterable = generator();
for (item of iterable) console.log(item);

// 1
// 2
// 3
```

#### 여기서 return 값은 언제 쓰이는 것일까?

- next()를 통해 객체를 순회할 때 `done이 true`인 경우 return 값이 value에 할당된다.

```java
function* generator() {
	yield 1;
	yield 2;
	yield 3;
	return -1;
}

const iterable = generator();
console.log(iterable.next()); // { value: 1, done: false }
console.log(iterable.next()); // { value: 2, done: false }
console.log(iterable.next()); // { value: 3, done: false }
console.log(iterable.next()); // { value: -1, done: true }
console.log(iterable.next()); // { value: undefined, done: true }
```
