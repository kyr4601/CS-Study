# WEB 통신 전체 흐름

주소창에 url을 입력했을때 통신의 흐름에 대해 알아보자.

웹 통신의 흐름을 보기 전에 알아야할 개념이 있다.

## IP 주소

컴퓨터 네트워크에서 장치들이 서로를 인식하고 통신을 하기 위해서 사용하는 특수한 번호

→ 현재 사용하는 IPv4는 128.0.0.1과 같은 32비트 수로 구성되어 있다.

## Domain Name

IP 주소를 문자로 표현한 주소

→ **DNS : IP주소와 도메인 이름의 매핑 정보를 담는 데이터베이스**

# 작동 방식

![https://velog.velcdn.com/images%2Fwoo0_hooo%2Fpost%2Fe119383c-61cc-46d5-a85d-b27b65ddee1e%2FUntitled.png](https://velog.velcdn.com/images%2Fwoo0_hooo%2Fpost%2Fe119383c-61cc-46d5-a85d-b27b65ddee1e%2FUntitled.png)

1. 사용자가 브라우저에 **도메인 이름을 입력**한다.
2. **DNS서버**에서 사용자가 입력한 Domain name을 검색하고, **매핑되는 IP주소**를 찾는다. 사용자가 입력한 URL 정보와 함께 리턴한다.
3. IP주소는 HTTP 프로토콜을 이용해서 **HTTP 요청 메세지를 생성**한다.
4. 생성된 HTTP 요청 메세지는 TCP 프로토콜을 사용해서 인터넷을 거쳐 **해당 IP 주소의 컴퓨터(서버)로** 전송된다.
5. 서버는 클라이언트의 요청을 승인하고, 응답 메세지를 전송한다.
6. 도착한 HTTP 응답 메세지는 HTTP 프로토콜을 사용하여 **웹페이지 데이터로 변환**되고, 웹 브라우저의 출력에 의해 사용자가 볼 수 있다.

# DNS 서버의 주소 찾기

## DHCP

- Dynamic Host Configuration Protocol

![https://velog.velcdn.com/images%2Fwoo0_hooo%2Fpost%2Fd5ff8273-2fa3-42ee-9158-e51139d93c53%2F2.png](https://velog.velcdn.com/images%2Fwoo0_hooo%2Fpost%2Fd5ff8273-2fa3-42ee-9158-e51139d93c53%2F2.png)

- 호스트의 IP 주소와 TCP/IP 설정을 클라이언트에 의해 자동으로 제공하는 응용 계층 프로토콜
    
    → 사용자는 DHCP 서버에서 자신의 IP 주소, 가장 가까운 라우터의 IP 주소, 가장 가까운 **DNS 서버의 주소**를 받는다.
    

## ARP

- Address Resolution Protocol
- 네트워크상에서 IP 주소를 물리적 네트워크 주소로 바인딩시키기 위해 사용하는 프로토콜
- DHCP로 얻은 라우터의 IP 주소를 MAC 주소로 변환

# DNS 서버에서 IP 정보 수신

![https://velog.velcdn.com/images%2Fwoo0_hooo%2Fpost%2F5ae6f285-3edd-4ed3-8bdf-7fdaaed19e2f%2F3.png](https://velog.velcdn.com/images%2Fwoo0_hooo%2Fpost%2F5ae6f285-3edd-4ed3-8bdf-7fdaaed19e2f%2F3.png)

- 사용자가 웹 브라우저 주소창에 www.example.com을 입력
- www.example.com에 대한 요청이 인터넷 서비스 제공업체(ISP)가 관리하는 DNS 해석기로 라우팅
- DNS 해석기는 요청을 DNS 루트 이름 서버에 전달
- DNS 해석기는 요청을 .com 도메인 TLD(Top-level Domain) 네임 서버 중 하나에 다시 전달
- DNS 해석기는 요청을 Route 53 네임 서버에 다시 전달
- Route 53 네임 서버는 www.example.com 레코드를 찾아 IP주소를 DNS 해석기로 반환
- DNS 해석기는 웹 브라우저에 IP주소 반환

DNS 서버는 계층화 구조를 이루는데, 최상단 계층인 가장 뒷쪽(.com, .kr 등등)을 담당하는 DNS 서버는 **전세계에 13개 뿐**이다.