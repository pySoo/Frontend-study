<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/426525a0-26ee-4dce-bae8-61df6209f9d4/image.png" />

## 클로저란?

MDN에서는 클로저를 다음과 같이 정의하고 있다.

> 클로저는 독립적인 (자유) 변수를 가리키는 함수이다. 또는, 클로저 안에 정의된 함수는 만들어진 환경을 '기억한다'.

이렇게만 들으면 이해하기 어려우니 코드를 통해 이해해보고자 한다.

```javascript
var base = "안녕, ";
function sayHelloTo(name) {
  var text = base + name;
  return function () {
    console.log(text);
  };
}

var hello1 = sayHelloTo("승민");
var hello2 = sayHelloTo("현섭");

hello1(); // '안녕, 승민'
hello2(); // '안녕, 현섭'
```

출력된 결과를 보면 `text` 변수가 동적으로 변화하고 있는 것처럼 보이지만, 실제로는 `text` 변수가 `여러 번 생성된 것`이다.

> 즉 hello1()과 hello2()는 서로 다른 환경을 가지고 있다.

<br>

### 클로저를 이용한 은닉화

```javascript
function Hello(name) {
  this._name = name;
}

Hello.prototype.say = function () {
  console.log("안녕, " + this._name);
};

var hello1 = new Hello("승민");
var hello2 = new Hello("현섭");

hello1.say(); // '안녕, 승민'
hello2.say(); // '안녕, 현섭'

hello1._name = "anonymous";
hello1.say(); // 안녕, anonymous'
```

위에서 Hello()로 생성된 객체들은 모두 `_name`이라는 변수를 가지게 된다. 변수 명에 underscore(\_) 를 포함했기 때문에 private 변수로 쓰고 싶다는 의도를 알 수 있다. 하지만 실제로는 여전히 외부에서도 쉽게 접근 가능한 변수이다.

이 경우, 클로저를 사용하여 외부에서 변수에 직접 접근하는 것을 제한할 수 있다.

```javascript
function Hello(name) {
  var _name = name;
  return function () {
    console.log("안녕," + _name);
  };
}

var hello1 = new Hello("승민");
var hello2 = new Hello("현섭");

hello1(); // '안녕, 승민'
hello2(); // '안녕, 현섭'
```

특별히 인터페이스를 제공하는 것이 아니라면, 여기서는 외부에서 `_name`에 접근할 방법이 전혀 없다. 클로저를 통해 은닉화를 쉽게 해결할 수 있다.

## 클로저의 장점

### 1. 데이터를 보존할 수 있다. (폐쇄성)

- 클로저 함수는 외부 함수의 실행이 끝나더라도 외부 함수 내 변수를 사용할 수 있다.

- 특정 데이터를 스코프 안에 가두어 둔 채로 계속 사용할 수 있는 폐쇄성을 갖는다.

### 2. 캡슐화

- 객체에 담아 여러 함수를 리턴하도록 만들어 정보의 접근 제한을 가능하게 한다.

### 3. 모듈화

- 클로저 함수를 각각의 변수에 할당하면 각자 독립적으로 값을 사용하고 보존할 수 있다.

- 클로저를 통해 데이터와 메서드를 묶어서 다룰 수 있다.
