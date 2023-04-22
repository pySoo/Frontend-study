HTML 요소를 원하는 위치에 배치하기 위해 사용하는 속성인 `position`에 대해서 알아보자.

## position

HTML 문서 상에서 요소가 배치되는 방식을 결정한다. 정확한 위치 지정을 위해 `top`, `left`, `bottom`, `right` 등의 속성과 함께 사용한다.

## position: static

position 속성을 별도로 지정하지 않으면 `기본값`인 `static`이 적용된다. static인 요소는 `HTML에 작성된 순서 그대로` 브라우저 화면에 표시된다. top, right 등의 속성 값은 무시된다.

## position: relative

요소를 원래 위치(static이었을 때)를 기준으로 상대적으로 이동시켜서 배치시킨다. top, bottom, left, right 속성을 이용해서 원래 위치에서부터 상하좌우로 얼마나 떨어지게 할지 지정할 수 있다.

## position: absolute

배치 기준이 상황에 따라 달라지기 때문에 주의해서 사용해야하는 속성값이다. 배치 기준을 자신이 아닌 상위 요소에서 찾는다. DOM 트리를 따라 올라가다가 `position이 static이 아닌` `첫 번째 상위 요소`가 `배치 기준`이 된다. 상위 요소가 없다면 DOM 트리 최상위에 있는 body 요소가 배치 기준이 된다.

보통 absolute를 사용할 때는 부모 요소를 relative로 지정해서 부모 요소를 기준으로 top, right 등을 사용하여 위치를 지정한다.

중요한 점은, position:absolute인 요소는 `HTML 문서 상에서 독립되어` 앞뒤에 나온 요소와 상호작용을 하지 않게 된다.

## position: fixed

스크롤을 하더라도 요소를 항상 고정된 위치에 배치한다. 이러한 동작이 가능한 이유는 `배치 기준이 뷰포트`, 즉 브라우저 전체화면이기 때문이다.

position:absolute와 마찬가지로 `HTML 문서 상에서 독립되어` 앞뒤에 나온 요소와 상호작용을 하지 않게 된다.

## position: sticky

기준점 이상을 넘지 않을 때는 `relative`처럼 동작하다가 그 이상을 넘게 될 경우 `fixed`처럼 동작한다. 그러다 스크롤이 scroll 박스 밖으로 벗어나게 될 경우 그 위치에서 정지하게 된다.

scroll 박스란 overflow 속성이 존재하는 부모 요소를 뜻한다. overflow를 특별히 명시하지 않았다면 부모 요소가 scroll 박스가 되지만, overflow: hidden일 경우 sticky가 제대로 동작하지 않을 수 있으므로 주의하여야 한다.
