# DHCP/NAT

IP 주소 부족의 문제를 해결하기 위해 적은 IP 주소를 동적으로 활용한다는 점에서 NAT는 DHCP와 비슷한 느낌으로 다가올 수 있다. 그렇지만, 이 둘은 근본적으로 다른 개념입니다. 먼저, DHCP와 NAT의 개념은 각각 IP 주소를 **할당**하고, IP 주소를 **매핑(변환)**하는 것이다

DHCP에서는 호스트가 하나의 IP 주소를 받아 그 주소를 이용해 통신하고, 

NAT는 자신이 사용하는 주소로 보낸 패킷이 그 주소와 매핑되는 주소로 다시 쓰여지는 과정이라는 점에서 근본적인 차이가 있다. 

# NAT (Network Address Translation)

네트워크의 **IP 주소를 관리하거나, IP 주소를 변경**하고 싶을 때 이용한다. 

### **NAT를 사용하는 목적**

1. **인터넷의 공인 IP주소를 절약할 수 있다.** 

![Untitled](DHCP%20NAT%20bca71bf3798649218ed365866f332abb/Untitled.png)

인터넷의 공인 IP주소는 한정되어 있기 때문에 이를 공유할 수 있도록 하는 것이 필요하다. **NAT를 이용하면 사설 IP 주소를 사용하면서 이를 공인 IP주소와 상호 변환 할 수 있도록 하여, 공인 IP주소를 다수가 함께 사용하게 함**으로써 이를 절약할 수 있다는 것이다. 

즉 회사와 같은 곳에서 내부에서는 사설 IP 주소를 통하여 그들끼리 통신하고 회사 밖 인터넷은 회사의 IP로써 통신하는 구조에서 사용된다 볼 수 있겠다.

1. **인터넷이란 공공망과 연결되는 사용자들의 고유한 사설망을 침입자들로부터 보호할 수있다.** 

공개된 인터넷과 사설망 사이에 **방화벽(Firewall)**를 설치하여 외부 공격으로부터 사용자의 통신망을 보호하는 기본적인 수단을 활용할 수 있다. 

이 때 외부 통신망 즉 인터넷망과 연결하는 장비인 라우터에 NAT를 설정할 경우, Space 내부에서는 private address, 외부에서는 public address를 사용할 수 있고 필요시에 이를 서로 변환할 수 있다.

따라서 외부 침입자가 공격하기 위해서는 사설망의 내부 사설 IP주소를 알아야 하기 때문에 공격이 불가능해지므로 내부 네트워크를 보호할 수 있다. 

# DHCP (Dynamic Host Configulation Protocol)

DHCP란 호스트의 **IP주소와 각종 TCP/IP 기본 설정을 클라이언트에게 자동적으로 제공해주는 프로토콜**을말한다. 

DHCP에 대한 표준은 RFC문서에 정의되어 있으며, DHCP는 네트워크에 사용되는 IP주소를 DHCP서버가 중앙집중식으로 관리하는 **클라이언트/서버 모델**을 사용하게 된다. DHCP지원 클라이언트는 **네트워크 부팅과정**에서 DHCP서버에 IP주소를 요청하고 이를 얻을 수 있다.

네트워크 안에 컴퓨터에 자동으로 네임 서버 주소, IP주소, 게이트웨이 주소를 **할당**해주는 것을 의미하고, 해당 클라이언트에게 **일정 기간 임대를 하는 동적 주소 할당 프로토콜**이다.

따라서 호스트가 빈번하게 접속을 연결하고 다시 갱신하는 가정 인터넷 접속 네트워크 및 **무선랜(LAN)**에서 폭 넓게 사용된다. 

**DHCP장점**

PC의 수가 많거나 PC 자체 변동사항이 많은 경우 IP 설정이 자동으로 되기 때문에 효율적으로 사용 가능하고, IP를 자동으로 할당해주기 때문에 IP 충돌을 막을 수 있다.

**DHCP단점**

DHCP 서버에 의존되기 때문에 서버가 다운되면 IP 할당이 제대로 이루어지지 않는다.

## DHCP 구성

### **1) DHCP서버**

 DHCP서버는 네트워크 인터페이스를 위해서 **IP주소를 가지고 있는 서버에서 실행되는 프로그램**으로

**일정한 범위의 IP주소를 다른 클라이언트에게 할당하여 자동으로 설정하게 해주는 역할**을 한다. **DHCP서버는 클라이언트에게 할당된 IP주소를 변경없이 유**지해 줄 수 있다.**클라이언트에게 IP 할당 요청이 들어오면 IP를 부여해주고 할당 가능한 IP들을 관리**해주게 됩니다.

### **2) DHCP클라이언트**

클라이언트들은 시스템이 시작하면 DHCP서버에 자신의 시스템을 위한 IP주소를 요청하고, DHCP 서버로부터 IP주소를 부여받으면 TCP/IP 설정은 초기화되고 다른 호스트와 TCP/IP를 사용해서 통신을 할 수 있다.

**즉, 서버에게 IP를 할당받으면 TCP/IP 통신을 할 수 있다.**

### DHCP 프로토콜의 원리

 DHCP를 통한 IP 주소 할당은 **“임대”**라는 개념을 가지고 있는데

 이는 DHCP 서버가 IP 주소를 영구적으로 단말에 할당하는 것이 아니고 **임대기간(IP Lease Time)**을 명시하여 그 기간 동안만 단말이 IP 주소를 사용하도록 하는 것이다.

단말은 임대기간 이후에도 계속 해당 IP 주소를 사용하고자 한다면 **IP 주소 임대기간 연장(IP Address Renewal)**을 DHCP 서버에 요청해야 하고 또한 단말은 임대 받은 IP 주소가 더 이상 필요치 않게 되면 **IP 주소 반납 절차(IP Address Release)**를 수행하게 된다.

[https://t1.daumcdn.net/cfile/tistory/2519193C5715E03427](https://t1.daumcdn.net/cfile/tistory/2519193C5715E03427)

이제 단발 (DHCP client)이 DHCP 서버로 부터 IP 주소를 할당(임대)받는 절차에 대해서 알아보자. 

## DHCP 연결 과정

![Untitled](DHCP%20NAT%20bca71bf3798649218ed365866f332abb/Untitled%201.png)

### **1) DHCP 서버 발견 (DHCP Server Discover)**

새롭게 도착한 호스트는 자신이 접속할 네트워크의 DHCP 서버 주소를 알지 못한다.

따라서 서버 발견 메세지(포트67, UDP 메세지)를 **브로드캐스트 IP주소 (출발지 주소: 0.0.0.0. 목적지: 255.255.255.255.)**로 캡슐화하여 서브넷 상의 모든 노드로 전송한다. 

**[주요 파라미터]** 

- Client Mac : 단말의 MAC 주소

### **2) DHCP 서버 제공 (DHCP Server Offer)**

DHCP 발견 메세지를 받은 DHCP 서버는 DHCP 제공 메세지를 이용해 클라이언트로 브로드 캐스트 한다. 

**단말브로드캐스트 메시지** (Destination MAC = FF:FF:FF:FF:FF:FF)이거나 **유니캐스트**일수 있다. 

이는 단말이 보낸 DHCP Discover 메시지 내의 Broadcast Flag의 값에 따라 달라지는데, 이 Flag=1이면 DHCP 서버는 DHCP Offer 메시지를 Broadcast로, Flag=0이면 Unicast로 보내게 된다

서버 제공 메세지는 클라이언트에게 제공될 IP주소 네트워크 마스크, 도메인, 이름 , IP 주소 임대기간 등의 클라이언트 설정 파라미터 값들이 포함된다. 

**[주요 파라미터]**

- Client MAC: 단말의 MAC 주소
- Your IP: 단말에 할당(임대)할 IP 주소
- Subnet Mask (Option 1)
- Router (Option 3): 단말의 Default Gateway IP 주소
- DNS (Option 6): DNS 서버 IP 주소
- IP Lease Time (Option 51): 단말이 IP 주소(Your IP)를 사용(임대)할 수 있는 기간(시간)
- DHCP Server Identifier (Option 54): 본 메시지(DHCP Offer)를 보낸 DHCP 서버의 주소. 2개 이상의 DHCP 서버가 DHCP Offer를 보낼 수 있으므로 각 DHCP 서버는 자신의 IP 주소를 본 필드에 넣어서 단말에 보냄

### 3) DHCP 요청 (DHCP Request)

IP 할당을 요쳥한 새 호스트는 하나 이상의 DHCP 서버 제공 메세지를 받고 그 중 가장 최적의 서버를 선택한 후 그 서버측으로 DHCP 요청 메세지를 보낸디. 

**[주요 파라미터]**

- Client MAC: 단말의 MAC 주소
- Requested IP Address (Option 50): 난 이 IP 주소를 사용하겠다. (DHCP Offer의 Your IP 주소가 여기에 들어감)
- DHCP Server Identifier (Option 54): 2대 이상의 DHCP 서버가 DHCP Offer를 보낸 경우, 단말은 이 중에 마음에 드는 DHCP 서버 하나를 고르게 되고,  그 서버의 IP 주소가 여기에 들어감. 즉, DHCP Server Identifier에 명시된DHCP서버에게"DHCP Request"메시지를 보내어 단말 IP 주소를 포함한 네트워크 정보를 얻는 것

### 4) DHCP Ack (Acknowledgement)

서버는 DHCP 요청 메세지에 대해 요청된 설정을 확인하는 ACK 메세지를 전송한다. 

단말브로드캐스트 메시지 (Destination MAC = FF:FF:FF:FF:FF:FF) 혹은 유니캐스트일수 있으며 이는 단말이 보낸 DHCP Request 메시지 내의 Broadcast Flag=1이면 DHCP 서버는 DHCP Ack 메시지를 Broadcast로, Flag=0이면 Unicast로 보내게 된다.

**[주요 파라미터]**

- Client MAC: 단말의 MAC 주소
- Your IP: 단말에 할당(임대)할 IP 주소
- Subnet Mask (Option 1)
- Router (Option 3): 단말의 Default Gateway IP 주소
- DNS (Option 6): DNS 서버 IP 주소
- IP Lease Time (Option 51): 단말이 본 IP 주소(Your IP)를 사용(임대)할 수 있는 기간(시간
- DHCP Server Identifier (Option 54): 본 메시지(DHCP Ack)를 보낸 DHCP 서버의 주소

이렇게 DHCP Ack를 수신한 단말은 이제 IP 주소를 포함한 네트워크 정보를 획득(임대)하였고, 이제 인터넷 사용이 가능하게 된다.