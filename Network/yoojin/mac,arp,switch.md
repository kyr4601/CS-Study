## MAC 주소

MAC 주소는 Media Access Control Address(물리주소)의 약어다.

MAC 주소는 48비트 숫자로 구성되있고, 앞쪽 24비트는 랜카드를 만든 제조사 번호 뒤쪽 24비트는 제조사가 랜 카드에 붙인 일련번호다.

**MAC 주소**는, 제조할 때 새겨지기에 **물리주소**라고도 부르는데 **전 세계에서 유일한 번호**로 할당하려고 노력하지만 종종 MAC 주소 충돌 문제가 발생한다. 

**MAC 주소를 사용한 통신**

OSI 모델의 데이터 링크 계층에서 **이더넷 헤더**와 **트레일러**를 붙인다.

**이더넷 헤더**는 목적지의 MAC주소(6바이트), 출발지 MAC 주소(6바이트), 유형(2바이트) 총 14바이트로 구성되있다.

**이더넷 유형(Ethernet type)**은 이더넷으로 전송되는 상위 계층 프로토콜의 종류를 나타낸다.

**트레일러**는 **FCS(Frame Check Sequence)**라고도 하는데, 데이터 전송 도중에 오류가 발생하는지 확인하는 용도로 사용한다.
 

## 스위치의 구조

### 1. MAC주소 테이블이란?

**스위치**는, **데이터 링크 계층**에서 동작하고, **레이어 2 스위치** 또는 **스위칭 허브**라고도 불린다. 

**스위치** 내부에는 **MAC 주소 테이블(MAC address table)**이 있다. **MAC 주소 테이블**은, 스위치의 **포트 번호**와 해당 포트에 연결되어 있는 컴퓨터의 **MAC 주소**가 등록되는 데이터 베이스다.

**스위치**의 전원을 켰을때, **MAC 주소 테이블**에는 **아무것도 등록되어 있지 않다.** 하지만, 컴퓨터에서 **목적지 MAC 주소가 추가된 프레임이 전송되면,** MAC 주소 테이블을 확인하고 **출발지 MAC 주소**가 등록되어 있지 않으면, **MAC 주소를 포트와 함께 등록**한다. 이를 **MAC 주소 학습 기능**이라고 한다. 

**예를 들어 스위치에 컴퓨터 A, B, C, D, E가 연결 되있다고 하자.**
나는 스위치를 처음키고 컴퓨터 A에서 컴퓨터 D로 데이터를 전송하고 싶다. 근데 컴퓨터 D의 목적지 MAC 주소가 MAC 주소 테이블에 등록되어 있지않아서 컴퓨터 D뿐 만 아니라 컴퓨터 B, C, D, E 모두에게 데이터(프레임)을 전송하는데, 이 전송을 플러딩(flooding, 홍수)라고 한다.
이 플러딩을 거치고 데이터가 컴퓨터 D에 도착하면 목적지 MAC 주소를 확인하여 MAC 주소 테이블에 컴퓨터 D 의 MAC 주소를 등록한다.

만약 컴퓨터 A가 또 컴퓨터 D에게 데이터를 전송한다면 MAC 주소 테이블에 컴퓨터 D의 MAC 주소가 등록되어 있으므로 플러딩 과정 없이 바로 컴퓨터 D에게만 데이터가 전송된다. 이것을 MAC 주소 필터링이라고 한다.


## ARP

**ARP(Address Resolution Protocol)**는 목적지 컴퓨터의 **IP 주소**를 이용하여, **MAC 주소**를 찾기 위한 프로토콜이다. 

이더넷 프레임을 전송하려면 목적지 컴퓨터의 MAC 주소를 알아야 하는데, 출발지 컴퓨터가 목적지 MAC 주소를 알아내기 위해 **네트워크에 브로드캐스트**하는데 이것을 **ARP 요청(ARP Request)**라고 한다. 

이 요청에 대해 지정된 IP 주소를 가지고 있지 않은 컴퓨터는 응답하지 않고, 지정된 IP 주소를 가지고 있는 컴퓨터는 MAC 주소를 응답으로 보낸다. 이것을 **ARP 응답(ARP Response)**라고 한다. 이 때에는 **유니캐스트**를 사용한다. 

ARP 응답으로 출발지 컴퓨터는 **MAC 주소**를 얻게 되고 이더넷 프레임을 만들 수 있게 된다. 또한, 출발지 컴퓨터는 **MAC 주소와 IP 주소의 매칭 정보를 메모리에 보관**하는데 이를 **ARP 테이블(ARP Table)**이라고 한다. 이후 데이터 통신은 자신의 컴퓨터에 보관된 ARP 테이블을 참고하여 전송한다.

하지만 **IP 주소가 변경되면 해당 MAC 주소도 함께 변경되므로** 제대로 통신할 수 없다. 따라서 ARP 테이블에서는, **보존 기간을 ARP 캐시로 지정하고 일정 기간이 지나면 삭제하고 다시 ARP 요청**을 한다.