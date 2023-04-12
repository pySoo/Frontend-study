## Critical Rendering Path

브라우저가 서버로부터 HTML 응답을 받아서 화면을 그리기 위해 실행하는 과정이다.
이번 글에서는 브라우저가 웹페이지를 어떻게 렌더링 하는지에 대해서 알아본다.

이 과정을 이해하고 각 단계에서 어떤 일이 일어나는지 파악할 수 있어야 브라우저 렌더링 최적화를 통해 어플리케이션의 응답 속도를 개선할 수 있다.

### CRP의 단계

![](https://velog.velcdn.com/images/soopy368/post/fa0cef8f-d6d9-4502-9a2e-46225cd7067e/image.jpeg)

> ### 요약

1. DOM 트리 만들기
2. CSSOM 트리 만들기
3. JavaScript 실행
4. Render Tree 만들기
5. Layout 생성
6. Paint

<br>

## 1. DOM 트리 만들기

![](https://velog.velcdn.com/images/soopy368/post/f6b8c3bf-70c0-4e31-9cab-177bdf69b4a0/image.png)

브라우저 주소창에 url을 입력하여 요청을 보내면 서버로부터 HTML 문서를 받아오게 된다. 브라우저는 HTML을 파싱하여 DOM 노드들을 만든다. 그리곤 DOM 노드들을 병합하여 DOM 트리를 만든다.

<br>

## 2. CSSOM 트리 만들기

![](https://velog.velcdn.com/images/soopy368/post/e1742704-1160-48a2-8121-613560d35fc9/image.png)

HTML을 파싱하다가 CSS 링크를 만나면 CSS 파일을 요청해서 받아오게 된다. CSS 파일은 HTML을 파싱한 것과 유사한 과정을 거쳐서 역시 트리 형태의 CSSOM으로 만들어진다.

특징:

1. 자식 노드들이 부모 노드들의 특성을 상속받는다. (cascading rule)
2. CSSOM까지 구성하는 것이 끝나야 Rendering을 시작할 수 있기 때문에, CSS를 렌더링 차단 리소스라고 부른다.

<br>

## 3. JavaScript 실행

JS 파일을 만나면 해당 파일을 받아와서 실행할 때까지 파싱이 멈춘다. 따라서, 문서 내의 요소(ex. DOM)를 참조하는 JS 파일이 있는 경우 해당 문서가 표시된 후에 배치해야 한다.

```javascript
<script async src="script.js">
```

JS가 파서를 차단하는 것을 피하기 위해 async나, window.load()를 사용할 수도 있다.

- window.load(): document, css, image 등을 모두 다운로드한 후에 실행됨

<br>

## 4. Render Tree 만들기

![](https://velog.velcdn.com/images/soopy368/post/2c7ee171-c733-4bf6-9a73-3aa57a693b0e/image.png)

컨텐츠를 설명하는 DOM과 스타일 규칙을 표현하는 CSSOM은 독립적인 객체이기 때문에 DOM과 CSSOM을 병합하여 Render Tree에 결합한다. Render Tree는 페이지에 표시되는 모든 DOM 컨텐츠와 각 노드에 대한 모든 CSSOM 스타일 정보를 가진다.

특징: css에 의해 화면에서 숨겨지는 노드들은 반영되지 않는다. 예를 들어 display:none.

<br>

## 5. Layout 생성

지금까지 표시할 노드와 노드의 스타일에 대해서는 계산했지만, 노드의 정확한 위치나 크기는 계산하지 않았다. 이를 계산하는 것이 레이아웃 단계이다. 이를 리플로우(reflow)라고 부르기도 한다.

레이아웃 과정에서는 뷰포트 내에서 각 요소의 정확한 위치와 크기를 계산하여 화면에 출력한다.

<br>

## 6. 페인팅

마지막으로, 렌더링 트리의 각 노드를 화면의 실제 픽셀로 변환한다. 텍스트, 색, 이미지, 그림자 등 요소의 모든 시각적 부분을 그리는 작업을 포함한다. Painting 과정 후 브라우저 화면에 UI가 나타나게 된다.

<br>

## 종합

![](https://velog.velcdn.com/images/soopy368/post/570b4373-a740-4010-a338-80a30e5e9873/image.png)

페이지가 로드 되었을 때의 네트워크 동작 순서이다. 앞서 언급한 순서와 다른 점은 1번이 브라우저의 HTML 요청으로 표기된 것과 Render Tree를 만드는 순서가 없는 것이다. 그렇지만 전체 흐름을 파악하기에 좋은 자료이다!

---

지금까지 HTML 파일을 우리가 보는 화면으로 그리기까지 브라우저가 어떠한 렌더링 과정을 거치는지 배워보았다. 중요한 것은, 이 과정을 이해하고 각 과정에서 일어나는 불필요한 작업을 제거해서 최적화를 하는 것이다.

복잡한 과정이지만 여기까지 잘 따라왔다면 다음 과정은 더 잘 이해할 수 있을 것이다! 👏👏👏👍
혹시 DOM에 대해서 더 알고 싶다면 [이 글을 읽어보는 것을 추천](./DOM,BOM,JS.md)한다.

다음 챕터에서는 브라우저 렌더링 최적화 방법에 대해 배워보도록 하겠다 😁

---

📄 references

- https://bitsofco.de/understanding-the-critical-rendering-path/
