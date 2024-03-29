# DHCP
## DHCP란?
     DHCP는 Dynamic Host Configuration Protocol의 약자로, 컴퓨터 네트워크에서 호스트의 IP 주소와 그 외 관련된 설정을 자동으로 할당해주는 프로토콜이다.

## DHCP의 목적
- 초기 인터넷에서는 호스트에게 IP 주소를 할당하기 위해 수동으로 설정

- 또한, 호스트가 나가고 들어올때마다 재배치가 필요하였음

- 각 호스트 IP를 추적해야하고 수동이기 때문에 번거로워 자동으로 IP를 할당해주는 DHCP 등장

## 장점


1. 자동화

    DHCP는 IP 주소를 자동으로 할당하므로, 네트워크 관리자가 각각의 장치에 수동으로 IP 주소를 설정할 필요가 없다.

2. IP 주소 관리 효율성

     IP 주소를 동적으로 할당하므로 IP 주소의 낭비를 줄일 수 있다.

3. 네트워크 설정의 단순화

    DHCP는 IP 주소 외에도 서브넷 마스크, 기본 게이트웨이, DNS 서버 등의 정보도 자동으로 설정할 수 있다.

## 단점

1. 서버 의존성

     DHCP 서버가 작동하지 않으면 새로운 장치는 IP 주소를 받을 수 없으므로 네트워크 접속에 문제를 일으킬 수 있다.

2. 보안 문제

     DHCP는 IP 주소를 무작위로 배정하므로, 특정 장치를 추적하기 어렵다. 따라서 보안에 취약할 수 있다.
     
3. IP 주소 변경

     DHCP는 IP 주소를 임시로 할당하므로, 장치가 네트워크에 재접속할 때마다 IP 주소가 바뀔 수 있다. 이는 일부 애플리케이션에서 문제가 될 수 있다.
    
    - 일부 애플리케이션과 서비스는 특정 IP 주소에 바인딩되어 작동하는 경우가 있다. 예를 들어, 웹 서버나 데이터베이스 서버, FTP 서버 등의 서비스는 특정 IP 주소를 통해 접근되거나, 고정된 IP 주소를 통해 외부와 통신하는 경우가 많다. 때문에 변경된 IP에 접근이 실패하는 경우가 생길 수 있다.


## IP 주소 할당 과정

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F2160C93D571C6CBC23" width="700" height="600">

<br>

- DORA (DHCP Discover, Offer, Request, Acknowledgement)라고 부르기도 한다.

1. DHCP Discover

    클라이언트가 네트워크에 브로드캐스트 메시지를 보내 DHCP 서버를 찾는다. 이 메시지는 클라이언트의 MAC 주소와 함께 전송되며, 클라이언트가 IP 주소를 필요로 한다는 것을 알리는 신호다.

2. DHCP Offer

    DHCP 서버가 이 메시지를 받으면, 사용 가능한 IP 주소와 함께, 서브넷 마스크, DNS 서버 주소, 라우터(게이트웨이) 주소, IP 주소 임대 기간 등의 정보를 제안하는 메시지를 보낸다.

3. DHCP Request

     클라이언트는 제안받은 주소를 수락하고, 이를 DHCP 서버에 알리는 요청 메시지를 보낸다. 이 메시지에는 클라이언트가 수락한 IP 주소와 네트워크 설정 정보가 포함된다.

4. DHCP Acknowledgement (ACK)

     DHCP 서버는 IP 주소 할당을 확정하고, 이에 대한 정보를 포함한 ACK 메시지를 클라이언트에게 보낸다. 이 메시지에는 할당된 IP 주소, 서브넷 마스크, DNS 서버 주소, 라우터 주소, IP 주소 임대 기간 등의 정보가 포함된다.

## 참조
https://jwprogramming.tistory.com/35