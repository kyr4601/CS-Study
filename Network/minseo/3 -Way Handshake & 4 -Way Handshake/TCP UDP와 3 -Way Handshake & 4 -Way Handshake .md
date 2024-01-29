# TCP/UDP와 3 -Way Handshake & 4 -Way Handshake

### TCP 프로토콜 연결/종료 과정

![Untitled](TCP%20UDP%E1%84%8B%E1%85%AA%203%20-Way%20Handshake%20&%204%20-Way%20Handshake%209142337603cb423a833662a2558b2cc3/Untitled.png)

```c
int main(){
	sock = socket(AF_INFT, SOCK_STREAM, 0);
    ..
    bind( ~)
    ..
    listen()
    ..
    new_sock = accept(sock~~)
    ..
    read(new_socket,
    ..
    write(
    ..
    close
    close

}
```

**서버** 

socket() 생성 → bind() 소켓 주소할당 → listen() 연결요청 대기상태 → accept() 연결허용 → read/write() 데이터 송수신 → close() 연결종료

**클라이언트** 

socket() 생성 → connect() 연결요청 → read/write() 데이터 송수신 → close() 연결종료

## 3-way handshaking (연결 과정)

![Untitled](TCP%20UDP%E1%84%8B%E1%85%AA%203%20-Way%20Handshake%20&%204%20-Way%20Handshake%209142337603cb423a833662a2558b2cc3/Untitled%201.png)

**3-way handshake란 ?**

- **TCP를 이용한 데이터 통신을 할 때 프로세스와 프로세스를 연결하기 위해 가장 먼저 수행되는 과정**
1. 클라이언트가 서버에게 요청 패킷을 보내고
2. 서버가 클라이언트의 요청을 받아들이는 패킷을 보내고
3. 클라이언트는 이를 최종적으로 수락하는 패킷을 보낸다. 

### A 프로세스(Client)가 B 프로세스(Server)에 연결을 요청

✅ 서비 listen() 상태 

**a. A -> B: SYN (접속 요청 패킷)**

- 접속 요청 프로세스 A가 연결 요청 메시지 전송 (SYN)
- 송신자가 최초로 데이터를 전송할 때 Sequence Number를 임의의 랜덤 숫자로 지정하고, SYN 플래그 비트를 1로 설정한 세그먼트를 전송한다.
- PORT 상태 - B: LISTEN, A: CLOSED

**b. B -> A: SYN + ACK (요청 수락 패킷)**

- 접속 요청을 받은 프로세스 B가 요청을 수락했으며, 접속 요청 프로세스인 A도 포트를 열어 달라는 메시지 전송 (SYN + ACK)
- 수신자는 Acknowledgement Number 필드를 (Sequence Number + 1)로 지정하고, SYN과 ACK 플래그 비트를 1로 설정한 세그먼트를 전송한다.
- PORT 상태 - B: SYN_RCV, A: CLOSED

**c. A -> B: ACK (요쳥 수락 확인 패킷)**

- PORT 상태 - B: SYN_RCV, A: ESTABLISHED
- 마지막으로 접속 요청 프로세스 A가 수락 확인을 보내 연결을 맺음 (ACK)
- 이때, 전송할 데이터가 있으면 이 단계에서 데이터를 전송할 수 있다.
- PORT 상태 - B: ESTABLISHED, A: ESTABLISHED

✅  연결 성립 (Established)

## 4-way handshaking (연결 해제 과정)

![https://velog.velcdn.com/images%2Fragnarok_code%2Fpost%2F1804bed2-b0e2-4b0b-b622-84ea78c6fb96%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202021-11-21%20%EC%98%A4%ED%9B%84%202.36.17.png](https://velog.velcdn.com/images%2Fragnarok_code%2Fpost%2F1804bed2-b0e2-4b0b-b622-84ea78c6fb96%2F%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202021-11-21%20%EC%98%A4%ED%9B%84%202.36.17.png)

**4-way handshake란 ?**

- TCP의 연결을 해제(Connection Termination)하는 과정

A 프로세스(Client)가 B 프로세스(Server)에 연결 해제를 요청

✅ Established  상태 

**a. A -> B: FIN (연결 종료 요청 FIN FLAG 전송)**

- 프로세스 A가 연결을 종료하겠다는 FIN 플래그를 전송
- 프로세스 B가 FIN 플래그로 응답하기 전까지 연결을 계속 유지

**b. B -> A: ACK (종료 수락 메세지 ACK 전송)**

- 프로세스 B는 일단 확인 메시지를 보내고 자신의 통신이 끝날 때까지 기다린다.(이 상태가 TIME_WAIT 상태)
- 수신자는 Acknowledgement Number 필드를 (Sequence Number + 1)로 지정하고, ACK 플래그 비트를 1로 설정한 세그먼트를 전송한다.
- 그리고 자신이 전송할 데이터가 남아있다면 이어서 계속 전송한다.

**c. B -> A: FIN (연결 종료 FIN FLAG 전송)**

- 프로세스 B가 통신이 끝났으면 연결 종료 요청에 합의한다는 의미로 프로세스 A에게 FIN 프래그를 전송

**d. A -> B: ACK (확인 메세지 ACK 전송)**

- 프로세스 A는 확인했다는 메시지를 전송

✅ 서버 소켓 연결 close(), 클라이언트 일정 시간 동안 TIME_WAIT (잉여패킷 대기)

> **포트(PORT) 상태 정보**
> 
> - CLOSED: 포트가 닫힌 상태
> - LISTEN: 포트가 열린 상태로 연결 요청 대기 중
> - SYN_RCV: SYNC 요청을 받고 상대방의 응답을 기다리는 중
> - ESTABLISHED: 포트 연결 상태

### TCP flag(URG, ACK, PSH, RST, SYN, FIN)

FLAG 순서  - | URG  | ACK | PSH | RST | SYN | FIN |

각각 1비트로 TCP 세그먼트 필드 안에 CONTROL BIT 또는 FLAG BIT 로 정의 되어 있다.

> **Flag Bit**
> 
> - URG: 긴급 위치 필드 유효 여부 설정
> - ACK: 응답 유효 여부 설정. 최초의 SYN 패킷 이후 모든 패킷은 ACK 플래그 설정 필요. 데이터를 잘 받았으면 긍정 응답으로 ACK(=SYN + 1) 전송
> - PSH: 수신측에 버퍼링된 데이터를 상위 계층에 즉시 전달할 때
> - RST: 연결 리셋 응답 혹은 유효하지 않은 세그먼트 응답
> - SYN: 연결 설정 요청. 양쪽이 보낸 최초 패킷에만 SYN 플래그 설정,  Sequence Number를 랜덤으로 설정하여 세션을 연결하는 데 사용하며, 초기에 Sequence Number를 전송한다.
> - FIN: 연결 해제. 세션 연결을 종료시킬 때 사용되며, 더 이상 전송할 데이터가 없음을 의미한다.

# cs 면접 질문

### Q1: TCP의 연결 설정 과정(3단계)과 연결 종료 과정(4단계)이 단계가 차이나는 이유 ?

- Client가 데이터 전송을 마쳤다고 하더라도 Server는 아직 보낼 데이터가 남아있을 수 있기 때문에 일단 FIN에 대한 ACK만 보내고, 데이터를 모두 전송한 후에 자신도 FIN 메시지를 보내기 때문이다.

### Q2: 만약 Server에서 FIN 플래그를 전송하기 전에 전송한 패킷이 Routing 지연이나 패킷 유실로 인한 재전송 등으로 인해 FIN 패킷보다 늦게 도착하는 상황이 발생하면 어떻게 될까?

- 이러한 현상에 대비하여 Client는 Server로부터 FIN 플래그를 수신하더라도 일정시간(Default 240sec)동안 세션을 남겨 놓고 잉여 패킷을 기다리는 과정을 거친다. (TIME_WAIT 과정)

### Q3: 초기 Sequence Number인 ISN을 0부터 시작하지 않고 난수를 생성해서 설정하는 이유?

- Connection을 맺을 때 사용하는 포트(Port)는 유한 범위 내에서 사용하고 시간이 지남에 따라 재사용된다. 따라서 두 통신 호스트가 과거에 사용된 포트 번호 쌍을 사용하는 가능성이 존재한다. 서버 측에서는 패킷의 SYN을 보고 패킷을 구분하게 되는데 난수가 아닌 순차적인 Number가 전송된다면 이전의 Connection으로부터 오는 패킷으로 인식할 수 있다. 이런 문제가 발생할 가능성을 줄이기 위해서 난수로 ISN을 설정한다.