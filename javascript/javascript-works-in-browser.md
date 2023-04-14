<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/423784e4-9949-46f1-b43c-487f90067bd4/image.png" />
자바스크립트의 특징들에 대해 배워보기 전에, 자바스크립트가 어떻게 동작하는지에 대해 간단하게 다뤄보고자 한다.

## JavaScript는 머리가 하나다 🧠

> 자바스크립트는 싱글 스레드 언어이다.

이 의미를 쉽게 풀어쓰면 자바스크립트는 `한 번에 한 가지 일`밖에 처리할 수 없다. 어려운 용어로는 `Call stack이 하나`라고 표현한다. 현재 실행되고 있는 함수가 있는 경우에 다른 일을 할 수가 없고, 다른 일들을 블락하게 된다.

예를 들어, 크롬에서 alert 창을 띄워보면 alert 창을 닫기 전까지는 아무것도 할 수 없다.

지금부터 브라우저에서 JavaScript 코드가 어떻게 동작하는지 그 과정을 살펴보겠다.

## 브라우저가 JS 코드를 실행한다.

브라우저는 `JS 코드를 실행하는 엔진`이라고 생각해두자. 대표적으로 크롬에서는 V8이라는 엔진으로 JS를 실행한다. 코드를 실행하는데는 `Heap`, `Stack`, `Queue`, `대기실`이라는 개념이 필요하다.

<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/729796cd-a1a3-4a80-a8a8-7084ded35d31/image.png" />

아래의 코드가 어떠한 순서로 동작할 지 예상해보자.

```javascript
console.log(1);
setTimeout(function () {
  console.log(2);
}, 0);
console.log(3);
```

결과

```javascript
1;
3;
2;
```

> #### setTimeout 안의 속도가 0인데 어째서 console.log(3)보다 늦게 출력 되었을까? (충격..)

## 코드 한 줄씩 모두 Stack으로

<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/c752bd64-460f-4d72-9854-8c9af2b43657/image.png" />

브라우저는 코드 한 줄 한 줄씩 Stack에 적재한다. 그리고 Stack에 있는 코드를 순차적으로 실행한다.

(참고로 이번 글에서는 Heap에 대해 다루지 않을 것이다. Heap이란 변수 등을 저장해두는 공간이다.)

<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/dcab407c-a273-44da-b796-7bf2937ecaaa/image.png" />

가장 상단에 있던 console.log(1)을 실행했다.

## 나중에 처리할 코드는 대기실로

<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/dcab407c-a273-44da-b796-7bf2937ecaaa/image.png" />

브라우저는 stack에 있는 코드를 실행하면서 나중에 처리해야하는 코드는 대기실로 보낸다. 대기실에 보내는 코드들은 대표적으로 서버와 통신하는 Ajax 요청 코드, 이벤트 리스너, setTimeout 등이 있다.

따라서 브라우저는 setTimeout이 포함된 코드를 대기실로 보낸다.

<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/32630bdf-bf83-4c31-9b29-5190a7ff9f31/image.png" />

## 대기실에 있는 일 중 실행할 수 있는 일은 Queue로 보낸다.

<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/6007fcc7-bbac-4a59-afc5-1b8a03ce159d/image.png" />

0초 뒤에 setTimeout 함수에 들어있던 console.log(2)를 실행할 수 있으므로 `console.log(2)`를 Queue로 보낸다.

<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/f1523c79-b4c0-4d10-a62d-b065e0a55d9f/image.png" />

## Stack에 있는 코드를 순차적으로 실행한다.

<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/92f21e5e-8f56-447a-9752-9ab4705159fd/image.png" />

Stack과 Queue 중에 무엇은 먼저 실행해야 하냐고 묻는다면 ~~(대답해주는게 인지상정!🚀)~~

Queue는 Stack에 가기 전 대기 중인 공간이기 때문에 Stack에 있는 코드를 먼저 실행한다. Queue는 Stack이 비어있을 때만 Stack으로 내용을 올려보내는 역할을 한다.

## Stack이 비어있다면 Queue에 있는 내용을 Stack으로 보낸다.

<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/2fd12eec-2208-4ede-985c-853cfb991de4/image.png" />

Queue는 Stack이 비어있을 때만 Stack으로 내용을 올려보낸다. 이제 Stack에 들어온 코드를 차례대로 실행한다.

## 실행 과정 요약

```javascript
console.log(1);
setTimeout(function () {
  console.log(2);
}, 0);
console.log(3);
```

과정

```javascript
1. 코드 한 줄씩 call stack에 적재
2. call stack에 있는 코드들 순차적으로 실행
3. Stack에 있는 `console.log(1)` 실행
3. 나중에 처리해야하는 setTimeout(function(){console.log(2)},0); 코드는 대기실로 보낸다.
4. 0초 뒤에 console.log(2)를 Queue로 적재
5. Stack에 있는 `console.log(3)` 실행
(Queue에 있는 내용은 Stack이 비워진 후에 실행되므로)
6. Queue에 있는 console.log(2)를 스택에 적재
7. Stack에 있는 `console.log(2)` 실행
```

> setTimeout으로 둘러싸인 코드는 대기실로 보내지고, 큐를 거쳐서 스택으로 이동되기 때문에 console.log(3)보다 늦게 실행된다.

<br>

## 그렇다면 JS 코드를 어떻게 짜는게 좋을까?

> 1.  Stack을 바쁘게 하지 말자. 🤯
> 2.  Queue를 바쁘게 하지 말자. 🤯

### Why?

### 1. 오래 걸리는 연산을 하게 되면 브라우저에서 다른 동작을 하지 못한다.

만약, 버튼에 이벤트 리스너를 등록했고 10초 이상 걸리는 어려운 연산을 Stack에서 실행 중이라면, 아무리 버튼을 눌러도 동작하지 않을 것이다. Queue에 있는 코드는 Stack에 있는 코드가 실행된 후에 실행되기 때문이다.

### 2. 이벤트 리스너를 너무 많이 등록하지 말자.

이벤트 리스너에 등록된 코드들은 모두 큐에 적재된다. 큐에 있는 코드가 많아진다면 Stack으로 이동하는 과정도 한번 더 거쳐야하고 결국 Stack에도 많은 코드가 적재되기 때문에 과부화가 올 수 있다.

## 글을 마치며

브라우저에서 JS가 어떻게 동작하는지 알게 되었더니, `앞으로 코드를 어떻게 짜면 좋을지에 대한 인사이트`를 얻을 수 있었다. 이번 글에서는 기초 원리에 대해서 배워보았고 앞으로의 글에서는 비동기 처리 방법, Event Loop에 대한 내용도 다뤄보고자 한다. 글에서 다뤘던 `대기실`이라는 추상적인 개념이 무엇인지 궁금하다면 다음 포스팅을 읽어보자.
