<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/2fcb214c-3c8c-4d3a-bbb8-0e4b026741e7/image.jpeg" />

HTML 요소를 원하는 위치에 배치하기 위해 사용하는 속성인 `position`에 대해서 알아보자.

## position

HTML 문서 상에서 요소가 배치되는 방식을 결정한다. 정확한 위치 지정을 위해 `top`, `left`, `bottom`, `right` 등의 속성과 함께 사용한다.

<br>

## position: static

<img width="70%" src="https://velog.velcdn.com/images/soopy368/post/6eb13e91-ae91-4d25-b5e1-4bbfec54352a/image.png" />

position 속성을 별도로 지정하지 않으면 `기본값`인 `static`이 적용된다. static인 요소는 `HTML에 작성된 순서 그대로` 브라우저 화면에 표시된다. top, right 등의 속성 값은 무시된다.

<br>

## position: relative

<img width="70%" src="https://velog.velcdn.com/images/soopy368/post/5e858841-b47d-4133-b6d1-97acf2594f7f/image.png" />

요소를 `원래 위치(static이었을 때)를 기준`으로 `상대적으로 이동`시켜서 배치시킨다. top, bottom, left, right 속성을 이용해서 원래 위치에서부터 상하좌우로 얼마나 떨어지게 할지 지정할 수 있다.

<br>

## position: absolute

<img width="70%" src="https://velog.velcdn.com/images/soopy368/post/ed0513a3-6215-4ac1-9a00-43fbafbb25ff/image.png" />

배치 기준이 상황에 따라 달라지기 때문에 주의해서 사용해야하는 속성값이다.

배치 기준을 자신이 아닌 상위 요소에서 찾는다. DOM 트리를 따라 올라가다가 `position이 static이 아닌` `첫 번째 상위 요소`가 `배치 기준`이 된다. 상위 요소가 없다면 DOM 트리 최상위에 있는 body 요소가 배치 기준이 된다.

보통 absolute를 사용할 때는 부모 요소를 relative로 지정해서 부모 요소를 기준으로 top, right 등을 사용하여 위치를 지정한다.

중요한 점은, position:absolute인 요소는 **HTML 문서 상에서 독립되어** 앞뒤에 나온 요소와 상호작용을 하지 않게 된다.

<br>

## position: fixed

스크롤을 하더라도 요소를 `항상 고정된 위치`에 배치한다. 이러한 동작이 가능한 이유는 `배치 기준이 뷰포트`, 즉 브라우저 전체화면이기 때문이다.

position:absolute와 마찬가지로 **HTML 문서 상에서 독립되어** 앞뒤에 나온 요소와 상호작용을 하지 않게 된다.

<br>

## position: sticky

<img width="70%" src="https://velog.velcdn.com/images/soopy368/post/d467a414-40b8-4bf2-b01b-1e5a98a25599/image.png" />

기준점 이상을 넘지 않을 때는 `relative`처럼 동작하다가 그 이상을 넘게 될 경우 `fixed`처럼 동작한다.

<img width="70%" src="https://velog.velcdn.com/images/soopy368/post/fb484f50-cf3d-4c71-9e39-c6ba7a30492f/image.png" />

<img width="70%" src="https://velog.velcdn.com/images/soopy368/post/c74c4b2f-4198-4ad8-9815-8563a20972a1/image.png" />

그러다 스크롤이 scroll 박스 밖으로 벗어나게 될 경우 그 위치에서 정지하게 된다.

<img width="70%" src="https://velog.velcdn.com/images/soopy368/post/d4c5e63e-72da-4e9e-8683-d305a3002529/image.png" />

예를 들어, top: 20px인 sticky 속성인 경우, 스크롤을 내렸을 때 뷰포트와 요소 사이의 거리가 20px 이상이면 `relative`처럼 동작하다가 20px 이하가 된다면 `fixed` 요소처럼 고정된다.

scroll 박스란 overflow 속성이 존재하는 부모 요소를 뜻한다. overflow를 특별히 명시하지 않았다면 부모 요소가 scroll 박스가 되지만, overflow: hidden일 경우 sticky가 제대로 동작하지 않을 수 있으므로 주의하여야 한다.
