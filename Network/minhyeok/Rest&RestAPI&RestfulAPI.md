# Rest & RestAPI & RestfulAPI

### REST(Representational State Transfer)
REST는 웹 서비스를 설계하는 아키텍처 패턴이다. 웹 상에서 클라이언트와 서버 간의 정보를 주고 받는 방법 중 하나로, HTTP 프로토콜을 이용하여 데이터를 송수신한다.
#### 구성요소
1. 자원(Resource) : HTTP URI
2. 자원에 대한 행위(Verb) : HTTP Method
3. 자원에 대한 행위의 내용 (Representations) : HTTP Message Pay Load
#### 특징
1. Stateless: 각 요청은 서버에서 다른 요청과 독립적으로 관리된다. 이전 요청에 대한 정보를 저장하지 않으므로 클라이언트의 요청에 필요한 모든 정보를 포함해야 한다.
2. Client-Server: 클라이언트와 서버는 독립적으로 운영되며, 각각의 역할이 명확하게 분리되어 있다.
3. Cacheable: 클라이언트는 서버의 응답을 캐시(임시 저장)할 수 있다. 이를 통해 불필요한 데이터 전송을 줄이고 효율성을 높일 수 있다.
4. Layered System: 클라이언트는 직접 서버에 접근하는 것이 아니라, 여러 계층을 통해 서버에 접근한다. 이를 통해 확장성과 유연성을 높일 수 있다.
5. Code on Demand (optional): 필요에 따라 서버는 클라이언트에 실행 가능한 코드를 전송할 수 있다.
6. Uniform Interface: REST는 통일된 인터페이스를 제공하여 간단하고 일관된 상호작용을 가능하게 한다.
   
### REST API
REST API는 REST 아키텍처를 따르는 API(Application Programming Interface)를 말한다. API는 애플리케이션 간의 정보 교환을 가능하게 하는 도구로, REST API는 HTTP 메소드(GET, POST, PUT, DELETE 등)를 이용하여 웹 서비스에 CRUD(Create, Read, Update, Delete) 연산을 수행한다.

#### RestAPI 디자인 가이드
1. URI는 정보의 자원을 표현해야 한다.
2. 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현한다.
   
    ex)
    - GET /posts: 블로그 포스트 목록을 조회한다.
    - POST /posts: 새로운 블로그 포스트를 작성한다.
    - PUT /posts/:id: 기존 블로그 포스트를 업데이트한다.
    - DELETE /posts/:id: 기존 블로그 포스트를 삭제한다.

### RESTful API
RESTful API는 REST 원칙을 완전히 따르는 API를 의미한다. 즉, RESTful API는 REST 아키텍처 스타일을 완벽하게 구현한 서비스 인터페이스다.

- 요약하면, REST는 웹 서비스를 설계하는 방법론이며, REST API는 이 방법론을 따르는 서비스 인터페이스를 말하고, RESTful API는 REST 원칙을 완벽하게 지키는 서비스 인터페이스를 말한다. 이 세 가지는 모두 웹 서비스를 효율적으로 제공하고 사용하기 위한 방법이다.

#### 장점

1. 별도의 인프라가 필요 없이 HTTP 프로토콜 인프라를 그대로 활용할 수 있다.
2. 모든 플랫폼에서 사용 가능하다. 모든 운영체제와 언어에서 지원한다.
3. 메시지가 의도하는 바를 명확하게 나타내어 이해하기 쉽다.
4. 서비스 디자인에서 발생할 수 있는 문제를 최소화한다.
5. 서버와 클라이언트의 역할을 명확하게 분리한다.
   
#### 단점

1. 표준이 존재하지 않아 정의가 필요하다.
2. HTTP Method의 형태가 제한적이다.
3. 브라우저를 통한 테스트가 많은 서비스의 경우, URL보다 Header 정보를 처리해야 하므로 전문성이 요구된다.
4. 구형 브라우저에서는 호환되지 않는 동작이 많아 지원이 어렵다.