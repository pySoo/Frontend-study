## Next.js의 Route

Next.js로 개발을 하면서 느꼈던 React와의 큰 차이점은 라우팅하는 방법이었습니다. Next.js는 src/pages 폴더 내의 폴더와 파일명으로 자동으로 path가 설정되기 때문에 이를 이해한 다음 디렉토리 구조를 정해야 합니다.

React와 다른 **Next.js의 라우팅 방법**을 정리하면서 `getstaticpaths`를 사용하여 **동적 라우팅을 하는 방법**에 대해서도 정리해보겠습니다.

## Route

```
src/pages
├── _app.tsx
├── index.tsx
├── naver.tsx
├── preview.tsx
└── search
    ├── [word].tsx
    └── index.tsx
```

Next.js의 경우 따로 path를 설정해주지 않아도 /pages 폴더 내의 폴더명과 파일명에 맞춰서 자동으로 path가 설정됩니다.

동적 라우팅의 경우는 파일이나 폴더명을 `[](대괄호)`로 감싸주면 됩니다.

예를 들어, /search/test로 이동했다면 `[word].tsx`에 해당하는 컴포넌트가 렌더링되고 “test”라는 값을 해당 컴포넌트에서 받아온 것입니다.

## next/route

react-router-dom의 useLocation, useNavigate의 기능을 ‘next/router’의 useRouter로 구현할 수 있습니다.

```tsx
import { useRouter } from "next/router";

export default function Home() {
  const router = useRouter();
  console.log(router);
  // ...
}
```

- pathname : `string` : 현재 경로로 /pages 의 페이지 경로이며(파일명), basePath 또는 locale 은 포함되지 않는다.
- query : `object` : 동적 라우트 매개변수를 포함하는 객체로 구문 분석된 쿼리 문자열
- asPath : `string` : basePath 또는 locale 없이 브라우저에 표시되는 경로(query 포함)
- isFallback : `boolean` : 현재 페이지의 fallback 모드 여부
- basePath : `string` : 활성화된 basePath
- locale : `string` : 활성화된 locale
- locales : `string[]` : 지원되는 모든 locale
- isReady : `boolean` : 라우터 필드가 클라이언트 측에서 업데이트되고 사용할 준비가 되었는지 여부. useEffect 메소드 내에서만 사용해야하며 서버에서 조건부로 렌더링 하는 데에 사용해야 한다.
- isPreview : `boolean` : 애플리케이션의 미리보기 모드

### router.push

클라이언트 사이드의 라우팅 처리를 할 수 있는 기능입니다.

```tsx
router.push(url, as, options);
```

- url(필수) : 이동할 경로
- as : 브라우저에서 표시된 경로
- options
  - scroll : 기본값은 true로 페이지 이동 후 상단으로 스크롤합니다.
  - shallow : 기본값은 false로 `getStaticProps`, `getServerSideProps`, `getInitialProps` 를 실행하지 않고 현재 페이지의 경로를 업데이트합니다.
  - locale : 새 페이지의 로케일을 나타냅니다.

## Dynamic Route 페이지 생성

Dynamic Route된 페이지는 빌드 시에는 path 값을 알 수 없기 때문에 SSR로 구현해야하지만, getstaticpaths를 사용하면 SSG나 ISR로 페이지를 미리 생성할 수 있습니다.

또한, 새로운 path에 대해 요청이 왔을 때 올바른 path인 경우 새 페이지를 렌더링할 수 있습니다.

### path가 고정된 경우

```tsx
export const getStaticPaths = () => {
  const paths = [
    { params: { id: "1" } },
    { params: { id: "2" } },
    { params: { id: "3" } },
  ];

  return {
    paths,
    fallback: false,
  };
};

export const getStaticProps = async (context: { params: { id: string } }) => {
  const res = await axios.get(`https://test.api/${id}`);

  return {
    props: { info: res.data.info },
    revalidate: 20,
  };
};

export default function DefaultPage({ info }) {
  return <div>{info}</div>;
}
```

`/test/[id].tsx` 파일에서 id로 받을 path가 ‘1’, ‘2’, ‘3’으로 고정되어 있는 경우

1. `getStaticPaths` 함수에서 정해진 값을 리턴합니다.
2. `getStaticProps`가 그 값을 받아서 빌드 시에 정적 페이지를 생성합니다.
3. revalidate: 20 옵션으로 인해 20초마다 페이지를 생성하고 업데이트된 fetching 데이터를 반영합니다.
4. `getStaticPaths`가 리턴하는 객체에 fallback: false를 설정하면 ‘1’, ‘2’, ‘3’ 외에 다른 path의 요청이 올 경우 404 에러 페이지가 보이게 됩니다.

### path가 변하는 경우

```tsx
export const getStaticPaths = () => {
  const res = await axios.get(`https://test.api/list`);
  const paths = res.data.list.map(({ id }) => {
    return { params: { id } };
  });

  return {
    paths,
    fallback: "blocking",
  };
};

export const getStaticProps = async (context: { params: { id: string } }) => {
  const res = await axios.get(`https://test.api/${id}`);

  if (!res.data.info) {
    return {
      notFound: true,
      revalidate: 20,
    };
  }

  return {
    props: { info: res.data.info },
    revalidate: 20,
  };
};

export default function DefaultPage({ info }) {
  return <div>{info}</div>;
}
```

`/test/[id].tsx` 파일에서 id로 받을 path가 추가로 생성되거나 삭제될 수 있는 데이터의 id인 경우

1. fallback: ‘blocking’을 사용합니다.
2. 빌드 시 getStaticPaths 함수에서 해당 시점의 id를 받아서 리턴하면 각 id를 path로 하는 정적 페이지가 생성됩니다.
3. fallback: ‘blocking’이기 때문에 새로운 path에 대해 요청을 받으면 그 시점에 새로운 정적 페이지를 생성하게 됩니다.

단, 주의할 점은 데이터가 삭제된 경우에도 revalidate 옵션에 의해 새롭게 페이지를 생성할 때 이전에 정상적으로 생성되었던 페이지도 출력 됩니다.

```tsx
if (!res.data.info) {
  return {
    notFound: true,
    revalidate: 20,
  };
}
```

따라서 삭제된 데이터에 해당하는 페이지가 출력되는 것을 막기 위해 데이터가 없을 때 강제로 404 페이지를 리턴해주어야 합니다.
