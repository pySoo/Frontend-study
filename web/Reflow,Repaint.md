## 브라우저 렌더링 최적화

![](https://velog.velcdn.com/images/soopy368/post/c27e89d4-9903-4fd5-8809-68f44d483b2b/image.png)

[이전 글](https://velog.io/@soopy368/Web-Critical-Rendering-Path)에서 CRP(Critical Rendering Path)에 대해서 배워보면서 웹페이지를 렌더링하는 과정에 대해 알게 되었다.

렌더링 과정에서 Reflow와 Repaint는 비용이 많이 드는 작업이다.

이번 글에서는 Reflow와 Repaint의 개념에 대해서 알아보면서 브라우저 렌더링을 최적화할 방법에 대해 알아보겠다.

## Reflow(Layout)란?

뷰 포트 내에서 DOM 요소들이 화면에 어떤 위치에 어떤 크기로 배치될지를 결정하게 되는 계산 과정이다. element의 reflow는 DOM에 있는 모든 하위, 상위 요소의 후속 리플로우를 유발한다.

> - 모든 엘리먼트의 위치나 크기 등을 다시 계산하는 과정
> - 상위 엘리먼트를 변경시키면 하위 엘리먼트에도 영향을 끼침
> - render tree를 재생성하므로 부하가 크고 레이아웃에 영향을 줌
> - DOM노드를 추가, 제거 , 업데이트하는 경우 발생

<br>

## Repaint란?

레이아웃에 영향을 미치지 않는 요소의 외관을 변경할 때 발생한다.

> - 레이아웃에 영향을 주지않지만 눈에 보이는 요소들(visibility, background-color, color,..)이 변경됨
> - reflow 보다는 부하가 크지는 않음

<br>

## Reflow와 Repaint가 모두 일어나는 경우

![](https://velog.velcdn.com/images/soopy368/post/f813bd4c-dd8a-4d7b-9bc6-340783d2bf09/image.jpeg)

Reflow가 발생하는 경우 CRP과정에 의해 대부분 Repaint까지 발생하기 때문에 Reflow를 최소화 하는 것이 중요하다.

<br>

## Reflow와 Repaint 최소화하는 방법

### 1) 영향받는 노드 최소화하기 (position fixed, absolute를 활용)

상위 노드의 스타일을 변경하면 하위 노드에 모두 영향을 미친다.

### 2) 최대한 DOM 구조상 끝단에 위치한 노드에 추가

너비나 위치를 변경해야 하는 경우 끝단 노드에서 변경을 한다면, reflow가 일어나는 것은 피할 수 없지만 성능 문제를 최소화 할 수 있다.

### 3) 숨겨진 엘리먼트 수정

display:none 스타일인 경우 DOM 조작과 스타일을 변경하더라도 Reflow, Repaint가 발생하지 않는다. 많은 수의 엘리먼트를 변경해야 할 경우 숨겨진 상태에서 엘리먼트를 변경하고 다시 보이도록 하여 레이아웃 발생을 최대한 줄인다.

주의) visibility: hidden은 보이지 않아 리페인트는 발생하지 않지만, 공간을 차지하기 때문에 레이아웃은 발생하게 된다.

### 4) 테이블 레이아웃 피하기

테이블 값에 따라 넓이를 계산하므로 렌더링이 느려진다. 만약 사용한다면 table-layout:fixed를 사용하면 렌더링을 조금 더 빠르게 할 수 있다.

### 5) 애니메이션 최적화

한 프레임 처리가 16ms(60fps) 내로 완료되어야 렌더링 시 끊기는 현상 없이 자연스러운 렌더링을 만들어낼 수 있다. JS 실행 시간은 10ms 이내에 수행되어야지 reflow, repaint 등의 과정을 포함했을 때 16ms 이내에 프레임이 완료될 수 있다. 따라서 애니메이션을 구현할 때 JS를 사용하는 것보다 CSS 사용을 권장한다.

- requestAnimationFrame() 사용

  브라우저의 프레임 속도(보통 60fps)에 맞추어 애니메이션을 실행할 수 있도록 해준다. 프레임을 시작할 때 호출되기 때문에 일정한 간격으로 애니메이션을 수행할 수 있는 장점이 있다.

### 6) top, left 변경보다 translate 사용하기

top, left는 Reflow를 유발하지만 translate는 Repaint 과정만 거치기 때문에, 위치를 변경하는 것이 필요하다면 되도록 translate를 사용하자.

### 7) will-change 사용

변화가 예상되는 요소를 브라우저에게 미리 알려준다. 브라우저는 실제 요소가 변화되기 전에 적절하게 최적화를 할 수 있습니다.

주의) 레이어를 추가로 만들어두기 때문에 남용하면 오히려 성능에 좋지 않다.

---

📄 references

- https://dev.to/gopal1996/understanding-reflow-and-repaint-in-the-browser-1jbg

- https://web.dev/rendering-performance/
