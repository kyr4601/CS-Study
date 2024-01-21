# HTTP & HTTPS
### HTTP (HyperText Transfer Protocol)

클라이언트(보통 웹 브라우저)와 서버 간에 웹 페이지와 같은 리소스를 주고 받을 때 사용하는 프로토콜이다.

- 80 포트 사용

- HTTP는 OSI(Open Systems Interconnection) 애플리케이션 계층 프로토콜로, 여러 유형의 요청과 응답을 정의하고 있다.
  
- HTTP는 암호화되지 않은 데이터를 전송해, 제3자가 그 정보를 가로채고 읽을 수 있다.
  
### HTTPS (HyperText Transfer Protocol Secure)

HTTP의 확장 버전으로 보다 안전한 버전이다. 통신하는 과정에서 HTTPS는 전송 내용을 암호화한다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbOzjCx%2Fbtrvvu5Nm2Y%2FiVDFRa1dBTAbX9K4zSdHsk%2Fimg.png" width="700" height="300">

- 443 포트 사용
  
- 기존 http 프로토콜에 SSL(현재는 TLS) 프로토콜을 추가해 데이터를 암호화하고, 이를 통해 사용자 민감정보를 보호할 수 있다.
  
- 제3자 인증, 공개키 암호화, 비밀키 암호화를 사용하여 보안
  
- 제 3자 인증을 하기위해, 접속한 서버가 신뢰할 수 있는지를 확인하기 위해 CA로부터 SSL 인증서를 발급받아야 한다.
  
- CA는 웹사이트 소유자의 신원을 확인하고, 해당 웹사이트가 인증서에 명시된 소유자에게 속하는 것을 보증한다.

    CA(Certificate Authority) : SSL 인증서를 보장해주기 위한 외부 기관
    인증서에는 사이트의 정보와 서버의 공개키를 포함




### HTTPS 사용 장점

- 구글 같은 검색엔진은 https를 사용하면 검색 순위에 상단에 위치 시킨다.


### 모든 웹 사이트가 https를 사용하지 않는다  

- 암호화 통신은 CPU나 메모리등 리소스가 많이 필요하기 때문
- 따라서 민감한 정보를 다룰 때 https를 사용  

