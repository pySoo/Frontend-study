# DOM과 BOM

지금부터 DOM과 BOM의 차이와 각 특징에 대해 알아보겠다.

## DOM (Document Object Model)

**웹페이지를 제어할 수 있게 해주는 문서 객체모델** 이다. <br />
문서 객체란 HTML 문서의 태그들을 JS가 읽을 수 있는 객체로 만든 것이다. (document.getElementById())

<br />

### DOM은 수정사항이 있을 때 처음부터 렌더링을 거치기 때문에 스케일이 커질수록 노드가 증가하여 브라우저 속도가 느려진다.

최상위 인터페이스로 Node가 있으며 document 객체를 통하여 접근이 가능하다. <br />

- getElementById()
- querySelector() 등

### 가상 DOM(Virtual DOM)

- React는 가상 DOM 기반의 렌더링 시스템으로 빠른 렌더링 속도를 지원한다.
- 가상 DOM은 실제 DOM의 복사본을 메모리에 저장하여 사용한다.
- 수정 될 때마다 전체가 렌더링 돼서 속도의 단점이 있는 DOM과 달리, 가상 DOM은 변경될 때마다 <br />
  진짜 DOM과 비교하여 차이나는 부분만 렌더링 하기 때문에 속도가 빠르다.

<br>

## BOM (Browser Object Model)

**브라우저를 제어할 수 있게 해주는 객체모델** 이다. <br />
이를 통해서 브라우저의 새 창을 열거나 다른 문서로 이동하는 등의 기능을 실행시킬 수 있다. <br />
DOM은 현재 보이는 웹문서에 대한 제어라면 BOM은 window를 제어한다.

- window 객체를 통해 접근한다.
- location, document, navigator, history, screen
