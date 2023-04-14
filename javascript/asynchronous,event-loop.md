<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/423784e4-9949-46f1-b43c-487f90067bd4/image.png" />

자바스크립트에서 가장 중요하지만 이해하기 어려운 부분이 비동기 동작이다. 이번 글에서는 자바스크립트가 어떻게 비동기 기반으로 동작하고 그 원리는 무엇인지에 대해 알아보도록 하겠다.

## JavaScript는 머리가 하나다 🧠

> 자바스크립트는 싱글 스레드 언어이다.

지난 포스팅 내용을 복습해보겠다. 자바스크립트는 `한 번에 한 가지 일`밖에 처리할 수 없다. 어려운 용어로는 `Call stack이 하나`라고 표현한다. 현재 실행되고 있는 함수가 있는 경우에 다른 일을 할 수가 없고, 다른 일들을 블락하게 된다.

예를 들어, 크롬에서 alert 창을 띄워보면 alert 창을 닫기 전까지는 아무것도 할 수 없다.

그렇다면 1분 이상 시간이 소요되는 오래 걸리는 작업이 있다면 그저 기다리기만 해야할까..? 아니다. 우리에게는 `비동기 콜백`이 있다.

비동기 콜백을 이해하기 위해서는 `JS Engine`과 `Web API`, `Event Loop`에 대한 이해가 먼저 필요하다.

## 자바스크립트를 실행시켜줘 (JS Engine)

자바스크립트 엔진은 JS 코드를 이해하고 실행을 도와주는 역할을 한다. 대표적으로 크롬에서는 V8이라는 엔진으로 JS를 실행한다. 자바스크립트 엔진은 크게 `Memory Heap`과 `Call Stack`으로 이루어져 있다.

### Memory Heap

데이터를 임시 저장하는 것으로, 주로 함수나 변수 또는 함수를 실행할 때 사용하는 값들을 저장한다.

### Call Stack

코드의 실행 순서를 기록해놓고 순차적으로 실행할 수 있도록 도와주는 것이다.

만약 어떠한 코드를 실행하는 시간이 오래 걸리게 된다면 아래에 있는 다른 코드들을 실행하지 못하게 되어서 `block` 상태가 될 것이다.

우리는 가만히 앉아 실행을 기다릴 만큼 인내심이 좋지 않다. 😅

<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/2b53ea20-7f10-474a-80d1-4220e7e9e1cb/image.png" />

- 이 때 효과적으로 event를 관리하기 위해 필요한 것이 바로 `web API`와 `Callback Queue` 그리고 `Event Loop`이다.
- 이제부터 치킨을 만드는 과정을 이용해 JS 코드가 비동기를 처리하는 방법에 대해 배워보겠다.

<br>

## 재료 손질이 끝나고 할 일은 Callback 함수에게 전해줘

치킨을 만들 때, 재료 손질이 끝났다면 재료를 구울 수도 있고 재료를 튀길 수도 있다. 이처럼 하나의 일이 끝나면 다음에 할 일이 달라질 수 있다.

자바스크립트에서는 함수 실행이 끝나면, 다음에 할 일을 정할 수 있는데 이것을 `Callback`이라고 부른다.

자고로 치킨이란 바삭바삭한 맛에 먹는게 제맛이기 때문에 재료 손질 후에 튀겨달라고 요청할 것이다.

```javascript
setTimeout(function () {
  console.log("치킨을 튀겨주세요");
}, 1000);
```

위 코드에서 setTimeout 함수는 첫 번째 인자로 `callback function`을 받고, 두 번째 인자로 기다릴 시간을 받는 함수이다.

코드를 실행하면 1초 뒤에 '치킨을 튀겨주세요'라는 내용이 console에 출력된다.

즉, `callback function`은 함수가 정해놓은 일이 끝나면 할 일을 가지고 있는 함수이다. 서버 통신에서 쓰이는 ajax나 DOM을 관리하는 event 등에서 주로 사용되기 때문에 JS를 효과적으로 사용하기 위해서 꼭 알아야 하는 개념이다.

<br>

## 오래 걸리는 일은 따로 처리할래 (web APIs)

브라우저의 `web API`는 브라우저 안에서 C++로 구현된 스레드로 주로 DOM event, Ajax request, setTimeout 등 `비동기 이벤트를 처리`한다. 장점은 자바스크립트의 `싱글 스레드의 영향을 받지 않고 독립적으로 이벤트를 처리`할 수 있다는 것이다.

치킨을 튀기는 것은 오래 걸리는 과정이기 때문에 web API에게 맡겨놓자.

<br>

## 머리 속에 다음에 할 일 생각해놓기 (Callback Queue)

사람은 어떠한 일이 끝나면 다음에 할 일을 머리 속에 저장해두지만, 브라우저의 경우 Callback Queue에 저장해놓고 사용한다.

즉, Callback Queue는 `web API`에 있는 `event가 실행이 되고나면` javascript에서 `실행할 callback을 저장`하고 있는 저장소이다.

우리는 web API에게 치킨을 튀겨달라고만 말했지, 튀긴 후에 무엇을 해달라고는 말하지 않았다. 오늘은 양념 치킨이 땡기기 때문에 튀긴 후에 소스를 발라달라고 부탁해보자🤤

```javascript
setTimeout(function () {
  console.log("소스를 발라주세요");
}, 80000);
```

위 코드를 실행하게 되면 8분이 지난 후 Callback Queue에 `function(){console.log('소스를 발라주세요')}` 함수가 담기게 된다.

그렇다면 튀긴 후에 Callback Queue를 확인하여 까먹지 않고 소스를 바르는 것을 수행할 수 있다!

<br>

## 할 일 정돈하기 (Event Loop)

능률적으로 일하려는 사람은 우선순위를 정해서 행동한다. 마찬가지로 JS 내에서도 일을 효과적으로 처리하기 위한 메커니즘이 있는데, 이를 바로 `Event loop`라고 부른다.

우리는 치킨을 튀겨달라고 요청한 뒤, 8분 후에 소스를 발라달라고 할 것이다. 8분 동안 마냥 기다리는 것보단 포장을 준비하는게 효율적이니 포장도 준비해달라고 하겠다.

```javascript
console.log("치킨을 튀겨주세요");
setTimeout(function () {
  console.log("소스를 발라주세요");
}, 80000);
console.log("포장을 준비해주세요");
```

<br>

> 위 코드가 실행되는 과정을 알아보면서 앞서 배운 개념들이 어떠한 역할을 하는지 배워보겠다.

### 1) 모든 코드를 Call Stack에 담는다.

<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/f02a3909-ae2a-49e7-8e24-c0aba4d66881/image.png" />

### 2) Call Stack에 있는 코드를 순차적으로 실행한다.

console.log("치킨을 튀겨주세요")가 실행되었다.
<br>

<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/9b783979-019b-4444-a664-2570360dec58/image.png" />

### 3) 오래 걸리는 작업은 Web API에게 맡긴다.

비동기 작업인 setTimeout()이 web API에게 맡겨졌다. 편의상 함수는 timeout()으로 줄여서 표기하였다.

```javascript
timeout() = setTimeout(function(){console.log("소스를 발라주세요")})
```

<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/290add83-2716-4780-8593-38666a5a8991/image.png" />

### 4) Call Stack에 있는 작업을 수행한다.

8분 동안 기다릴 수는 없으니 Call Stack에 있는 console.log("포장을 준비해주세요") 작업을 먼저 실행했다.
<br>

<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/ebe4df37-5946-48fa-8376-6f83835eeffb/image.png" />

### 5) 8분 후 setTimeout() 작업이 끝났다.

Callback Queue에 다음에 할 작업인 Callback Function이 저장되었다.
<br>

<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/9a16b00f-80a4-4f9e-9ff5-861ba35c018e/image.png" />

### 6) Event Loop가 Stack이 비었는지 확인한다.

Stack이 비어있다면 지금 할 작업이 없는 상태이므로 Queue에 있는 코드를 Stack에 넣을 수 있다.
<br>

<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/9a16b00f-80a4-4f9e-9ff5-861ba35c018e/image.png" />

### 7) Queue에 있던 코드를 Stack에 넣고 실행한다.

<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/3a97627e-cd68-410b-a6d1-83e3cc9014a3/image.png" />

<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/ccb16cd3-79b3-4124-8aad-cd81749b6f30/image.png" />

> 위 과정으로 Call Stack, Web API, Callback Queue, Event Loop가 어떠한 역할을 하는지 배울 수 있엇다.

`Event loop`는 `Call stack`이 비어있는지를 주기적으로 확인한다. 반복적으로 `Call stack`이 비어있는지 확인하는 것을 `tick`이라고 한다. Call stack이 비어있다면, `Callback queue`에서 `Callback function`을 가져와 stack에 올린다.

요약하자면 Event loop는 Call stack이 다 비워지면 Callback queue에 존재하는 함수를 하나씩 Call stack으로 옮기는 역할을 한다.

## 글을 마치며

자바스크립트 환경에서 비동기 이벤트가 어떻게 동작하는지 알아보았다.

> - 자바 스크립트는 단일 스레드 기반의 언어로 한 번에 하나의 작업만을 처리할 수 있다.
> - 하지만 비동기로 동작하기 때문에 단일 스레드임에도 불구하고 동시에 많은 작업을 수행할 수 있다.
> - 비동기로 동작하는 핵심 요소는 자바스크립트가 아닌 브라우저가 가지고 있다. (Call Stack, Web API, Callback Queue, Event Loop)
