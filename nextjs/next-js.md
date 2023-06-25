## Next.js와 CSR, SSR

### CSR(Client-Side Rendering)

- CSR은 쉽게 말하면 클라이언트 단에서 렌더링을 모두 처리하는 방식입니다.
- 서버에서 전체 페이지를 한 번 렌더링 하여 보여주고, 사용자가 요청할 때마다 **클라이언트가 JS를 이용하여 화면을 동적으로 렌더링**합니다.
- SSR과 달리 서버에 HTML 문서를 요청하지 않고 브라우저에서 JS로 화면을 렌더링 합니다.

**장점**

- 동적으로 빠르게 렌더링이 되기 때문에 **사용자 경험(UX)이 좋습니다.**
- 서버에게 요청하는 횟수가 적기 때문에 **서버의 부담이 덜합니다.**

**단점**

- 모든 스크립트 파일이 로드 될 때까지 기다려야 하기 때문에 **첫 화면을 보기까지 시간이 오래 걸립니다.**
- **SEO 성능이 낮습니다.** HTML에 있는 태그를 분석하는 검색 엔진들은 JS 위주인 CSR에서 크롤링을 하는데 어려움을 겪습니다.

<br />

### SPA로 구성된 웹 앱에서 SSR이 필요한 이유

- **SEO 성능 향상**
  - 일반 사용자에게 검색되어야 하는 사이트라면 SEO 성능을 반드시 고려하게 됩니다.
  - SSR 방식은 페이지 이동 시에 서버로부터 HTML을 받아오기 때문에 HTML의 타이틀, 태그 등으로 검색 엔진에 노출되기 훨씬 쉬워집니다.
- **빠른 첫 화면 로딩**
  - CSR에서는 첫 렌더링 시에 모든 HTML과 JS 파일이 로드 될 때까지 기다려야 하기 때문에 로딩 시간이 길지만, SSR은 만들어진 HTML 문서를 서버로부터 바로 받기 때문에 페이지 로딩이 빨라집니다.

<br />

### Next.js의 작동원리

- SPA를 사용하는 **React에서 Next.js를 사용하는 가장 큰 이유는 SSR** 때문입니다.
- 하지만 CSR 방식으로도 사용하면서 SPA의 장점을 유지할 수 있습니다.

1. 초기에 사용자가 서버에 데이터를 요청하면, **SSR 방식으로 렌더링 될 HTML을 브라우저로** 보냅니다.
2. 브라우저에서 JS를 다운로드하고 React를 실행합니다.
3. 사용자와 페이지가 상호작용하여 **다른 페이지로 이동**할 때는, 서버가 아닌 **CSR 방식으로 브라우저에서 처리하여 화면 전환 속도를 높입**니다.

<br />

### Next.js에서 yarn start를 실행했을 때

- 이 항목의 과제를 내주신 이유는 라이브러리의 동작과 구현이 궁금할 때 공식 문서 또는 Github에서 코드를 보며 직접 찾는 방법을 익힐 수 있게 하기 위함인 것 같습니다.
- https://github.com/vercel/next.js/

1. yarn start 명령어를 입력하면 next start가 실행이 됩니다.

   ```json
   	"scripts": {
       "dev": "next dev",
       "build": "next build",
       "start": "next start",
       "lint": "next lint"
     }
   ```

2. next start의 코드를 참조하기 위해서 node_modules를 참고하였습니다.

   nextStart 내부의 startServer를 통해 서버가 실행됩니다.

   ```jsx
   // node_modules/next/dist/cli/next-start.js

   /** nextStart */
   const nextStart = (argv)=>{
       const validArgs = {
           // Types
           "--help": Boolean,

   /** ...  */

       const dir = (0, _getProjectDir).getProjectDir(args._[0]);
       const host = args["--hostname"] || "0.0.0.0";
       const port = (0, _utils).getPort(args);
       const keepAliveTimeoutArg = args["--keepAliveTimeout"];

   /** ...  */

   /** 서버 실행  */
   startServer({
           dir,
           hostname: host,
           port,
           keepAliveTimeout
       }).then(async (app)=>{
           const appUrl = `http://${app.hostname}:${app.port}`;
           Log.ready(`started server on ${host}:${app.port}, url: ${appUrl}`);
           await app.prepare();
       }).catch((err)=>{
           console.error(err);
           process.exit(1);
       });
   };
   exports.nextStart = nextStart;
   ```

3. startServer의 코드 흐름입니다.
   \_next를 통한 핸들러를 찾아볼 수 있습니다.

   ````jsx
   // node_modules/next/dist/server/lib/start-server.js

       var _next = _interopRequireDefault(require("../next"));

        /** ...  */

       function startServer(opts) {

       /** ...  */

           return new Promise((resolve, reject)=>{

       /** ...  */

               /** _next를 이용한 핸들러  */
                   const app = (0, _next).default({
                       ...opts,
                       hostname,
                       customServer: false,
                       httpServer: server,
                       port: addr && typeof addr === "object" ? addr.port : port
                   });
                   requestHandler = app.getRequestHandler();
                   upgradeHandler = app.getUpgradeHandler();
                   resolve(app);
               });
               server.listen(port, opts.hostname);
           });
       }
       ```

   ````

4. next.js 코드를 살펴보면 서버를 만드는 역할을 하는 것을 알 수 있습니다.

   ```jsx
   // node_modules/next/dist/server/next.js

   class NextServer {
       constructor(options){
           this.options = options;
       }
       get hostname() {
           return this.options.hostname;
       }
       get port() {
           return this.options.port;
       }
       getRequestHandler() {
           return async (req, res, parsedUrl)=>{
               const requestHandler = await this.getServerRequestHandler();
               return requestHandler(req, res, parsedUrl);
           };
       }
       getUpgradeHandler() {
           return async (req, socket, head)=>{
               const server = await this.getServer();

       }
   /** ...  */

       setAssetPrefix(assetPrefix) {
           if (this.server) {

   /** ...  */

   exports.NextServer = NextServer;

    /** 여기에서 서버를 만드는 것 같음  */

   function createServer(options) {
       if (options == null) {
           throw new Error("The server has not been instantiated properly. https://nextjs.org/docs/messages/invalid-server-options");
       }
   ```

- 코드 참조: [https://velog.io/@jungjaedev/Next.js-yarn-start-스크립트를-실행했을-때-흐름](https://velog.io/@jungjaedev/Next.js-yarn-start-%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A5%BC-%EC%8B%A4%ED%96%89%ED%96%88%EC%9D%84-%EB%95%8C-%ED%9D%90%EB%A6%84)
