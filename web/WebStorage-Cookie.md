<img width="400px" src="https://velog.velcdn.com/images/soopy368/post/a6e619a4-a2e8-45bd-ad15-b6f8fbe85014/image.png" />

Web Storage는 기존 웹 환경의 쿠키(Cookie)와 매우 유사한 개념이지만 쿠키의 일부 단점을 개선하였다. 두 저장 도구 모두 많이 쓰이고 있지만, 적절한 상황에 선택하여 쓸 수 있도록 Web storage와 쿠키의 차이점에 대해서 알아보도록 하겠다.

## 쿠키(Cookie)

> 브라우저 로컬에 저장되는 키와 값(Key/Value)이 들어있는 데이터 파일

### 특징

- `유효 시간을 명시`할 수 있으며, 유효 시간이 남아있다면 브라우저가 종료되어도 인증이 유지됨
- `개수 제한`이 있다. 한 도메인당 20개의 값만 가질 수 있다.
- `크기 제한`이 있다. 쿠키 하나는 4KB까지 저장이 가능하다.
- 사용자가 따로 요청하지 않아도 브라우저가 `Request`시에 `Request header`를 넣어서 `자동으로 서버에 매번 전송`을 한다.
  - 매 HTTP 요청마다 서버로 전송되기 때문에 서버에 부담이 크다.

<br>

## 쿠키의 동작 방식

<div align="center"><img width="80%" src="https://velog.velcdn.com/images/soopy368/post/5a0a8542-303e-404d-ab9e-2209dc9223aa/image.png" /></div>
<br>

1. 클라이언트가 페이지를 요청
2. 서버에서 쿠키를 생성
3. HTTP 헤더에 쿠키를 포함시켜서 응답
4. 브라우저가 종료되더라도 쿠키 만료 시간이 있다면 클라이언트에서 보관하고 있음
5. 같은 요청을 할 경우 HTTP 헤더에 쿠키를 함께 보냄
6. 서버에서 쿠키를 읽어서 이전 상태를 변경할 필요가 있을 때, 업데이트한 쿠키를 HTTP 헤더에 포함시켜 응답

<br>

## Web Storage

> 브라우저에 데이터를 저장할 수 있도록 HTML5부터 새롭게 지원하는 저장소. 키/값 쌍으로 웹의 데이터를 저장한다.

### 특징

- 쿠키와 달리, 서버에 전송되지 않으므로 서버에 부담이 가지 않는다. (명시적으로만 전송 가능)
- 쿠키와 달리, 필요한 경우에만 꺼내 쓰므로 `자동 전송의 위험이 없다.` 다른 도메인에서 요청할 경우, 꺼내쓰고 싶어도 `도메인 단위로 접근이 제한` 되는 특성 덕분에 값을 꺼내 쓸 수 없다.
- 쿠키와 달리, `유효기간이 존재하지 않으며` 대략 5MB까지의 데이터를 저장할 수 있다.
- `데이터의 지속성`에 따라 `LocalStorage`와 `SessionStorage`로 나뉜다.

Web Storage가 쿠키에 비해 특별히 좋은 점은 없다고 봐도 무방하지만, 쿠키는 매 요청마다 서버에 전송을 하기 때문에 **서버 부담을 줄이길 원한다면 Web Storage 사용을 추천**한다.

<br>

## LocalStorage vs SessionStorage

Web Storage는 데이터의 지속성과 관련하여 두 가지 용도의 저장소를 제공한다.

우선 기본적으로 쿠키와 마찬가지로 사이트의 `도메인 단위로 접근이 제한`된다. 예를 들어, 네이버에서 저장한 데이터는 카카오 도메인에서 조회할 수 없다는 것이다.

### LocalStorage

- 데이터를 명시적으로 지우지 않는 이상 `영구적`으로 보관이 가능하다.
- 서로 다른 브라우저 탭이라도 `동일한 도메인`이라면 `동일한 저장소`를 사용한다.
- `지속적`으로 필요한 정보를 저장하기 좋다. (예: 자동 로그인)

### SessionStorage

- 데이터 유효 기간 설정이 가능하고, `탭/윈도우 단위`로 스토리지가 생성된다.
- LocalStorage와 다르게 같은 도메인이라도 `창이 다르다면` `다른 스토리지`를 가진다.
- `일회성` 정보를 저장하기 좋다. (예: 입력 폼 저장, 일회성 로그인)
