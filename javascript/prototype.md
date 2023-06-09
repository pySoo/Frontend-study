# 프로토타입

- 프로토타입은 자바스크립트에서 `상속`을 지원하기 위한 `유전자` 역할을 한다고 생각하면 이해가 쉬워진다.
- 자바스크립트의 모든 객체는 프로토타입이라는 객체를 가지고 있다.
- 자바스크립트의 모든 객체는 프로토타입으로부터 프로퍼티와 메서드를 `상속`받는다.
- 즉, 프로토타입에 추가한 속성은 자식들도 사용할 수 있는 것이다.

### 프로토타입 체인(Prototype chain)

- new 연산자를 사용해서 만든 객체는 생성자의 프로토타입을 자신의 프로토타입으로 상속받는다.
- 자식이 갖고 있지 않은 속성이 있다면 부모의 프로토타입을 참조하고, 또 없다면 부모의 부모를 참조하는 `재귀` 과정을 프로토타입 체인이라고 부른다.

```jsx
function 기계() {
  this.price = 0;
  this.numer = 123;
}

기계.prototype.name = "익명";

const 냉장고 = new 기계();

console.log(냉장고); // { price: 0, number: 123 }
console.log(냉장고.name); // 익명
```

- 냉장고(자식)를 출력해보면 price와 number만 가지고 있지만, 부모의 prototype에서 명시한 name 속성까지 접근할 수 있다.
- 이는 자식이 `부모의 프로토타입을 물려받았기 때문`이다. 부모의 유전자를 물려받듯이 name이라는 속성을 물려받은 것이다.

<br />

## 프로토타입을 왜 사용하나요?

#### 1. 메모리 절약

- 생성자 함수를 통해 새로운 객체가 생성되면 프로퍼티와 메서드가 매번 생성되어 메모리 낭비로 이어진다.
- 프로토타입을 사용하면 부모 객체 생성시에 `한 번만 생성`하게 되어 메모리를 절약할 수 있다.

#### 2. 상속을 통해 하위 객체들이 부모 객체의 프로퍼티를 사용할 수 있다.

- 자식은 해당 프로퍼티를 가지고 있지 않아도 상속을 통해 부모의 프로퍼티를 사용할 수 있다.

#### 3. 생성자 함수의 프로퍼티나 메서드를 외부에서 수정할 수 있다.

#### 4. 중복된 코드를 줄이고 재사용성을 높일 수 있다.
