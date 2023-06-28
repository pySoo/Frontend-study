## Next.js의 SSR, SSG, ISR 이해하기

### Next.js란?

React를 서버 사이드 렌더링 할 수 있게끔 하는 프레임워크입니다. SSR의 장점으로는 SEO에 유리하고 첫 렌더링 화면이 빠르다는 점이 있습니다.

```
// Next.js로 생성한 프로젝트의 src/pages 디렉토리 구조
src
└── pages
    ├── _app.tsx
    ├── index.tsx
    └── search
        ├── [word].tsx
        └── index.tsx
```

서버 사이드 렌더링을 하기 위해서는 유저가 접속할 페이지를 미리 알고 있어야 합니다. Next.js는 생성할 페이지를 src/pages의 폴더명과 파일명으로 구분합니다.

`index.tsx` 는 루트(/) 경로에 해당하는 페이지이고, `[word]`처럼 대괄호로 감싸져 있는 폴더나 파일명은 동적 라우팅을 할 수 있는 페이지가 됩니다. ‘/search/word’에 해당하는 동적인 경로에 [word]에 해당하는 페이지가 렌더링 되고 그 값은 컴포넌트에서 받아서 사용할 수 있습니다.

그 외에도 `_app.tsx`는 모든 페이지를 초기화하고 재설정할 수 있는 페이지이며 Next.js에 의해 미리 규칙이 정해진 이름을 가진 페이지들도 존재합니다.

### SSR

SSR은 CSR과 반대되는 개념으로 서버에서 페이지를 렌더링해서 클라이언트에 전달해주는 방식입니다.

Next.js에서 SSR이 SSG나 ISR과 구분되는 이유는 렌더링 시점 때문입니다. **SSR은 사용자가 요청을 할 때마다 그 시점에 페이지를 새롭게 렌더링**합니다. 그렇기 때문에 Fetching 하는 데이터가 빈번하게 변경될 때 사용됩니다.

```tsx
export default function SSRPage({ dateTime }: SSRPageProps) {
  return (
    <main>
      <TimeSection dateTime={dateTime} />
    </main>
  );
}

export const getServerSideProps: GetServerSideProps = async () => {
  const res = await axios.get('https://worldtimeapi.org/api/ip');

  return {
    props: { dateTime: res.data.datetime },
  };
```

해당 파일에서 `getServerSideProp`s라는 이름의 함수를 export하면, 클라이언트에서 페이지를 요청할 때 `getServerSideProps` 함수가 실행되고 값을 SSRPage에 전달하여 렌더링한 후 클라이언트에 전달됩니다.

### SSG

SSG는 Static Site Generation의 약자로 Next.js에서 페이지를 생성할 때 기본으로 적용되는 설정입니다. **SSR과 다른 점은 클라이언트가 요청하는 시점이 아니라, 빌드 시에 페이지를 미리 생성**해놓는 점입니다.

```tsx
export default function SSGPage({ dateTime }: SSGPageProps) {
  return (
    <main>
      <TimeSection dateTime={dateTime} />
    </main>
  );
}

export const getStaticProps: GetStaticProps = async () => {
  const res = await axios.get("https://worldtimeapi.org/api/ip");

  return {
    props: { dateTime: res.data.datetime },
  };
};
```

설정 없이도 SSG가 생성되지만 Data Fetching이 필요한 경우에 `getStaticProps` 함수를 export하고 함수 내에서 데이터를 받아서 리턴하면 빌드 시에 `getStaticProps` 함수가 실행되고 컴포넌트에서 리턴 값을 받아서 페이지를 미리 렌더링하게 됩니다.

단, **빌드하는 시점에 페이지가 미리 생성**되는 것이기 때문에 fetching하는 데이터가 변경되더라도 **다시 빌드하지 않는 이상 반영이 되지 않습니다.**

### ISR

ISR은 Incremental Static Regeneration의 약자로 **빌드 시점에 페이지를 렌더링 한 후, 설정한 시간마다 페이지를 새로 렌더링**합니다. 따라서 사실 SSG에 포함되는 개념이라고 할 수 있으나, SSG는 Fetching하는 데이터가 변경되었을 때 다시 빌드하는게 필요하지만 ISR은 일정 시간마다 알아서 페이지를 업데이트 하는 차이가 있습니다.

```tsx
export default function ISR20Page({ dateTime }: ISR20PageProps) {
  return (
    <main>
      <TimeSection dateTime={dateTime} />
    </main>
  );
}

export const getStaticProps: GetStaticProps = async () => {
  const res = await axios.get("https://worldtimeapi.org/api/ip");

  return {
    props: { dateTime: res.data.datetime },
    revalidate: 20,
  };
};
```

SSG와 동일하게 `getStaticProps` 함수에서 fetching할 데이터를 객체 내의 props라는 key 값으로 보내고 revalidate의 값에 따라 해당 초마다 페이지가 새로 렌더링 됩니다.
