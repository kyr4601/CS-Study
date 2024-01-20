# 📌 REST / REST API / RESTful API

## 🔸REST

**웹의 장점을 최대한 활용할 수 있는 아키텍처 스타일**

Representational State Transfer

자원(resource)의 표현(representation)에 의한 상태 전달

- 자원 : 해당 SW가 관리하는 모든 것 (문서, 그림, 데이터 등)
- 표현 : 그 자원을 표현하기 위한 이름 (학생 정보가 자원이라면 'students' 등)
- 상태 전달 : 데이터가 요청되는 시점에 자원의 상태를 전달 (JSON 혹은 XML)

어떤 자원에 대해 CRUD(Create, Read, Update, Delete) 연산을 수행하기 위해

URI(Resource)로 GET, POST 등의 방식(Method)을 사용하여 요청을 보내며,

요청을 위한 자원은 특정한 형태(Representation of Resource)로 표현

### 특징

1. Server-Client (서버-클라이언트 구조)
    - 자원이 있는 쪽이 Server, 자원을 요청하는 쪽이 Client
2. Stateless (무상태)
    - HTTP 프로토콜은 Stateless Protocol이므로 REST 역시 무상태성
3. Cacheable (캐시 처리 기능)
    - 웹에서 사용하는 기존의 인프라를 그대로 활용
4. Uniform Interface (인터페이스 일관성)
    - URI로 지정한 Resource에 대한 요청을 통일되고, 한정적으로 수행
5. Self-Descriptiveness (자체 표현)
    - 요청 메시지만 보고도 쉽게 이해할 수 있는 자체 표현 구조
    

## 🔸REST API

REST의 특징을 기반으로 서비스 API를 구현한 것

각 요청이 어떤 동작이나 정보를 위한 것인지를 그 요청의 모습 자체로 추론이 가능한 것

1. URI는 정보의 자원을 표현 ⇒ /user, /review 등
2. 자원에 대한 행위는 HTTP Method로 표현 ⇒ 행위는 URL에 포함하지 않는다

### 설계규칙

1. URI는 명사를 사용할 것
    - 동사를 사용하지 말라는 얘기=> 예시 : `/getUsers, /createNewUser`
2. 슬래시( / )로 계층 관계를 표현할 것
    - 예시 : `/review/comment`
3. URI 마지막 문자로 슬래시( / )를 포함하지 말 것
4. 밑줄( _ )을 사용하지 말고 하이픈( - )을 사용할 것
5. URI는 소문자로만 구성할 것
6. HTTP 응답 상태 코드 사용할 것
    - 클라이언트는 해당 요청에 대한 실패 처리완료, 잘못된 요청 등에 대한 피드백 필요
    - 2xx은 성공 / 4xx은 클라이언트 실패 / 5xx은 서버 실패
7. 파일확장자는 URI에 포함하지 않는다.
    - 예시 : `http://test.com/review/img.jpg`
    

## 🔸RESTful API

REST의 설계 규칙을 잘 지켜서 설계된 API
즉, REST의 원리를 잘 따르는 시스템을 RESTful 이란 용어로 지칭한다. 

RESTful API는 이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것을 목적으로, 

만약 성능이 중요한 상황이라면 굳이 RESTful 한 API를 구현할 필요는 없다.