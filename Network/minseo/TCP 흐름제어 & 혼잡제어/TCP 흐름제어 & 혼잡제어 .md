# TCP 흐름제어 & 혼잡제어

# **들어가기 전**

# **TCP 통신이란?**

- 네트워크 통신에서 신뢰적인 연결방식
- TCP는 기본적으로 unreliable network에서, reliable network를 보장할 수 있도록 하는 프로토콜

### 그러나 **reliable network를 보장한다는 것은 4가지 문제점 존재한다.**

1. **손실** : packet이 손실될 수 있는 문제
2. **순서 바뀜** : packet의 순서가 바뀌는 문제
3. **Congestion** : 네트워크가 혼잡한 문제
4. **Overload** : receiver가 overload 되는 문제

# 흐름제어

- 송신측과 수신측 사이의 데이터 처리 속도 차이를 해결하기 위한 기법이다
- 송신 측의 전송량 > 수신측의 처리량인 경우 , 패킷이 수신 측의 큐를 넘어 손실될 수 있어서 송신측 패킷 전송량을 제어해줘야 한다.

## 1. Stop and Wait (정지-대기)

상대방에게 데이터를 보낸 후 잘 받았다는 응답이 올 때까지 기다리는 방식

![Untitled](TCP%20%E1%84%92%E1%85%B3%E1%84%85%E1%85%B3%E1%86%B7%E1%84%8C%E1%85%A6%E1%84%8B%E1%85%A5%20&%20%E1%84%92%E1%85%A9%E1%86%AB%E1%84%8C%E1%85%A1%E1%86%B8%E1%84%8C%E1%85%A6%E1%84%8B%E1%85%A5%201441d8f69b89480b8e5eecbe66083599/Untitled.png)

매번 전송한 패킷에 대한 응답을 받아야 그 다음 패킷을 전송할 수 있다.

⇒ 패킷을 하나씩 보내기 때문에 비효율적인 방법

## 2. Sliding Window (슬라이딩 윈도우)

**송신 측이 수신 측에서 받은 윈도우 크기를 참고해서 데이터의 흐름을 제어하는 방식**

⇒ 오늘날 사용하는 방식 

![https://velog.velcdn.com/images%2Fchullll%2Fpost%2Fc15d30cb-1688-44a4-9620-3a9141e7e2bf%2F253F7E485715ED5F27.png](https://velog.velcdn.com/images%2Fchullll%2Fpost%2Fc15d30cb-1688-44a4-9620-3a9141e7e2bf%2F253F7E485715ED5F27.png)

수신 측에서 설정한 윈도우 크기만큼 송신 측에서 확인 응답 없이 세그먼트를 전송할 수 있게 해서 데이터 흐름을 동적으로 조절한다.

송신 측은 확인응답(ACK) 라는 프레임을 받게 되면 ACK 프레임에 따른 프레임의 수 만큼 오른쪽으로 경계가 확장된다.

> 윈도우?
> 
> 
> 수신, 송신 스테이션 양쪽에서 만들어진 버퍼 크기!
> 

---

# 혼잡제어

네트워크 내에 패킷의 수가 과도하게 증가하는 현상을 **혼잡**이라고 한다.

혼잡 제어는 **혼잡 현상을 방지하고 제거하기 위한 기능**이다.

흐름 제어는 송신 측과 수신 측의 전송 속도를 다루고, 혼잡 제어는 라우터를 포함한 넓은 범위의 전송 문제를 다룬다.

### 1. AIMD(Addictive Increase Multiplicative Decrease)

![https://velog.velcdn.com/images%2Fchullll%2Fpost%2F7f6f3777-a305-411b-be15-c3caf9a782be%2Fimage.png](https://velog.velcdn.com/images%2Fchullll%2Fpost%2F7f6f3777-a305-411b-be15-c3caf9a782be%2Fimage.png)

- 처음에 패킷을 하나씩 보내고 문제가 발생하지 않으면 윈도우 크기를 1씩 증가하는 방법
- 패킷 전송에 실패하거나 일정 시간을 넘으면 패킷 전송 속도를 절반으로 줄인다.
- 네트워크에 늦게 들어온 호스트가 처음에는 불리하지만, 시간이 흐르면서 평형상태로 수렴한다.
- 단점
    - 처음에 전송 속도를 올리는 데 시간이 오래걸린다.
    - 네트워크가 혼잡해지는 상황을 미리 감지하지 못한다. 즉, 네트워크가 혼잡해지고 나서야 대역폭을 줄인다.

### 2. 슬로우 스타트

![https://velog.velcdn.com/images%2Fchullll%2Fpost%2Fa721c0b1-9ec8-4bdf-bd28-2ec540def9e6%2Fimage.png](https://velog.velcdn.com/images%2Fchullll%2Fpost%2Fa721c0b1-9ec8-4bdf-bd28-2ec540def9e6%2Fimage.png)

- AIMD와 같이 패킷을 하나씩 보내고 문제가 발생하지 않으면 각 ACK 패킷마다 윈도우 크기를 1씩 늘려준다. 즉, 한 주기가 지나면 윈도우 크기는 2배가 된다.
- AIMD와 달리 전송 속도를 지수 함수 꼴로 증가시켜서 윈도우 크기를 더 빠르게 증가시킨다.
- 혼잡이 감지되면 윈도우 크기를 1로 줄인다.
- 처음에는 네트워크 수용량을 예상할 수 있는 정보가 없지만, 한 번 혼잡 현상이 발생한 후에는 네트워크의 수용량을 어느 정도 예상할 수 있다.
- 그래서 혼잡 현상이 발생하는 윈도우 크기의 절반가지는 지수 함수 꼴로 윈도우 크기를 증가시키고 그 이후에는 완만하게 1씩 증가시킨다.

### 3. 빠른 회복

- 혼잡한 상태가 되면 윈도우 크기(cwnd) 를 1이 아니라 절반으로 줄이고 선형 증가 시키는 방법
- 빠른 회복 정책이 적용되면 이후엔 순수한 AIMD 방식으로 동작

### 4. 빠른 재전송

![Untitled](TCP%20%E1%84%92%E1%85%B3%E1%84%85%E1%85%B3%E1%86%B7%E1%84%8C%E1%85%A6%E1%84%8B%E1%85%A5%20&%20%E1%84%92%E1%85%A9%E1%86%AB%E1%84%8C%E1%85%A1%E1%86%B8%E1%84%8C%E1%85%A6%E1%84%8B%E1%85%A5%201441d8f69b89480b8e5eecbe66083599/Untitled%201.png)

- 패킷을 받는 수신자 입장에서 세그먼트로 분할된 내용이 순서대로 도착하지 않는 경우가 있다.
- 수신 측에서 패킷을 받을 때 먼저 올 패킷보다 다음 패킷이 먼저 도착해도 ACK 를 보냄
- 단, 이 때 순서대로 잘 도착한 패킷의 마지막 순번을 ACK 에 실어서 보냄
- 따라서 중간에 패킷이 손실되면 송신 측에서는 순번이 중복된 ACK 패킷을 받게 되는 것이다. 이것을 감지하면 문제가 되는 순번의 패킷을 재전송할 수 있다.
- 3 ACK Duplicated: 빠른 재전송은 중복된 순번의 패킷을 3개 받으면 재전송한다.
- 재전송하는 경우 혼잡 상태로 간주하고 혼잡 회피를 한다.