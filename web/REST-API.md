# REST API

# REST란 무엇인가요?

REST(Representational State Transfer)의 약자로 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것을 의미합니다.

### **즉 REST란**

1. HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고,
2. HTTP Method(POST, GET, PUT, DELETE, PATCH 등)를 통해
3. 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것을 의미합니다.

# **REST의 특징**

### 1. 균등한 인터페이스 (Uniform Interface)

REST가 HTTP의 표준만 따른다면 어떠한 기술이던지 접목하여 사용할 수 있기 때문에 플랫폼이나 언어의 제약에 구애받지 않습니다. 요즘은 REST API를 정의할 때 JSON(JavaScript Object Notation) 방식을 가장 많이 사용하지만 XML(eXtensible Markup Language)도 적용할 수 있습니다.

### 2. 무상태성 (Stateless)

서버는 클라이언트의 상황을 고려하지 않고 API 요청에 대해서만 처리하기 때문에 이를 **"상태가 없다"** 라고 표현합니다. 이렇게 되면 클라이언트를 고려하지 않아도 되기 때문에 구현이 간결해집니다.

### 3. 캐싱 가능 (Cacheable)

REST는 HTTP 표준을 기반으로 만들어졌기 때문에 HTTP의 특징인 캐싱을 사용할 수 있습니다. REST API를 활용하여 `GET` 메소드를 `Last-Modified` 값과 함께 보낼 경우, 컨텐츠의 변화가 없을 때 캐시된 값을 사용하게 됩니다. 이렇게 되면 네트워크 응답시간 뿐만 아니라 API 서버에 요청을 발생시키지 않기 때문에 부담이 덜 하다는 장점 또한 가지게 됩니다.

### 4. 자체 표현성 (Self-Descriptiveness)

REST API의 자원명시 규칙 및 메소드는 그 자체로 의미를 지니기 때문에 어떠한 요청에 있어서 그 요청 자체로 어떤 것을 표현하는지 알아보기 쉽습니다. 물론 API를 규정한 각 서비스들이 문서를 제공하지만 이 특성에 따라서 요청하는 방식만으로 어떠한 의미인지 알 수 있어야 좋은 REST API라고 할 수 있습니다.

### 5. 클라이언트-서버 구조 (Client-Server Architecture)

REST 서버가 API를 제공하는 방식이기 때문에 클라이언트에서 처리하는 부분과 독립적으로 동작합니다. 따라서, 서로간의 의존성이 줄어들고 클라이언트와 서버를 최대한 독립적으로 개발할 수 있도록 도와줍니다.

### 6. 계층형 구조 (Layered System)

클라이언트는 계층형 구조가 불가능하지만 REST 서버의 경우, 보안/로드 밸런싱/암호화 등을 추가할 수 있고 Proxy 및 게이트웨이 등의 중간매체를 사용할 수 있습니다.

# **REST의 장단점**

**장점**

- HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구출할 필요가 없습니다.
- HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능합니다. (안드로이드, iOS, 웹 등)
- REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.
- 서버와 클라이언트의 역할을 명확하게 분리한다.

**단점**

- 브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 정보의 값을 처리해야 하므로 전문성이 요구된다.
- 표준이 자체가 존재하지 않아 정의가 필요하다.
- HTTP Method 형태가 제한적이다.

# REST API란?

REST의 원리를 따르는 API를 의미합니다. REST API를 올바르게 설계하기 위해서 지켜야 하는 규칙에 대해 알아보겠습니다.

### 1. URI는 리소스를 표현해야 한다.

- **리소스 명은 동사가 아닌 명사를 사용해야 한다.**

`/students/1`

- **리소스는 Collection과 Document로 표현할 수 있다.**

이 때 Collection은 **복수** 를 사용함을 주의하자.

`/locations/seoul/schools/3`

여기서 `locations` 는 Collection을, `seoul` 은 Document를 표현한다.

### 2. 그 리소스에 대한 행위는 HTTP의 Method로 표현해야 한다.

- **GET은 리소스를 조회한다. (학생 목록 조회)**

`GET /students`

- **POST는 리소스를 생성한다. (학생 생성)**

`POST /students`

- **PUT은 리소스를 업데이트한다. (1번 학생 정보 업데이트)**

`PUT /students/1`

- **DELETE는 리소스를 삭제한다. (1번 학생 삭제)**

`DELETE /students/1`

# RESTful API란?

- RESTFUL이란 **REST의 원리를 따르는 시스템**을 의미합니다. 하지만 REST를 사용했다 하여 모두가 RESTful 한 것은 아닙니다.
- REST API의 설계 규칙을 올바르게 지킨 시스템을 RESTful하다 말할 수 있으며, 모든 CRUD 기능을 POST로 처리 하는 API 혹은 URI 규칙을 올바르게 지키지 않은 API 등은 REST API를 사용하였지만 RESTful 하지 못한 시스템이라고 할 수 있습니다.

## 잘못된 REST 사용

- **GET/POST의 부적합한 사용** : 기존의 조회/생성의 기능이 아닌 다른 방식으로 사용하는 경우이다.
- **자체 표현적이지 않음** : REST의 특징 중 하나인 자체표현성에서 떨어지는 경우로 이해하기 어렵다.
- **HTTP 응답 코드 미사용** : 위에서 정리한 응답에 관한 상태코드를 명확하게 정의하지 않은 경우이다.
- 그 외에 리소스 표현 가이드 및 REST의 특징을 위반한 경우들을 주의하자.
