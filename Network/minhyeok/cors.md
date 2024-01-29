# CORS(Cross Origin Resource Sharing)

- CORS는 SOP정책의 제한을 유연하게 조정하면서, 필요한 경우에만 다른 Origin의 리소스 접근을 허용하는 웹 보안 메커니즘이다.
- 즉, 브라우저에서 다른 출처의 리소스를 공유하는 방법.

### 같은 Origin(출처)란?
<img src="https://velog.velcdn.com/images/gnoeyah/post/c196eace-c2cd-4a9c-bc2e-5bdb03fd18ac/image.png" width="700" height="300">

- 프로토콜 + 도메인 + 포트가 같으면 같은 출처(Origin)이라고 함
  
### SOP(Same Origin Policy)이란?

- 동일한 Origin으로만 요청을 보낼 수 있게 하는 것이다. 즉, 다른 Origin으로 요청을 보낼 수 없도록 금지하는 브라우저의 기본적인 보안 정책이다. 

### SOP 사용 이유

- 예를들어, A라는 웹사이트에서 B라는 웹사이트의 API를 통해(즉, 다른 출처에 있는) 정보를 가져오고자 HTTP 요청을 보냈다. 하지만 CORS 정책에 의해 요청이 차단되는 경우가 있음.
- 브라우저가 방문하려는 사이트를 신뢰하지 않기 때문에 '브라우저'에서 요청이 차단되는 것.
- 해커가 유사한 사이트를 만들어 접속을 유도하고, 개인정보 등 중요데이터를 탈취하는 등의 문제가 발생할 수 있기 때문.
- 이런 상황을 막기 위해 SOP 같은 보안 정책을 사용.

### CORS의 필요성

- 기술이 발전하며 다른 Origin끼리 데이터를 주고받아야 하는 일이 증가하였다.
- 따라서, CORS이 등장하였고 몇 가지 예외 상황에 대해 다른 Origin으로도 요청을 보낼 수 있게 할 수 있다.   

### CORS 동작

### Simple Requests (단순 요청)

<img src="https://user-images.githubusercontent.com/45676906/128662896-89904d60-9b83-460a-9aa4-d0a963dd5ef3.png" width="700" height="500">

<br>

- 과정
  
1. 클라이언트측이 서버에 api요청
2. 서버측에서 Access-Control-Allow-Origin 헤더를 포함한 응답 
3. 브라우저는 Access-Control-Allow-Origin 헤더 체크
4. CORS 수행 판단

- 조건

1. 요청 메소드는 get,post,head 이어야함
2. Accept, Accept-Language, Content-Language, Content-Type, DPR, Downlink, Save-Data, Viewport-Width, Width 헤더 사용
3. Content-Type 헤더는 application/x-www-form-urlencoded, multipart/form-data, text/plain 중 하나를 사용
   
<br>

### Preflighted Requests(프리플라이트 요청) 

<img src="https://user-images.githubusercontent.com/45676906/128663868-79411a72-7317-47c1-b1f5-25597da7da22.png" width="700" height="500">

<br>

- 과정
 
1. 먼저 요청을 받을 수 있는지 확인하기 위해서 Option 메소드를 이용하여 사전 요청을 보낸다. 
2. 서버는 이 예비 요청에 대한 응답으로 Access-Control-Allow-Origin 헤더를 포함한 응답을 브라우저에 전송
3. Access-Control-Allow-Origin 헤더를 확인해서 CORS 동작을 수행할지 판단