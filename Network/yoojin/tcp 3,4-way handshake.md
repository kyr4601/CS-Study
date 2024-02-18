## **3-way Handshake**

**3-Way Handshake는 TCP/IP프로토콜을 이용해서 통신을 하는 응용프로그램이 데이터를 전송하기 전에 먼저 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미한다.**

TCP 3 way handshake를 간단히 표현하면 다음과 같다.

1. A -> B : 내 말 들려?
2. B -> A : 잘 들려. 내 말은 들려?
3. A -> B : 잘 들려!

![Untitled](../yoojin/img/TCP%203,4-Way%20Handshake_1.png)

- SYN (synchronize sequence numbers) - ***연결 설정***. 연결 확인을 위해 보내는 무작위의 숫자값 (내 말 잘 들려?)
- ACK (acknowledgements) - ***응답 확인***. Client 혹은 Server로부터 받은 **SYN에 1을 더해** SYN을 잘 받았다는 ACK (잘 들려)

**[STEP 1]**

**클라이언트는 서버와 커넥션을 연결하기 위해 SYN(x) 을 보낸다.**

**[STEP 2]**

**서버가 SYN(x)을 받고, 클라이언트로 받았다는 신호인 ACK(x+1)와 SYN(y) 패킷을 보낸다.**

**[STEP 3]**

**클라이언트는 서버의 응답은 ACK(x+1)와 SYN(y) 패킷을 받고, ACK(y+1)를 서버로 보낸다.**

<br>

## **4-way Handshake**

3 way handshake와 반대로 **연결을 해제**할 때 주고 받는 확인작업이다. 이 역시 4번의 확인과정을 거친다고 하여 4 way handshake라고 부른다.

TCP 4 way handshake를 간단히 표현하면 다음과 같다.

1. A -> B: 나는 다 보냈어. 이제 끊자!
2. B -> A: 알겠어! 잠시만~
3. B -> A: 나도 끊을게!
4. A -> B: 알겠어!

![Untitled](../yoojin/img/TCP%203,4-Way%20Handshake_2.png)

- FIN (Finish) - ***연결 해제***. TCP 연결을 종료하겠다는 메시지

**[STEP 1]**

클라이언트는 서버에게 연결을 종료한다는 **FIN** 플래그를 보낸다.

**[STEP 2]**

서버는 FIN 플래그를 받고, 일단 확인했다는 **ACK**를 클라이언트에게 보내고 **모든 데이터를 보낼 때까지 기다리는 `CLOSE_WAIT` 상태**에 들어간다.

→ **Client가 데이터 전송을 마쳤다고 하더라도 Server는 아직 보낼 데이터가 남아있을 수 있기 때문에**

**[STEP 3]**

서버가 **데이터를 모두 보냈다면,** 서버는 연결 종료에 합의 한다는 의미로 **FIN** 패킷을 클라이언트에게 보낸다.

**[STEP 4]**

- 클라이언트는 FIN을 받고, 확인했다는 **ACK**를 서버에게 보낸다.
- 클라이언트는, **아직 서버로부터 받지 못한 데이터가 있을 수 있으므로,** `TIME_WAIT`을 통해 기다린다.
    
    → **TIME_WAIT** 시간이 끝나면, `CLOSED` 상태가 된다.