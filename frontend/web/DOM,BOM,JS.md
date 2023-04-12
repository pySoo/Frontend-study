![](https://velog.velcdn.com/images/soopy368/post/7d285e93-3618-4921-a659-30cae6d40efd/image.png)

## Window는 DOM, BOM, JS의 조상이다 👴

프론트 개발을 하다보면 만날 수 밖에 없는 이 개념들! 기본 중 기본이라고 할 수 있다.
프론트 최적화에서 꼭 필요한 개념인 성능 보장 렌더링 순서(CRP) 단계에서도 등장하니 지금부터 DOM, BOM, JS의 개념에 대해서 잘 알아두자.

## 1. Window

> - 자바스크립트의 최상위 객체 / 전역 객체 / 모든 객체가 소속된 객체

- **브라우저를 대변하고 이를 제어할 수 있는 메서드를 제공**한다.

![](https://velog.velcdn.com/images/soopy368/post/b49c3b17-b668-4f97-aa65-0ed25d55bf14/image.png)

개발자 모드 console 창에서 this를 출력하면 window 객체가 출력되는 것으로 window가 최상위 객체임을 확인할 수 있다.

흥미로운 점은 this=window이기 때문에 window.clearInterval에서 window를 생략해도 동일하게 동작한다!

window가 JS의 최상위 객체인 것을 확인했으니,
지금부터 window의 하위 객체인 DOM, BOM, JS에 대해서 알아보자.

## DOM(Document Object Model)

![](https://velog.velcdn.com/images/soopy368/post/77f12ba8-8d42-47e0-9972-f330224ce96c/image.png)

> - 문서 객체 모델(DOM): 객제 지향 모델로써 구조화된 문서를 표현하는 방식이다.

- **웹 문서를 브라우저가 이해할 수 있는 구조로 구성한 것**

**브라우저는 HTML의 tag들을 이해할 수 없기 때문에** 브라우저가 이해할 수 있는 방식인 **DOM tree 형태로 변경한다.**

---

![](https://velog.velcdn.com/images/soopy368/post/b9bfe36b-9d37-468c-92c8-6d7670a50673/image.png)

#### DOM Tree의 객체 구성

타입스크립트를 써보았다면 input 태그를 다룰 때 타입을 무엇으로 설정해야 하는지 검색해본 적이 있을 것이다. 그 때 쓰인 HTMLInputElement를 위 트리에서 확인할 수 있다! 사실 저 객체는 JS Node->Element->HTMLElement->HTMLInputElement인 셈이다.

HTML의 Tag는 브라우저가 이해할 수 있는 형태인 JS Node로 변환하기 때문에 부모가 Node이다.

## BOM(Browser Object Model)

![](https://velog.velcdn.com/images/soopy368/post/4929d568-a523-4e60-8471-727012e483b1/image.jpeg)

> - 브라우저 객체 모델(BOM): JS가 브라우저와 소통하기 위해 만들어진 모델

- JS를 이용하면 브라우저의 정보에 접근하거나, 브라우저의 여러 기능들을 제어할 수 있다. 이 때 사용하는 객체 모델이 BOM이다.

#### BOM 요소에 대한 설명

<table>
<tbody>
<tr>
<td>navigator</td>
<td>브라우저와 버전 정보를 속성으로 가짐</td>
</tr>
<tr>
<td>location</td>
<td>현재 url에 대한 정보, 브라우저에서 사용자가 요청하는 url</td>
</tr>
<td>document</td>
<td>현재 문서에 대한 정보</td>
</tr>
<tr>
<td>screen</td>
<td>브라우저의 외부 환경에 대한 정보를 제공</td>
</tr>
<tr>
<td>history</td>
<td>현재의 브라우저가 접근했던 url history</td>
</tr>

</tbody>
</table>

<br>

> ❓ 여기서 드는 의문 하나..
> document는 DOM과 BOM에 모두 나오는데 둘 다 포함인 건가요? <br>
> 💡 YES!
> DOM은 BOM의 일부이지만 매우 중요하기 때문에 따로 나누어서 본다.
> DOM은 부모 요소가 Document인 트리 구조로 이루어져있고 BOM과 달리 DOM은 W3C의 표준 객체 모델이다.

## JavaScript

> DOM, BOM 요소를 조작할 수 있다.

### DOM 조작

```javascript
// class명이 target인 요소 노드를 선택한다.
const taget = document.querySelector(".target");

// target 노드의 className을 'non-target'으로 변경한다.
target.className = "non-target";
```

### BOM 조작

```javascript
// 현재 페이지 새로고침
window.location.reload(true);

// 크롬 환경인지 아닌지 확인하기
const userAgent = window.navigator.userAgent.toLowerCase();
if (userAgent.indexOf("chrome") > -1) {
  console.log("크롬이다!");
} else {
  console.log("크롬이 아니다");
}
```
