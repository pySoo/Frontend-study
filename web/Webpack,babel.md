# 모듈 번들러란?

![](https://velog.velcdn.com/images/soopy368/post/da007d4c-8e2b-4c86-9bfd-1af94cda46d1/image.png)

프론트엔드 개발은 모듈 단위로 파일을 엮어서 개발하는 방식이다. 즉, 모듈은 서로 의존성을 띄고 있는데 이런 점에서 다음과 같은 문제들이 생긴다.

- 수많은 모듈들의 순서를 어떻게 처리할 것인가? (의존성 처리)
- 모듈이 많아질수록 HTTP 요청이 많아질텐데 이로 인한 오버헤드는 어떻게 해결할 것인가?
- ES6+ 스펙의 코드를 어떻게 처리할 것인가?

위 문제들을 해결하기 위해 등장한 것이 모듈 번들러이다. `모듈 번들러`란 html 파일에 들어가는 `JS 파일들을 각각의 모듈 의존성을 해결하여 하나의 JS 파일로 만들어주는 도구`이다. 이미지 압축, 최소화 등의 기능들도 제공하며 유명한 번들러로는 Webpack, Parcel, Rollup 등이 있다.

우린 그 중 최신 프론트엔드 프레임워크에서 가장 많이 사용되는 `Webpack`에 대해서 알아보겠다.

<br>

## Webpack을 왜 사용해야 할까?

![](https://velog.velcdn.com/images/soopy368/post/916caa94-18a3-46d5-8136-c74034ca3f64/image.png)

- 옛날에는 페이지마다 새로운 html을 요청해서 뿌려 주는 방식이였다면, 요새는 SPA **하나의 html 페이지에 여러개의 자바스크립트 파일들이 포함**한다. Webpack을 사용하면, **연관 되어 있는 자바스크립트 `파일들을 의존성을 해결`하여 하나의 파일로 묶어줘서 관리하기 편하다.** <br><br>
- 파일을 컴파일 할 때, **여러 모듈들의 파일을 읽어오는데 시간이 오래 걸린다.** 그 부분을 해결하기 위해 여러 파일을 `하나의 파일로 번들링` 해준다. <br><br>
- 하나의 자바스크립트 파일로 만들어서 `웹페이지 성능 최적화`를 해준다.

Webpack의 구조에 대해 알아보기 전에, 함께 쓰이는 `Babel`에 대한 이해가 필요하다.

<br>

## Babel이란 무엇인가?

- Babel은 JS의 최신 ES6 버전을 구 버전인 ES5로 변환해준다.

## Babel은 왜 사용해야 하는가?

- 최신 업데이트 중 ES6 버전 업데이트는 큰 부분을 차지한다. 크롬, 사파리, 파이어폭스(98%)와 같은 에버그린 브라우저는 최신 업데이트 버전으로 지원을 해준다. 하지만 인터넷 익스플로러11는 사용하는 비율이 11% 정도나 되는데 지원을 하지 않는다. <br><br>
- **아직 `ES6를 지원하지 않는 브라우저도 존재`하기 때문에 구 버전인 ES5 버전으로 바꿔주는 것이 필요하다.** <br><br>
  - 그래서 개발환경을 설정할 때, webpack이랑 babel로 기초 환경 설정을 잡고 개발을 해야 한다.

<br>

## Webpack의 구조

#### 1. [Entry](https://webpack.js.org/concepts/#entry)

웹팩이 build를 시작하는 지점

#### 2. [Output](https://webpack.js.org/concepts/#output)

웹팩이 build를 완료하면 output의 경로를 통해 번들된 빌드 파일을 생성한다.

#### 3. [Loaders](https://webpack.js.org/concepts/#loaders)

기본적으로 웹팩은 JS나 JSON 파일들만 번들 모듈로 관리할 수 있지만,
Loader들을 통해 css나 babel, 이미지 파일들도 관리할 수 있다.

#### 4. [Plugins](https://webpack.js.org/concepts/#plugins)

Loader가 파일 단위로 번들링을 처리하는 반면, 플러그인은 번들된 결과물을 처리한다.
예를 들어 플러그인을 통해서 번들된 결과물을 HTML로 생성해준다거나, 웹팩이 빌드하는데 걸린 시간 등을 플러그인을 통해 처리할 수 있게 된다.

#### 5. [Mode](https://webpack.js.org/concepts/#mode)

웹팩의 번들링은 빌드 모드에 따라서 차이가 있다.
Development는 빠르게 빌드하기 위해 빌드 최적화를 안 하는 특징을 가지고 있고 Production은 빌드할 때 최적화 작업을 하고 있다. (기본적인 파일 압축 등)

## Webpack의 특징: Code Splitting

만약 번들링된 파일의 용량이 너무 크다면 해당 파일을 읽어오는데 상당히 오랜 시간이 걸릴 것이다.
code splitting은 사용자가 현재 필요로 하는 것들만 load하게 되어 로딩 시간을 단축시켜준다.

---

📄 references

- https://webpack.js.org/concepts/#entry
