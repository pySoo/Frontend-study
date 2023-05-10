## 🧑‍💻 JavaScript의 this

이번 글에서는 `this`의 정의와 용법에 대해서 알아보겠다.
자바스크립트의 경우 함수를 호출할 때, `함수가 어떻게 호출되었는지에 따라` this에 바인딩할 객체가 동적으로 결정된다.

함수를 호출하는 방식은 아래와 같이 다양하다.

1. 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. apply/call/bind 호출

### 🏃 1. 함수 호출

- 전역 객체는 모든 객체의 유일한 최상위 객체를 의미하며 일반적으로 Browser-side에서는 `window`, Server-side(Node.js)에서는 `global` 객체를 의미한다.

```js
// in browser console
this === window; // true

// in Terminal
node;
this === global; // true
```

- 글로벌 영역에 선언한 `함수`에서 `this는 전역 객체`에 바인딩된다.
- 즉, this는 선언된 메서드를 가지고 있는 그 상위 오브젝트를 의미한다.

```js
function foo() {
  console.log("foo's this: ", this); // window
  function bar() {
    console.log("bar's this: ", this); // window
  }
  bar();
}
foo();
```

- 또한, foo 안의 bar라는 내부함수도 this는 전역 객체에 바인딩된다.
- 내부 함수에서 this가 전역 객체를 참조하는 것을 회피하는 방법은 글의 마지막에서 다룰 `apply, call, bind` 등의 방법이 있다.
