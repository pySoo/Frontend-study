<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/0e4317b0-01fe-46b2-a160-720e56dabab5/image.png" />

`여백을 조정할 때 쓰이는` margin과 padding의 차이는 위의 박스 모델을 보면 이해가 쉽다.

## Margin

- Object와 화면과의 여백 `(외부 여백)`
- 다른 요소와 거리를 두기 위한 영역

## Padding

- Object 내의 `내부 여백`
- Content와 Border 사이의 여백. Content 영역이 배경 요소를 가질 때 padding에도 영향을 미치기 때문에 content의 연장으로도 볼 수 있다.

그 외의 차이점은 `margin만 음수 값과 auto를 적용`할 수 있다는 점이다.

## Margin과 Padding의 사용법

#### 1. 네 가지 속성 사용

- 시계 방향(위, 오른쪽, 아래, 왼쪽) 순서로 값을 주면 된다.
  - 예) margin: 10px 5px 10px 5px

#### 2. 두 가지 속성 사용

- 상하 / 좌우 여백을 의미한다.
  - 예) margin: 10px 5px

#### 3. 한 가지 속성 사용

- 위, 오른쪽, 아래, 왼쪽 모두 같은 값을 사용하게 된다.

#### 4. 단일 속성 부여

- 방향에 대한 속성을 직접 지정해주면 된다.
  - 예) margin-left: 10px, padding-top: 5px

#### 5. 가운데 정렬

- auto를 이용한다. **padding은 auto 사용이 불가능하다.**
