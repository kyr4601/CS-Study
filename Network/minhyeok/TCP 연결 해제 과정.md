# TCP 연결 해제 과정
### TCP란?
- Transprot Layer의 프로토콜
- 연결지향적
- 신뢰성이 높음
- 패킷 전달이 순서적, 안정적, 에러 없이 교환하는 것이 목표

## 3-Way-Handshake (연결 과정)
   - 데이터를 주고 받기 전에 안정적으로 데이터를 주고 받을 수 있을 지 클라이언트와 서버측 간 세션을 수립하는 과정

   <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FZR8xW%2FbtqUOBZHfwT%2FjlvqPUKsvCZ2RKi2eieA5k%2Fimg.jpg" width="700" height="500">

   ### Flag 정보
   - SYN(연결 설정) : 세그먼트로 나누어진 패킷의 순서 // 초기에 랜덤값으로 보내짐 
   - ACK(응답 확인) : 수신자가 성공적으로 값을 받은 것을 알리기 위해 사용
   - FIN(연결 해제) : 세션 연결 종료시 사용. 전송할 데이터가 없음을 알림

   ### 과정

   1. Client => Servcer 
   - tcp 헤더 안에 syn패킷에 (클라이언트 seq 임의의 숫자)을 보낸다

   2. Server => Client 
   server는 이를 받고 syn과 ack패킷을 보낸다. ack으로 클라이언트의 seq에 +1을 한 값을 보내고, 서버 seq을 보낸다.

   3. Client => Server
   응답으로 ack패킷을 보내고 server로 부터 받은 seq에 1을 더한 값을 보낸다.

## 4-Way-Handshake (해제 과정)
   - 세션을 종료하기 위해 수행되는 과정

   <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fd6DVuS%2FbtqF5zIQmf6%2FaAJneDBBDp2qkRgnYrepKk%2Fimg.png" width="700" height="500">

1. Client => Server
- close()을 호출하여 FIN패킷 보냄

2. Server => Client
- ACK패킷 보내고, 남은 데이터를 마저 보낸다.

3. Server => Client
- 잔여 데이터를 모두 보낸 뒤 close() 호출하여 FIN 패킷을 보낸다

4. Client => Server
- FIN 패킷을 받고, 아직 다 오지 않은 데이터를 받기 위해 TIME_WAIT 상태 유지(기본 240초)
- 240초 뒤에 닫음

## 질문
Q. TCP 연결 후 3-way-handshake 를 주고 받는 과정에서 seq 번호를 랜덤으로 설정하는 이유는?

A. 첫번째 이유로는 이전 연결에서 재전송되거나, 지연된 패킷이 현재 연결에 혼돈을 줄 수 있기 때문입니다. 두번째는 보안상의 이유입니다. seq 번호가 예측이 가능하다면 공격자는 이를 이용하여 tcp연결을 하이재킹하거나 가짜 패킷을 삽입할 수 있습니다.

but 한번 연결된 이후로는 seq 번호가 순차적으로 증가