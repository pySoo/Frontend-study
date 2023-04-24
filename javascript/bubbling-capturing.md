## 이벤트 버블링 (Event Bubbling)

- `이벤트 버블링`이란, 하위 엘리먼트에서 이벤트가 발생했을 때 그 엘리먼트로부터 시작해서 상위 요소까지 이벤트가 전달되는 방식을 말한다.
- 부연 설명을 하자면, 한 요소에 이벤트가 발생하면 이 요소에 등록된 핸들러가 동작하고, 이어서 부모 요소의 핸들러가 동작한다. 가장 최상단의 조상 요소를 만날 때까지 위 과정이 반복된다.

코드를 통해 더 쉽게 이해해보도록 하자.

```html
<form onclick="alert('form')">
  <div onclick="alert('div')">
    <p onclick="alert('p')" />
  </div>
</form>
```

- 가장 안쪽의 `<p>`를 클릭하면 순서대로 다음과 같은 일이 벌어진다.

1. `<p>`에 할당된 onclick 핸들러가 동작
2. 바로 바깥의 `<div>`에 할당된 onclick 핸들러 동작
3. 그 바깥의 `<form>`에 할당된 핸들러가 동작
4. document 객체를 만날 때까지 각 요소에 할당된 onclick 핸들러가 동작한다.

<img width="300px" src="https://velog.velcdn.com/images/soopy368/post/a392c3dc-cdb0-4e0c-8e0d-805f3f79c53c/image.png" />

<br>

## 이벤트 캡쳐링 (Event Capturing)

- `이벤트 캡쳐링`이란, 하위 엘리먼트에 이벤트 핸들러가 있을 때 상위 엘리먼트로부터 이벤트가 발생하기 시작해서 하위 엘리먼트까지 이벤트가 전달되는 방식이다.
- 실제 코드에서 기본 동작은 `이벤트 버블링`이고 이벤트 캡쳐링은 자주 쓰이진 않는다. 혹시나 이벤트 캡쳐링 방식으로 코드를 실행시키려면 `addEventListener()`의 마지막 인자로 `{ capture:true }`를 전달해주면 된다.

```javascript
document.querySelector("ul").addEventListener(
  "click",
  () => {
    console.log("클릭");
  },
  { capture: true }
);
```

## event.stopPropagation()

> 난 그냥 원하는 화면 요소의 이벤트만 신경 쓰고 싶어요

이럴 땐 어떻게 하면 좋을까? `event.stopPropagation()` 웹 API를 사용하면 된다.
위 API는 해당 이벤트가 전파되는 것을 막는다.
이벤트 버블링의 경우 클릭한 요소의 이벤트만 발생시키고 상위 요소로 이벤트를 전달하는 것을 방해한다. 이벤트 캡쳐의 경우도 마찬가지로 하위 요소들로 이벤트를 전달하지 않는다.

> 하지만, 계층 구조가 깊은 곳에서 쓰게 된다면 원하는대로 동작하지 않을 때 `어디서 문제가 발생했는지 찾기 어려울 수 있다`. 그렇다면 어떻게 구현하는게 좋을까?

<br>

## 이벤트 위임

- `이벤트 위임`은 하위 요소에 각각 이벤트를 붙이지 않고 상위 요소에서 하위 요소의 이벤트들을 제어하는 방식이다.
- 이벤트 버블링와 캡쳐링은 이벤트 위임을 위한 선수지식이고, 이벤트 위임은 실제로 JS로 웹을 구현할 때 자주 쓰이는 패턴이다.
- 공통 조상에 할당한 핸들러에서 `event.target`을 이용하면 실제 어디서 이벤트가 발생했는지 알 수 있다.

```html
<div onclick="alert('div')">
  <button>첫 번째</button>
  <button>두 번째</button>
  <button>세 번째</button>
</div>
```

### button에 모두 이벤트 리스너 적용하기

지양 🙅: button의 개수가 매우 많아지면 메모리에도 이벤트 핸들러가 쌓이기 때문에 성능에 문제가 생길 수 있다.

```javascript
const buttons = document.querySelectorAll("button");
buttons.forEach(function (button) {
  button.addEventListener("click", function (event) {
    alert("clicked");
  });
});
```

이벤트 위임: 가장 상위 요소인 div 리스너를 등록하고 하위에서 발생한 이벤트를 감지한다.

```javascript
const div = document.querySelector("div");
div.addEventListener("click", function (event) {
  alert("clicked");
});
```

이벤트 위임 👍: 만약 div를 클릭했을 때가 아니라 button만 클릭했을 때 동작시키고 싶다면? `target`을 사용한다.

```javascript
const div = document.querySelector("div");
const button = document.querySelector("button");
div.addEventListener("click", function (event) {
  // event의 target이 현재 클릭한 target이 아니라면 동작시키지 않는다!
  if (event.target !== event.currentTarget) {
    return;
  }
  alert("clicked");
});

button.addEventListener("click", function (event) {
  alert("clicked");
});
```

### 이벤트 위임으로 얻는 장점

1. 동적으로 엘리먼트를 추가할 때마다 핸들러를 고려할 필요가 없다.
2. 상위 엘리먼트에 하나의 이벤트 핸들러만 추가하면 되기 때문에 코드 가독성이 좋아진ㄴ다.
3. 메모리에 있는 이벤트 핸들러가 적어지기 때문에 퍼포먼스 측면에 이점이 있다.

<br>

## 글을 마치며

이번 글에서는 브라우저가 이벤트를 감지하는 방식과 우리는 이벤트를 어떻게 다뤄야 하는지에 대해 알아보았다. 아래의 이벤트 흐름 3가지를 통해 오늘의 내용을 복습해보자.

### DOM 이벤트 흐름 3가지 단계

1. 캡쳐링 단계 - 이벤트가 하위 요소로 전파되는 단계
2. 타깃 단계 - 이벤트가 실제 타깃 요소에 전달되는 단계
3. 버블링 단계 - 이벤트가 상위 요소로 전파되는 단계

<img width="80%" src="https://velog.velcdn.com/images/soopy368/post/d48467ae-6f2c-469e-9081-32edaa1af1bc/image.png" />
