# 네트워크 2주차

https://hyper-hotel-11e.notion.site/2-6287a99bb3984c248c4823c0a4c012ed?pvs=4

# 📌TCP

## UDP

UDP에도 오류를 감지하는 기능은 있다. ⇒ Checksum으로.

근데 감지만 하고, TCP와 달리 오류를 정정하거나 재전송하는 메커니즘을 제공하진 않는다.

정정안해줄거면 왜 굳이 감지를 해줄까?

⇒ 과도한 체크섬 오류는 네트워크 장비의 문제나 데이터 경로상의 문제를 나타내줌.

⇒ Application layer에서 필요하면 선택적으로 재요청하게끔 적어도 알려는 준다는 느낌

## TCP

네트워크 통신에서 신뢰적인 연결방식을 사용한다는 특징이 있다.

TCP는 기본적으로 unreliable network에서, reliable network (reliable data transfer)를 보장할 수 있도록 하는 프로토콜이다.

TCP 통신을 하다보면 Segment 전송에 에러가 생기는 경우가 있다. 

- Loss - 손실 문제
- Corruption - bit error
- Congestion - 네트워크 혼잡 문제

Solution ⇒ ACK, Checksum, Time-out, Retransmission, Seq #

TCP는 이 에러들을 정정해줘야하고, 흐름과 혼잡을 제어해야한다. 왜냐하면 신뢰적인 연결방식을 사용하는 프로토콜이니까. TCP 프로토콜의 흐름제어와 혼잡제어 방식을 이해하기위해서는 몇가지 용어들과 과정들을 알아야한다. 

### ACK

ACK(Acknowledgements) : 리시버가 센더에게 OK 잘 왔음이라고 답해주는 것

ack은 리시버가 센더에게 잘 받았다고 전달해주는 메시지 개념인데, 이 ack 또한 센더한테 전달될때 에러가 날 수 있음. 이때 센더입장에서는 “못 받았나보네, 다시 보내야지” 하면서 첫번째 Segment를 다시 보냄. 그러면 리시버 입장에서는 첫번째 segment를 받은 상태이기 때문에 위 센더가 다시 보낸 값을 두번째 segment로 인식하게 됨. 이러한 문제를 해결하기 위해 Sequence Number 개념이 탄생.

sequence number로 전달할 segment의 순서를 구분하게됨.

### TCP Header

https://blog.kakaocdn.net/dn/b2OtbA/btqAMlJTLi1/CTJcSSlZ7qzxlBrmgc5Pb1/img.png

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/1eec8832-fa93-4c7d-9472-9f11a4a4a946/34117a49-c5ac-445a-abef-b88fec44ee73/Untitled.png)

TCP flag(URG, ACK, PSH, RST, SYN, FIN)

FLAG 순서  - | URG  | ACK | PSH | RST | SYN | FIN |

### ACK(Acknowledgement) - Ack : 응답

상대로부터 패킷을 받았다는 걸 알려주는 패킷

### SYN(Synchronization:동기화) - S : 연결 요청 플래그

TCP 에서 세션을 성립할 때  가장먼저 보내는 패킷, 시퀀스 번호를 임의적으로 설정하여 세션을 연결하는 데에 사용되며 초기에 시퀀스 번호 (ISN) 를 보내게 된다.

### FIN(Finish) - F : 종료

연결 종료 요청세션 연결을 종료시킬 때 사용. 더이상 전송할 데이터가 없음을 나타냄

### RST(Reset) - R : 리셋

연결 종료재설정(Reset)을 하는 과정, 양방향에서 동시에 일어나는 중단 작업

비 정상적인 세션 연결 끊기. 현재 접속하고 있는 곳과 즉시 연결을 끊고자 할 때 사용

## 🔸TCP 3 way handshaking (연결)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/1eec8832-fa93-4c7d-9472-9f11a4a4a946/5861c431-00b6-44f2-be31-4538d7856090/Untitled.png)

서버 listen() 상태

1. 클라이언트 connect(), C->S 접속요청 패킷 SYN(M) 전송

2. 서버 accept(), S->C 요청 수락 패킷 ACK(M+1), SYN(N) 전송

3. 클라이언트 connect(), C->S 요청 수락 확인 패킷 ACK(N+1) 전송

### 연결 성립

## 🔸TCP 4 way handshaking (해제)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/1eec8832-fa93-4c7d-9472-9f11a4a4a946/29872917-61f8-4440-b440-d86cc8187d3c/Untitled.png)

1. C->S 연결 종료요청 FIN FLAG 전송

2. S->C 종료 수락 메시지 ACK 전송

일시적 TIME_OUT (데이터를 모두 보낼 때 까지)

3. S->C 연결 종료 FIN FLAG 전송

4. C->S 확인 메시지 ACK 전송

서버 소켓 연결 close(), 클라이언트 일정 시간 동안 TIME_WAIT (잉여패킷 대기)

### 연결 해제

## 🔸TCP 흐름제어

 **Window** : 윈도우는 메모리 버퍼의 일정 영역이라고 생각하면 된다. TCP/IP를 사용하는 모든 호스트들은 송신하기 위한 것과 수신하기 위한 2개의 Window를 가지고 있다. 호스트들은 실제 데이터를 보내기 전에 '3 way handshaking'을 통해 리시버의 receive window size에 자신의 send window size를 맞추게 된다.

### **TCP Buffer**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/1eec8832-fa93-4c7d-9472-9f11a4a4a946/29b4ffdd-d8e8-41b4-a234-6920e88302b6/Untitled.png)

흐름 제어는 송신 측과 수신 측의 TCP 버퍼 크기 차이로 인해 생기는 데이터 처리 속도 차이를 해결하기 위한 기법이다.

수신측에서 제한된 저장 용량을 초과한 이후에 도착하는 데이터는 손실 될 수 있으며, 만약 손실 된다면 불필요하게 응답과 데이터 전송이 송/수신 측 간에 빈번이 발생한다.

이러한 위험을 줄이기 위해 송신 측의 데이터 전송량을 수신측에 따라 조절해야한다.

흐름제어기법

### 1. stop and wait

매번 전송한 패킷에 대해 확인 응답(ACK)를 받으면 다음 패킷을 전송하는 방법이다. 그러나 패킷을 하나씩 보내기 때문에 비효율적인 방법이다.

⇒ 파이프라이닝 형식을 사용해 효율성을 높임 

**슬라이딩 윈도우** 

- Go-back-N (슬라이딩 윈도우 방식을 이용한 오류제어 기법)
- Selevtive Repeat  (슬라이딩 윈도우 방식을 이용한 오류제어 기법)

### 2. Sliding Window

리시버는 rwnd 값을 포함한 tcp header를 센더에게 전달되는 segment에 담아 보내기 때문에 센더는 리시버의 rwnd값에 따라 in-flight 값을 제한할 수 있다. 

window size만큼 순서대로 모두 보내고, ack이 오면 그만큼 윈도우 이동하는 방식

## 🔸TCP 혼잡제어

센더의 데이터는 지역망이나 인터넷으로 연결된 대형 네트워크를 통해 전달된다. 만약 데이터가 한 곳으로 몰릴 경우, 자신에게 온 데이터를 모두 처리할 수 없게 된다. 이런 경우 혼잡으로 인한 오버플로우나 데이터 손실이 발생한다.

따라서 이러한 네트워크의 혼잡을 피하기 위해 센더측에서 보내는 데이터의 전송속도를 강제로 줄이게 되는데, 이러한 작업을 혼잡제어라고 한다.

TCP Tahoe와 Reno 등 혼잡 제어 정책들은 공통적으로 `혼잡이 발생하면 윈도우 크기를 줄이거나, 혹은 증가시키지 않으며 혼잡을 회피한다` 라는 전제를 깔고 있다.

TCP Tahoe와 Reno 정책을 알아보기전에 몇가지 혼잡제어 방식과 관련 용어들을 알아야한다.

1. **Timeout**

여러 가지 요인으로 인해 센더가 보낸 데이터 자체가 유실되었거나, 

리시버가 응답으로 보낸 ACK이 유실되는 경우 센더가 설정해놓은 timeout에 걸리게 된다.

1. **3 Duplicate ACKs**

!https://blog.kakaocdn.net/dn/ci8GSU/btrlpLaCuUk/KHpoUNv1tRnEHPzOtXd4Gk/img.png

 중복 ACK 3개를 받으면 문제가 있다고 판단하여 해당 패킷을 재전송한다. 해당 기법을 `빠른 재전송` 이라고 부르며, 센더는 자신이 설정한 타임 아웃 시간이 지나지 않았어도 바로 해당 패킷을 재전송할 수 있기 때문에 보다 빠른 전송률을 유지할 수 있게 된다.

1. ****AIMD(Additive Increase / Multiplicative Decrease)****
    - 문제가 발생하기 전까지 cwnd 1씩 증가
    - 문제가 발생하면 cwnd를 절반으로 감소
    
     문제점은 초기에 네트워크의 높은 대역폭을 사용하지 못하여 오랜 시간이 걸리게 되고, 네트워크가 혼잡해지는 상황을 미리 감지하지 못한다. 즉, 네트워크가 혼잡해지고 나서야 대역폭을 줄이는 방식이다.
    
2. ****Slow Start****
    - Slow Start 방식은 AIMD와 마찬가지로 패킷을 하나씩 보내면서 시작하고, 패킷이 문제없이 도착하면 각각의 ACK 패킷마다 window size를 1씩 늘려준다. 즉, 한 주기가 지나면 window size가 2배로 된다.
    - 전송속도는 AIMD에 반해 지수 함수 꼴로 증가한다. 대신에 혼잡 현상이 발생하면 window size를 1로 떨어뜨리게 된다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/1eec8832-fa93-4c7d-9472-9f11a4a4a946/f226d3a2-bba1-4619-b011-78d442888372/Untitled.png)
    

**Slow Start 임계점 (ssthresh)**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/1eec8832-fa93-4c7d-9472-9f11a4a4a946/33cc15d4-ea94-42cc-89e1-7ce8e203dfdc/Untitled.png)

 `Slow Start Threshold(ssthresh)` 여기까지만 Slow Start를 사용하겠다는 의미를 가진다.

Slow Start를 사용하며 윈도우 크기를 지수적으로 증가시키다보면 어느 순간부터는 윈도우 크기가 기하급수적으로 늘어나서 제어하기가 힘들다. 또한, 네트워크의 혼잡이 예상되는 상황에서 빠르게 값을 증가시키기 보다는 조금씩 증가시키는 편이 훨씬 안전하다.

그래서 특정한 임계점을 정해 놓고, 그 임계점이 넘어가면 AIMD 방식을 사용하여 선형적으로 윈도우를 증가시킨다. 

(이 임계점은 계속 초기값을 가지는 건 아님. loss event가 발생하면 그 발생했을때의 cwnd의 절반 사이즈로 세팅됨)

1. **Fast Recovery**

혼잡한 상태가 되면 window size를 1이 아닌 반으로 줄이고 선형증가시키는 방법이다. 이 정책까지 적용하면 혼잡 상황을 한번 겪고 나서부터는 순수한 AIMD 방식으로 동작하게 된다.

### TCP Tahoe

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/1eec8832-fa93-4c7d-9472-9f11a4a4a946/419d14d4-aa3d-4516-9f93-e506c880e70b/Untitled.png)

이 정책은 한 번 혼잡 상황이 발생한 지점을 기억하고 그 지점이 가까워지지 않도록 합리적으로 조절하고 있다. 하지만, 초반의 Slow Start 구간에 윈도우 크기를 늘릴 때 오래 걸린다는 단점이 있고, 혼잡 상황이 발생했을 때 다시 윈도우 크기를 1에서부터 시작해야 한다는 단점이 있다.

### TCP Reno

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/1eec8832-fa93-4c7d-9472-9f11a4a4a946/88c472f3-11f0-47c8-980e-f2d5ec5ad093/Untitled.png)

TCP Reno는 TCP Tahoe 이후에 나온 정책으로, Tahoe와 마찬가지로 Slow Start로 시작하여 임계점을 넘어서면 AIMD을 사용한다. 다만, Tahoe와는 다르게 3 ACK Duplicated와 Timeout 혼잡 상황을 구분한다.

Reno는 3개의 중복 ACK가 발생했을 때, 윈도우 크기를 1로 줄이는 것이 아니라 AIMD처럼 반으로만 줄이고 sshthresh를 줄어든 윈도우 값으로 정하게 된다. 이 방식을 `빠른 회복`이라고 부른다.

그러나 Timeout에 의해서 데이터가 손실되면 Tahoe와 마찬가지로 윈도우 크기를 바로 1로 줄여버리고 Slow Start를 진행한다. 이때 ssthresh를 변경하지는 않는다.

즉, Reno는 ACK 중복은 Timeout에 비해 그리 큰 혼잡이 아니라고 가정하고 혼잡 윈도우 크기를 1로 줄이지도 않는다는 점에서 혼잡 상황의 우선 순위를 둔 정책이라 볼 수 있다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/1eec8832-fa93-4c7d-9472-9f11a4a4a946/fce15f46-875e-4d18-b56a-e9763bdee438/Untitled.png)

---

# 📌IP (Internet Protocol)

> 인터넷 프로토콜의 약자로 송신 호스트와 수신 호스트가 패킷 교환 네트워크에서 정보를 주고받는데 사용하는 정보 위주의 규약
> 

- 인터넷에서 데이터를 송수신하기 위한 주요 프로토콜
- 데이터를 소스에서 목적지까지 전달
- 비신뢰성과 비연결성
- 패킷 전송과 정확한 순서를 보장하기 위해 TCP 프로토콜을 이용

**고정 IP**

- 컴퓨터가 고정적으로 가지고 있는 IP이다.
- 한 번 부여받으면 **반납하기 전까지 변하지 않는다**.
- 주로 인터넷 사이트를 운영할 때 사용한다.

**유동 IP**

- 컴퓨터에 고정된 IP를 부여하지 않고 **IP 갱신 주기가 되었을 때 ISP로부터 할당받는 IP**이다.
- 사용하지 않는 IP를 수거하는 방식이다.
    - 더 많은 사용자에게 인터넷 서비스를 제공할 수 있다.

**공인 IP**

- **외부에 공개된 IP**다.
- 해킹의 위험이 있기에 보안 프로그램이 필요하다.

**사설 IP**

- **외부에서 접근할 수 없는 IP**다.
- 주로 일반 가정 또는 회사 내부에서 사용한다.
- 공인 IP가 할당된 라우터나 공유기를 통해 사설 IP가 할당된다.

## IPv4

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/1eec8832-fa93-4c7d-9472-9f11a4a4a946/ce4aebb6-a3a8-4a18-8d4f-4aca4b71e1b0/Untitled.png)

IPv4 주소는 전세계적으로 관리되는 32bit의 유효한 자원으로 8 bits 씩 나눠져서 일부는 Network, 다른 일부는 Host의 번호를 표현하는데 사용된다. 네트워크 부분은 특정 네트워크를 식별하고, 호스트 부분은 그 네트워크 내의 특정 장치를 식별한다.

- 주소는 점으로 구분된 네 개의 숫자(예: 192.168.0.1)로 표현
- 인터넷 상에서 장치를 식별하는 데 사용 ⇒ IPv4 주소는 고유해야 함

### IPv4  주소 고갈 & 대응 방안

원인 : 인터넷의 지속적인 성장과 연결되는 장치의 증가

IPv4 주소는 32비트로 구성되어 있어, 이론적으로 약 42억 개(2의 32승)의 고유한 주소만 제공 가능

**대응 방안**

 (IPv6가 근본적인 주소 고갈 대응 방안이라고 할 수 있고, 나머지는 IPv4 주소 공간을 보다 효율적으로 관리하고 활용하기 위한 임시 방안이라고 할 수 있다.)

1. Subnetting & CIDR
2. NAT
3. DHCP
4. IPv6

## IPv6

IPv4의 고갈 문제를 해결하기 위해 IPv6가 등장하였다.

총 128비트로 각 16비트씩 8자리로 각 자리는 ‘:’(콜론)으로 구분한다.

ex) 2001:0DB8:1000:0000:0000:0000:1111:2222

총 2^128 의 IP 주소를 만들어낸다.

### IPv4와의 차이점

---

1. **주소 체계 및 길이**

- **IPv4**:
    - 32비트 주소 체계
    - 주소는 점으로 구분된 십진수 형식으로 표시 / 예: 192.168.1.1
- **IPv6**:
    - 128비트 주소 체계를 사용하여, 거의 무한에 가까운 주소 공간을 제공
    - 주소는 콜론으로 구분된 16진수 형식으로 표시됩니다. / 예: 2001:0db8:85a3:0000:0000:8a2e:0370:7334
    

**2. 주소 할당 및 관리**

- **IPv4**:
    - 정적(수동) 또는 동적(DHCP) 방식으로 주소 할당
    - 주소 공간의 제한으로 인해 주소 할당을 효율적으로 해야한다.
- **IPv6**:
    - 주소의 풍부함으로 인해 주소 할당에 있어 더 많은 유연성을 가짐
    

**3. 보안**

- **IPv4**:
    - 기본적으로 보안 기능이 내장되어 있지 않습니다.
    - 보안을 위해 IPsec과 같은 별도의 프로토콜을 사용할 수 있으나, 선택적입니다.
    
    (IPsec은 Host-to-Host와 Network-to-Network간의 Data Flow를 암호화 시키는 Protocol Suite)
    
- **IPv6**:
    - 보안을 위한 IPsec이 기본적으로 내장되어 있어, 보다 향상된 보안 기능을 제공합니다.
    
    (참고로, 초창기 모든 IPv6는 IPsec를 필수적으로 지원하게 되어 있어 IPv6 Network의 높은 보안성을 기대하였으나, 낮은 Network Processor를 가진 IPv6 Nodes로 인하여 IPsec의 기능이 "Recommended" 수준으로 하향 조정 되었다. 하지만, IPsec이 "Option"으로 지원이 되는 IPv4 Network에 비하여 IPv6 Network이 여전히 높은 보안성을 가진다.)
    

**4. 헤더 형식 및 처리**

- **IPv4**:
    - 헤더가 상대적으로 복잡하고 옵션 필드가 있어 처리가 더 복잡함
    - 헤더 길이가 가변적

- **IPv6**:
    - 헤더 구조가 간소화되어 있고, 대부분 고정 길이
    - checksum x (DataLink Layer에서 오류를 검사하기 때문에 IP 패킷의 빠른 처리를 위해 삭제)
    

**5. fragmentation 단편화**

Fragmentation은 데이터 패킷을 네트워크를 통해 전송할 때 발생하는 과정으로, 큰 데이터 패킷을 더 작은 단위로 나누는 것

왜? 

MTU라는 한 번에 전송할 수 있는 데이터의 최대 크기를 나타내는 단위가 있는데, 네트워크 경로상 각 네트워크 세그먼트의  MTU보다 보내려는 데이터 패킷 크기가 크다면 그 패킷은 네트워크를 통과할 수 없다. 따라서 데이터 패킷은 MTU 크기에 맞게 여러 개의 작은 패킷으로 분할되어야 한다.

- **IPv4**:
    - 출발/목적지 또는 중간 라우터에서 수행
- **IPv6**:
    - 송신자만 수행하며, 중간 라우터에서는 수행x
        
        단편화 & 재결합 과정은 시간이 소요되는 일이므로 라우터에서 수행을 하지 않음으로써 네트워크 효율성이 증가
        

---
<br>
<br>

# 면접질문

## TCP의 등장을 IP와 연결지어 설명하라.
    
    
    IP는 데이터가 소스에서 목적지까지 도달하는 것만을 책임지며, 데이터의 순서, 무결성, 신뢰성 등에 대해서는 보장하지 않습니다.
    IP에서 오류가 발생한다면 ICMP에서 오류가 발생했음을 알려주긴 하지만 ICMP는 알려주기만 할 뿐 대처를 해주지 않기 때문에 IP보다 위에서 오류에 대한 처리를 해줘야 합니다. 즉 이를 Transport layer에서 해결을 해주어야합니다.
    
    따라서, TCP는 IP의 기능을 보완하고, 다양한 네트워크 애플리케이션과 서비스의 요구 사항을 충족시키기 위해 도입되었습니다.
    
    - ICMP : 인터넷 제어 메시지 프로토콜로 네트워크 컴퓨터 위에서 돌아가는 운영체제에서 오류 메시지를 전송받는데 주로 쓰임.  ICMP는 문제를 알리지만, 문제 해결은 해주지 않는다.