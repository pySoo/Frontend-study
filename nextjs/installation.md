## 사전 환경 세팅

#### Node

```bash
brew install node
```

#### create-next-app

React 프레임워크인 Next.js를 사용하기 위해서는 create-next-app을 설치해야 합니다.

```bash
npm install -g create-next-app
```

<br />

## Next.js 프로젝트 생성

create-next-app을 이용하여 Next.js 프로젝트를 생성합니다.

```bash
npx create-next-app my-app
```

위의 명령어를 입력하면 아래의 폴더들이 생성됩니다.

```
|-- pages
|-- public
|-- styles
|-- .eslintrc.json
|-- next.config.js
|-- package.json
```

<br />

## 프로젝트 실행

package.json 파일을 열면 다음 내용을 확인할 수 있습니다.

```json
"scripts": {
  "dev": "next dev",
  "build": "next build",
  "start": "next start",
  "lint": "next lint"
},
```

- dev: Next.js 프로젝트를 개발 모드로 실행합니다.
- build: Next.js 프로젝트를 **production mode로 빌드**합니다.
- start: Next.js 프로젝트를 **productoin mode로 실행**합니다.
- lint: Next.js에 기본 설정된 ESLint 설정을 사용하여 ESLint를 실행합니다.

<br />

Next.js 프로젝트를 개발 모드에서 실행하기 위해서 다음 명령어를 입력합니다.

```bash
npm run dev
```

이로 인해 Next.js의 기본 구성이 완료되었고 다음과 같은 기능이 제공됩니다.

- 자동 컴파일 및 번들링
- React Fast Refresh
- 페이지의 정적 생성 및 SSR
- 기본 url(/)을 통해 제공되는 정적 파일
