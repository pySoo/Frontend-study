## 🧑‍💻 JavaScript의 this

이번 글에서는 `this`의 정의와 용법에 대해서 알아보겠다.
자바스크립트의 경우 함수를 호출할 때, `함수가 어떻게 호출되었는지에 따라` this에 바인딩할 객체가 동적으로 결정된다.

함수를 호출하는 방식은 아래와 같이 다양하다.

> 1. 함수 호출
> 2. 메서드 호출
> 3. 생성자 함수 호출
> 4. apply/call/bind 호출

<br />

### 🏃 1. 함수 호출

- 전역 객체는 모든 객체의 유일한 최상위 객체를 의미하며 일반적으로 Browser-side에서는 `window`, Server-side(Node.js)에서는 `global` 객체를 의미한다.

```js
// in browser console
this === window; // true

// in Terminal
node;
this === global; // true
```

- 글로벌 영역에 선언한 `함수`에서 this는 `전역 객체`에 바인딩된다.
- 즉, this는 선언된 메서드를 가지고 있는 그 상위 오브젝트를 의미한다.
- 단, `strict 모드`에서는 this가 `undefined`이다.

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

- 또한, foo 안의 bar라는 `내부함수`도 this는 `전역 객체`에 바인딩된다.
- 내부 함수에서 this가 `전역 객체를 참조하는 것을 회피` 하는 방법은 글의 마지막에서 다룰 `apply, call, bind` 등의 방법이 있다.

<br />

### 🏃 2. 메서드 호출

함수의 객체가 프로퍼티 값이면 메서드로서 호출된다. 이때 메서드 내부의 `this`는 해당 `메서드를 호출한 객체`에 바인딩된다.

```js
var obj1 = {
  name: "Lee",
  sayName: function () {
    console.log(this.name);
  },
};

var obj2 = {
  name: "Kim",
};

obj2.sayName = obj1.sayName;

obj1.sayName(); // obj1
obj2.sayName(); // obj2
```

<br />

### 🏃 3. 생성자 함수 호출

자바스크립트에서는 비슷한 객체를 여러개 사용하길 원할 경우 생성자를 만들어서 사용한다. `생성자`란 `객체를 복사해주는 기계`라고 생각하면 쉽다!

```js
function Constructor() {
  this.name = "pySoo";
}

var obj = new Constructor();

console.log(obj);
```

여기서의 `this`는 `새롭게 복사된 객체`를 의미한다. this.name은 새로 생긴 객체의 name 키값에 "pySoo"라는 값으로 할당해달라는 의미가 된다.

<br />

### 🏃 4. apply/call/bind 호출

this에 바인딩될 객체는 함수 호출 패턴에 의해 결정된다. 이러한 자바스크립트 엔진의 암묵적 this 바인딩 이외에 this를 특정 객체에 `명시적으로 바인딩 하는 방법`도 있다.

그것은 바로 Function.prototype의 apply, call, bind 메서드를 이용하는 방법이다.

#### 1. apply

```js
func.apply(thisArg, [argsArray]);

// thisArg: 함수 내부의 this에 바인딩할 객체
// argsArray: 함수에 전달할 argument의 배열
```

기억해야 할 점은 apply `메서드를 호출하는 주체`는 `함수`이며 apply 메서드는 this를 특정 객체에 바인딩할 뿐 `본질적인 기능`은 `함수 호출`이다.

글로만 읽으면 이해하기 쉽지 않으니 코드를 통해 살펴보자!

```js
const Person = function (name) {
  this.name = name;
};

const obj = {};

// apply() 메서드는 생성자 함수 Person을 호출한다. 이 때 this에 객체 obj를 바인딩한다.
Person.apply(obj, ["pySoo"]);

console.log(obj); // { name: 'pySoo' }
```

apply 메서드를 호출한 후 Person 함수의 `this`는 `obj 객체`가 된다.

Person 함수는 this의 name 프로퍼티에 매개변수 name의 값을 할당하는데, this에 바인딩된 `obj 객체에는 name 프로퍼티가 없으므로` name 프로퍼티가 `동적으로 추가`되고 값이 할당된다.

<br />

#### 2. call

```js
Person.apply(foo, [1, 2, 3]);
Person.call(foo, 1, 2, 3);
```

`call 메서드`의 경우 `apply와 기능은 같지만` apply의 두 번째 인자에서 배열 형태로 넘긴 것을 `각각 하나의 인자로 넘기는 점`이 다르다.

<br />

#### 3. bind

bind는 apply, call 메서드와 달리 `함수를 실행하지 않는다.` 함수에 인자로 전달한 `this`가 바인딩된 `새로운 함수를 리턴`한다.

여기서부터는 코드가 조금 길어지니 천천히 읽어보도록 하자.

```js
function Person(name) {
  this.name = name;
}

Person.prototype.doSomething = function (callback) {
  if (typeof callback == "function") {
    // --------- 1) this = Person 객체
    callback();
  }
};

function foo() {
  console.log(this.name); // --------- 2) this = 전역 객체 window
}

var p = new Person("Lee");
p.doSomething(foo); // undefined
```

이 코드에서의 문제점을 발견하였는가? 1의 시점에서 this는 Person 객체이지만 2의 시점에서 this는 전역 객체 window를 가리킨다🤯 1번 항목에서 다뤘던 함수에서는 this가 전역 객체에 바인딩 되는 경우다.

따라서, `콜백 함수 내부의 this(1)`를 `콜백함수를 호출하는 함수 내부의 this(2)`와 `일치`시켜 주어야 한다.

<br />

bind 메서드를 이용하면 해결이 가능하다!

```js
function Person(name) {
  this.name = name;
}

Person.prototype.doSomething = function (callback) {
  if (typeof callback == "function") {
    // callback.call(this);와 동일한 코드이다.
    callback.bind(this)();
  }
};

function foo() {
  console.log(this.name);
}

var p = new Person("Lee");
p.doSomething(foo); // 'Lee'
```

bind 메서드에서 함수에 인자로 전달한 this가 바인딩된 새로운 함수를 리턴한다. 쉽게 얘기하자면 `bind`는 `this를 수정할 수 있게` 해주는 메서드이며, this가 `Person`으로 `고정된 function이 할당`된다.

<br />

### 🏃 5. 추가 자료: 이벤트 리스너

실무에서는 이벤트 리스너 또는 제이쿼리를 썼을 때의 this 용법이 헷갈리기 때문에 문제가 되는 경우가 있다.

this의 개념에 대해서 알아보았으니 이벤트 리스너와 제이쿼리의 사례까지 꼼꼼하게 잡아보고 가도록 하자!

#### 1. 이벤트 리스너

```js
document.body.onclick = function () {
  console.log(this); // <body>
};
```

앞서 1번 사례에서 함수의 경우 전역 객체인 window에 바인딩 된다고 배웠는데, 이 경우 body에 바인딩이 되었다.

`이벤트가 발생`하는 경우, 내부적으로 `this`가 이벤트의 `currentTarget`과 일치하게 된다. (주의❗)

#### 2. 제이쿼리

```js
$("div").on("click", function () {
  console.log(this); // <div>
  function inner() {
    console.log("inner", this); // inner window
  }
});
```

사람들을 미치게 하는 응용사례이다. 앞서 클릭 이벤트에서 내부적으로 this를 currentTarget으로 바꾼다고 배웠다. 하지만 `inner 함수` 호출 시에는 `this가 window`가 된다.

사실 inner는 죄가 없다. 함수의 this는 기본적으로 전역 객체 window라는 원칙을 충실히 따랐을 뿐이다. 잘못이 있다면 `이벤트 리스너`가 `내부적으로 this를 바꿨음`에도 명시적으로 `알리지 않은 것`이 잘못이라고 할 수 있다.

<br />

#### 해결1. this를 변수에 저장

```js
$("div").on("click", function () {
  console.log(this); // <div>
  const that = this;
  function inner() {
    console.log("inner", that); // inner <div>
  }
});
```

위의 문제를 해결하기 위해서는 this를 변수에 저장해서 사용하는 방법 또는,

#### 해결2. 화살표 함수

```js
$("div").on("click", function () {
  console.log(this); // <div>
  const inner = () => {
    console.log("inner", this); // inner <div>
  };
});
```

ES6 화살표 함수를 쓰는 방법이 있다. `화살표 함수`는 this에 window 대신 `상위 함수의 this를 가져온다.`

<br />

### 글을 마치며

- `this는 기본적으로 window`이지만 객체 메서드 호출, 생성자 함수 호출, 또는 apply, call, bind를 사용할 때 `this가 변경`된다.

- `이벤트 리스너`의 경우 `this를 내부적으로 바꿀 수`도 있으니 항상 this를 확인해보는 것이 중요하다.
