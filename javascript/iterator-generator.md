## 이터레이터와 제너레이터

이번 글에서는 제너레이터를 이해하기 위해 필요한 이터러블, 이터레이터의 개념에 대해서 배워보고 제너레이터의 특징과 사용했을 때의 장점에 대해서 알아보고자 한다. 더불어 제너레이터를 사용하여 성능을 개선할 수 있는 방법인 지연 평가의 개념과 성능을 개선한 사례에 대해서도 알아보자.

## 이터러블(iterable)

- `이터러블 프로토콜을 준수`한 객체.
- ‘이터러블 프로토콜’이란 이터러블을 for...of, 전개 연산자 등과 함께 동작하도록 한 규약

- 쉽게 말하면, 반복이 가능하며 이터레이터를 리턴하는 `[Symbol.iterator]()`를 가진 객체이다.
- 배열, 문자열, Map, Set 모두 이터러블이다.

#### 특징 1. 반복 가능한 객체

```jsx
// 반복이 가능한 객체, for of 반복문은 iterable 요소들만 수행할 수 있다.
const obj = { a: 1, b: 2 };
for (item of obj) {
  console.log(item);
}

// TypeError: obj is not iterable (오브젝트는 iterable하지 않다)
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

실행 결과: 일반 객체( obj = { a: 1, b: 2 } )는 iterable이 아님

```java
Object [Array Iterator] {}
[Set Iterator] { 1, 2, 3, 5 }
[Map Entries] { [ 'a', 'A' ], [ 'b', 'B' ] }
console.log(obj[Symbol.iterator]());
                                ^

TypeError: obj[Symbol.iterator] is not a function
```

<br />

## 이터레이터(iterator)

- 이터러블에 Symbol.iterator 메서드로 접근했을 때 `{ value, done }` 객체로 반환되는 값

```jsx
const arr = [1, 2, 3];
const iterator = arr[Symbol.iterator()];
```

- 이터레이터는 `next`라는 메서드를 가지고 있는데, 이걸 이용해서 각 요소를 순회할 수 있다.
- next 메서드를 호출하면 Iterator result object가 반환된다. 이 객체는 value와 done이라는 프로퍼티를 가지고 있다.

```jsx
const arr = [1, 2, 3];
const iterator = arr[Symbol.iterator()];

const iteratorResultObject = iterator.next();
console.log(iteratorResultObject); // { value: 1, done: false }
```

<br />

## 제너레이터

- **이터레이터를 만드는 함수**
- `yield` 연산자를 사용해서 `원하는 위치에서 정지`한 다음 중지된 위치에서 로직이 실행되는 함수
- 제너레이터 함수를 호출하면 제너레이터 객체가 반환된다.

```java
// function 뒤에 *을 붙이는 것이 규칙이다.
function* generator () {}
```

### 이터러블 하다는 것은?

- `yield` 옆의 표현식이 `배열의 요소가 된 것처럼` 이터러블하게 동작하고 요소들을 `순회`할 수 있게 된다.

```java
function* generator() {
	yield 1;
	yield 2;
	yield 3;
	return -1;
}

const iterable = generator();
for (item of iterable) console.log(item); // [1,2,3] 처럼 동작

// 1
// 2
// 3
```

<br />

### 일반 함수와 어떤 점이 다른가요?

#### 1. `yield` 키워드를 이용해서 중간에 원하는 부분에서 멈추었다가 그 부분부터 다시 실행할 수 있다.

    - 일반 함수: 함수 호출자에게 함수 실행에 관한 제어권이 없다.

#### 2. 함수 호출자와 양방향으로 함수의 상태를 주고 받을 수 있다.

    - 일반 함수: 외부로부터 값을 전달 받고 실행되는 동안에는 함수의 상태를 변경할 수 없다.

#### 3. 호출 시 제너레이터 객체를 생성해서 반환한다.

    - 일반 함수: 함수의 코드 블록을 실행시킨다.

<br />

### 제너레이터의 next()와 return

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

<br />

### 제너레이터를 언제 사용하면 좋은가요?

- **값을 지연 평가할 때**
- 제너레이터는 이터레이터의 next()를 활용하여 값을 지연 평가할 수 있게 한다.

### 지연 평가

```java
const a = 3 + 5;
const b = 2 + 4;

console.log(a)
```

- 위 코드에서 b는 사용하지 않기 때문에 계산을 할 필요가 없다.
- 이와 같이, 불필요한 연산을 막기 위해 계산을 늦추는 개념을 지연 평가라고 부른다.

<br />

### 지연 평가 예시

1부터 N까지의 숫자 중, 10의 배수를 작은 순서대로 5개 찾아보자.

#### 방법 1. 배열로 만들기

```java
function makeArray(n) {
  const arr = [];
  let idx = 1;
  while (idx <= n) {
    arr.push(idx++);
  }
  return arr;
}
const arr = makeArray(50);
console.log(arr);

// [ 10, 20, 30, 40, 50]
```

- 배열이 만들어진다. 제너레이터를 사용하면 이 코드를 훨씬 효율적으로 개선할 수 있다.

#### 방법 2. 제너레이터로 만들기

```java
function* makeIterable(n) {
  let i = 1;
  while (i <= n) yield i++;
}
const iter = makeIterable(50);
console.log(iter);

// Object [Generator] {}
```

- 반복 가능한 object가 만들어졌다. 이전에는 1부터 n이 담긴 배열을 전달하고 있었다면, 새로운 코드에서는 `제너레이터 객체를 인자로 전달`하고 있다.
- 즉시 평가되지 않았기 때문에 배열이 만들어지지 않고 필요한 값만 뽑아서 사용하게 된다.

#### 같은 동작, 다른 성능

```java
function dividableWith10(iter) {
  const arr = [];
  for (const elem of iter) {
    if (elem % 10 === 0) {
      arr.push(elem);
    } else if (arr.length === 5) {
      return arr;
    }
  }
}

const arr = makeArray(50);
const iter = makeIterable(50);
console.log(dividableWith10(arr)); // [10, 20, 30, 40, 50]
console.log(dividableWith10(iter)); // [10, 20, 30, 40, 50]
```

- 결과는 같지만 N이 커질수록 두 코드의 실행 속도에 큰 차이가 생긴다.
- 결과적으로 `제너레이터`가 `훨씬 빠르게 동작`한다.
- 배열을 사용할 때는 즉시 평가 방식을 사용하기 때문에 1부터 N까지 담긴 전체 배열을 전달하고 그 안의 값을 사용했지만,
- 제너레이터 객체를 사용할 때는 `지연 평가` 방식을 사용하기 때문에 for-of 문을 순회하면서 `필요한 범위 내의 값`만 호출해 사용하기 때문에 I/O 성능을 크게 개선할 수 있다.

#### 성능 차이

1. 배열

```java
console.time('');
console.log(fiveArr(makeArray(50000000)));
console.timeEnd(''); // : 144.364ms
```

2. 제너레이터

```java
console.time('');
console.log(fiveArr(makeIterable(50000000)));
console.timeEnd(''); // : 0.201ms
```
