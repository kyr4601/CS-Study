# IP

## 들어가기 전

### OSI 7 layer **3계층의 기능**

![https://blog.kakaocdn.net/dn/dWzBzw/btq94sBiljQ/s36y1cdWf2G82YJUh1NQuK/img.png](https://blog.kakaocdn.net/dn/dWzBzw/btq94sBiljQ/s36y1cdWf2G82YJUh1NQuK/img.png)

![https://blog.kakaocdn.net/dn/ubZ8s/btq98vwYv3i/wLL4xuowPj5IkPiTLu0JF0/img.png](https://blog.kakaocdn.net/dn/ubZ8s/btq98vwYv3i/wLL4xuowPj5IkPiTLu0JF0/img.png)

*발신에서 착신까지의 패킷 경로를 제어한다 .*

3계층은 다른 네트워크 대역,

멀리 떨어진 곳에 존재하는 네트워크까지 어떻게 데이터를 전달할지 제어하는 일을 담당한다.

![https://blog.kakaocdn.net/dn/bawB9z/btq96Eg8ym1/wlVkmee21KVjxVKCb6N8Z0/img.png](https://blog.kakaocdn.net/dn/bawB9z/btq96Eg8ym1/wlVkmee21KVjxVKCb6N8Z0/img.png)

서로 다른 네트워크 대역을 연결시켜주는,

서로 다른 LAN과 LAN 을 연결시켜주는(WAN에서 통신) 일을 하며, 라우터와 같은 3계층 장비가 필요하다.

2계층 장비 스위치만으로는 서로 다른 네트워크 대역으로 만들 수 없다. (안될건 없지만 서로 통신을 하려면 결국 3계층 장비가 필요함.)

**즉, 멀리있는 곳으로 가기 위해선**

**3계층, IP 주소가 필요하다.**

# **IP 주소**

**IP 주소**라는 단어는 꽤 익숙한 단어이다.

우리가 익숙히 접할 수 있는 192.168.0.1과 같은 형태인 IP는,

일반적인 환경에서 우리가 인터넷에 접속할 때

이 **IP 주소**가 **우리 집 주소**의 역할을 한다고 생각하면 되겠다.

그리고 마찬가지로 우리 집에서 네이버, 구글도 각자의 IP가 있으며

거기까지 가는 길목에 있는 지점들도 모두 IP 주소를 가지고 있다.

**IP 프로토콜에서는 현재 IPv4(Internet Protocol version 4)의 주소 체계를 사용하고 있다.**

### **IPv4란?**

**IPv4의 IP 주소는 32비트*(bit), 즉 0 또는 1로 만으로 표기하는 이진(Binary)수 32자리로 구성**되어 있다.

이진수로 표기된 IP 주소는 사람이 알아보기 어렵기 때문에  

아래 같이 전체 32비트를 8비트씩 4그룹으로 나누어, 각 그룹을 십진수로 변환하고, 그룹의 경계에 '. (닷, 점)'을 넣은 형식(Dotted-decimal notation)으로 표기하고 있다.

![https://blog.kakaocdn.net/dn/9eEB0/btrgGrvpd5C/9SWfdCqDYcB71khy4rDeI1/img.png](https://blog.kakaocdn.net/dn/9eEB0/btrgGrvpd5C/9SWfdCqDYcB71khy4rDeI1/img.png)

8비트를 십진수로 변환하면 0~255 사이의 값을 갖기 때문에 I**Pv4에서는 256 이상의 값을 갖는 IP 주소는 존재하지 않는다.**

IPv4로 할당할 수 있는 IP 주소의 개수는 2의 32승인 약 43억 개다.

![https://blog.kakaocdn.net/dn/b5uoSX/btrgU2Oya4X/yI1ZDB7LTRdv26sDiWqWc0/img.png](https://blog.kakaocdn.net/dn/b5uoSX/btrgU2Oya4X/yI1ZDB7LTRdv26sDiWqWc0/img.png)

참고로 IPv6는 128비트로 구성되어 2의 128승, 약 340간(약 340조 X 1조 X 1조) 개의 IP 주소를 사용할 수 있습니다.

# **IP 주소의 클래스**

처음 IP 라는 주소를 만들었을 때,

**Classful하게, '클래스 별로 IP를 나누어' 사용했다.**

### **IPv4의 클래스**

IP 주소는 무제한으로 막 퍼줄 수 있는 자원이 아니라 유한한 자원이었다. 그래서 특정 대역마다 사용처를 나누어서 관리했는데, 이때 나누어진 대역을 **클래스**라고 부르며, **A~E의 총 5개의 클래스로 나누어 관리했다.** 

각 클래스는 이진법으로 표현한 IPv4인 00000000.00000000.00000000.00000000 중 **첫번째 필드에 있는 8비트에 제한을 만들어서 표기하기 때문에 첫번째 필드의 숫자만 보면 해당 IPv4 주소가 어느 클래스에 속해있는 지 알 수 있다.**

이 제한은 바로 **첫번째 필드의 최상위 비트를 강제하는 방식이**다.

예를 들어 A 클래스는 무조건 비트가 0으로 시작해야하기 때문에 00000000(0)에서 01111111(127)사이의 첫번째 필드를 사용할 수 있고 C 클래스는 무조건 비트가 110로 시작해야하기 때문에 11000000(192)부터 11011111(223)까지의 첫번째 필드를 사용할 수 있는 식.

| 클래스 | 네트워크 구분 | 시작주소 | 주소범위 |
| --- | --- | --- | --- |
| A | 0xxxxxxx , 첫번째필드 | 0.0.0.0 | 0.0.0.0 ~ 127.255.255.255 |
| B | 10xxxxxx , 두번째필드 | 128.0.0.0 | 128.0.0.0 ~ 191.255.255.255 |
| C | 110xxxxx , 세번째필드 | 192.0.0.0 | 192.0.0.0 ~ 223.255.255.255 |
| D:멀티캐스트 | 1110xxxx | 224.0.0.0 | 224.0.0.0 ~ 239.255.255.255 |
| E:예약 | 1111xxxx | 224.0.0.0 | 240.0.0.0 ~ 255.255.255.255 |

이렇게 나누어진 클래스들에 대해 조금 더 살펴보자.

이렇게 **클래스로 나누었던 이유는 관리를 위해 구분**하기 위해서 였다.

**첫번째 필드는 네트워크 대역을 '구분'하는데 쓰고,**

나머지 두번째 세번째 필드는 **그 하나의 네트워크 대역 안에 속해있는 각각의 PC들을 '구분'한다.** 

우선 A 클래스의 첫번째 필드를 보자.

![https://blog.kakaocdn.net/dn/rop73/btq97lVWFMB/uCKCqvPTzaKOAz4heXmwZ1/img.png](https://blog.kakaocdn.net/dn/rop73/btq97lVWFMB/uCKCqvPTzaKOAz4heXmwZ1/img.png)

A클래스에서도 여러개의 네트워크 대역 A1, A-2 등이 존재할 수 있을텐데

가장 처음 필드, 첫번째 필드에서만 네트워크 대역을 구분하니까, 이 두 구역을 구분하려면

앞의 숫자 0과 127 ( **0**.0.0.0 ~ **127**.255.255.255) 로 구분했다.

즉, A 클래스의 네트워크 대역은 전세계에 총 128개 뿐 인 것이다. 

하지만 이 네트워크 대역 안에 속해있는 PC들의 수를 생각해 보자.

0.**0.0.0** ~ 127.**255.255.255**

**2의 24승 = *16777216 개의 PC를 구분할 수 있는 셈이다.***

하나의 네트워크 대역안에 있는 PC의 수가 16777216개라니.

매~~~우큰 A 클래스는 아주 큰~~ 기관에서나 쓰는 주소다.

B클래스를 살펴보자.

![https://blog.kakaocdn.net/dn/borRso/btracPVx2RT/4LDVkiBpQpf1L1tKYgNy8k/img.png](https://blog.kakaocdn.net/dn/borRso/btracPVx2RT/4LDVkiBpQpf1L1tKYgNy8k/img.png)

마찬가지로 네트워크 대역과 그 대역대 안에 속해있는 PC의 수를 생각해 보면

확실히 A 클래스 보다는 네트워크 대역을 구분할 수 있는 숫자가 많아졌고,

그 대역에 안에 속해있는 PC들의 수는 많이 줄었다. 2의 16승 개=65536 대이다. 

C클래스는 여기서 조금 더 줄었다.

![https://blog.kakaocdn.net/dn/nk4KR/btq97l2HkVF/nnZjkD2WmOVFDkbKWoQ8u0/img.png](https://blog.kakaocdn.net/dn/nk4KR/btq97l2HkVF/nnZjkD2WmOVFDkbKWoQ8u0/img.png)

이제 하나의 대역에 안에서 쓸 수 있는 PC의 수가 한 눈에 보인다. 256개!

그리고 많은 네트워크 대역들을 구분할 수 있게 됐다.(192.0.0~233.255.255)

그래서 일반적으로 많이 쓰던 것이 이 C클래스 였다.

이 A,B,C 클래스는 일반 PC들 용으로 사용하였고..

D클래스는 멀티캐스트용을 위해 남겨둔 주소,

E클래스는 IP주소 관련 연구, 실험용으로 남겨둔 주소로 구분하여 사용했었다.

*그런데,*이렇게 잘 나누어 사용하던 IP 주소에 문제가 생겼다.

# **낭비되는 IP 주소**

만약 한 회사에 컴퓨터가 31대가 있고,

회사에 30명이 있다고 가정해 보자.

우린 A클래스 네트워크 대역을 사용했고,

만약 아래와 같은 대역대를 썼다고 가정해보자.

```python
100.0.0.1 ~ 100.0.0.31
```

첫번째 부분인 100. 네트워크 대역을 구분하는 클래스이니까,

우리 회사에서만 쓸 수 있는 네트워크 대역인데....

그럼 약 1600만 대의 IP가 사용되지 못하고 낭비 되기 시작한 것이다.

사실 1960, 70년 대는 이 사실이 상관 없었다. 낭비해서 써도 남았던 시절이라.

그러나 세상의 발전은 빨랐고 IP주소가 부족해져 하루빨리 IP주소의 낭비 문제를 해결해야 했다.

해결책은 ***클래스에 맞게 쓰지 말자*** 였다.

> 하나의 큰 네트워크를 여러개의 작은 네트워크로 나누어 쓰자!
> 

라는 **서브넷 마스크** 개념이 도입됐다.

# **서브넷 마스크**(Subnet mask)의 도입

'Classless IP' 주소

이전에 아래와 같이 고정된 필드 단위로 구분(.)을 했다면,

```python
00000000.00000000.00000000.00000000
```

낭비를 줄이기 위해,

이 하나하나의 필드를 아래와 같이 원하는 곳에서 자를 수 있게 (구분할 수 있게)

```python
00000000.00000000.00000000.00 000000 //이렇게,
00000000.00000000.00000000.00000 000 //이렇게도 가능!
```

'Subnet mask',

네트워크를 서브 = 보조 한다.

**네트워크 대역을 잘게 쪼개는 값**이라는 녀석을 도입한 것이다.

네트워크 대역을 어디서부터 구분할 것인지 지정하는 값이 서브넷 마스크다.

**그래서 지금 IP주소는 항상 서브넷 마스크랑 같이 사용한다.**

### 서브넷 마스크

---

: 네트워크 대역을 나눠주는데 사용하는 값.

: 2진수로 표기했을 때 1로 시작하고, 1과 1 사이에 0이 올 수 없다는 규칙을 가지고 있다.

---

이 ***서브넷마스크로 어떻게 필드를 구분할까?*** 

간단하게만 보자면,

서브넷 마스크를 도입한 후 네트워크 대역과 대역대에 속한 PC의 수를 구분하는 방법은 아래와 같다.

```python
예시)
255.255.255.192 ->
11111111.11111111.11111111.11  000000
      (네트워크 대역 구분)    ,    (PC의 수)
```

**11111111.11111111.11111111.11**

여기까지가 **네트워크 대역을 구분**하는 곳이고,

**000000**

여기서부터 하나의 **네트워크 대역에서 속해있는 PC를 구분.**

그렇다면 여기서 하나의 네트워크 대역에서 쓸 수 있는 PC의 수는?

0부터 63일 테니까, 총 64대가 들어간다.

낭비되는 IP가 이전보다 많이 줄긴한다.

여기서 필드를 하나만 더 쪼개어 보자.

1을 하나만 더 늘려보도록 한다.

```python
11111111.11111111.11111111.111   00000
       (네트워크 대역 구분)     ,   (PC의 수)
```

여기서는 몇 대의 PC가 들어갈 수 있는지 계산해 보면 이번엔 32대!

또 나쁘지 않은 것 같지만....

만약 PC를 20대 만 사용하려 하는 네트워크 대역이라고 가정해보자.

아까의 32대가 들어간 네트워크 대역에서

한번 더 쪼개면 16대가 들어갈 수 있다.

내가 20대의 PC만 쓰고 싶은 경우, 16대는 부족하니까

네트워크 대역을 32대 쓸 수 있게 잘라야 하고...

결국 10대가 낭비되는 것.

**어쨌든 또 IP 주소 낭비가 되고, IP가 부족해지기 시작한 것이다.**

또.. 그렇게 생각보다 전세계의 인터넷 사용자가 빠르게 늘면서 결국

2011년 2월 4일부로 모든 주소가 소진되어 현재는 IPv4의 할당이 중지되었다.

그래서 IPv6로 넘어가자는 말이 나온다.

4바이트인 IPv4에서 -> 16바이트인 IPv6가 되면 표현할 수 있는 수가 훨~~씬 늘어나니까!

그치만... 몇십년 동안 IPv4를 쓰다가 갑자기 IPv6 로 넘어가려면,

전세계에 있는 수 많은 장비들, 컴퓨터들을 싹 다 바꿔야 하는데 이것도 보통 일이 아니었다.

고심끝에 여기서 또 하나의 개념이 도입된다.

**바로 사설IP와 공인IP!**

# **사설IP, 공인IP**

*'Classless IP' 주소*

![https://blog.kakaocdn.net/dn/Qwsar/btracObg0sw/1BDbuCggqPEKbMzttkRqYK/img.png](https://blog.kakaocdn.net/dn/Qwsar/btracObg0sw/1BDbuCggqPEKbMzttkRqYK/img.png)

이것이 현재 사용하고 있는 IPv4의 개념.

우리가 현재 사용하고 있는 방식이 이 공인ip/사설ip (+classless) 이다.

![https://blog.kakaocdn.net/dn/bNWjA4/btq96E9hq8D/PMmSSKSxKyrdixlwIIPWK0/img.png](https://blog.kakaocdn.net/dn/bNWjA4/btq96E9hq8D/PMmSSKSxKyrdixlwIIPWK0/img.png)

간단히 말해

**공인 IP 하나 당, 사설 네트워크 대역이 아예 따로 생겼기 때문에**

**IP 하나당 0.0.0.0~255.255.255.255 까지가 생긴 것.** 그래서 사용가능한 IP의 범위가 넉넉해 졌다고 할 수 있겠다.

![https://blog.kakaocdn.net/dn/cgQdMy/btq94qDvtdK/0kwFzTTYK4GtcBjISvs9X1/img.png](https://blog.kakaocdn.net/dn/cgQdMy/btq94qDvtdK/0kwFzTTYK4GtcBjISvs9X1/img.png)

![https://blog.kakaocdn.net/dn/d3o8kj/btq98u5YP2r/tvIe4YN4oYAKbvVQY2TQR1/img.png](https://blog.kakaocdn.net/dn/d3o8kj/btq98u5YP2r/tvIe4YN4oYAKbvVQY2TQR1/img.png)

공인/사설 IP 도입 전 -> 도입 후

# **특수한 IP 주소, 예약된 IP 주소**

![https://blog.kakaocdn.net/dn/bi2HR7/btrabw25nHI/4DsiIs3OQcZYMc0bEoZVi1/img.png](https://blog.kakaocdn.net/dn/bi2HR7/btrabw25nHI/4DsiIs3OQcZYMc0bEoZVi1/img.png)

### Wildcard **0.0.0.0 (할당되지 않은 메타 주소)**

쉽게 설명 하자면 *'나머지' 모든 IP 를 뜻한다.*

![https://blog.kakaocdn.net/dn/bvyzz8/btracOvAGTq/B1cJqaTD3Fm3W52D16i0Ok/img.png](https://blog.kakaocdn.net/dn/bvyzz8/btracOvAGTq/B1cJqaTD3Fm3W52D16i0Ok/img.png)

라우팅 경로를 찾지못한 나머지 모든것을 나타내는 의미로 쓰인다.

0.0.0.0은 일반적으로 컴퓨터에 사용하는 애가 아님!

### 나 자신을 나타내는 주소**, 127.0.0.1 (자기 자신, 일명 루프백)**

![https://blog.kakaocdn.net/dn/baJLWw/btq97OcAiaw/dhXSNXtOydg6k7CfMG46A0/img.png](https://blog.kakaocdn.net/dn/baJLWw/btq97OcAiaw/dhXSNXtOydg6k7CfMG46A0/img.png)

꼭 127.0.0.1이 아니더라도,

127.0.0.2, 4 등...

127. 으로 시작하는 이 주소는 '나 자신의 주소'.

소프트웨어의 테스트 목적으로 주로 사용되며,

**같은 장치 상에서 통신할 때 사용한다.**

127.0.0.1은 localhost라는 이름으로 접근이 가능한데,

- **localhost :**

컴퓨터 네트워크에서 사용하는 loopback 호스트명으로, 자신의 컴퓨터를 의미.

로컬 컴퓨터를 원격 컴퓨터인것 처럼 통신할 수 있어 주로 테스트 목적으로 사용된다.

(IPv4 에선 127.0.0.1, IPv6 에선 ::1)

### **게이트웨이 주소**

일반적으로 공유기의 IP를 쓰고

이 네트워크 대역에서 쓸 수 있는 가장 작은 IP를 쓰는 게이트웨이.

![https://blog.kakaocdn.net/dn/bGmI82/btq98cD5vbS/Hg2hgJFegXkQh4C9gPmsNK/img.png](https://blog.kakaocdn.net/dn/bGmI82/btq98cD5vbS/Hg2hgJFegXkQh4C9gPmsNK/img.png)

게이트웨이란, **외부랑 통신할 때 무조건 들어와야하는 문**이라고 생각하면 된다.

외부 세상으로 나가는 문이 어딘지 알려주는 값!

![https://blog.kakaocdn.net/dn/cUSFn0/btq97Op9RVn/SQmLc1anQAEjBgav1kq7P1/img.png](https://blog.kakaocdn.net/dn/cUSFn0/btq97Op9RVn/SQmLc1anQAEjBgav1kq7P1/img.png)

아이피 주소와 서브넷 마스크는 나를 알려주는 값이고,

게이트웨이는 나가는 문이 어딘지 알려주는 값이니

게이트웨이 설정을 안해도 내 컴퓨터 IP 설정은 되지만, 인터넷이 되지 않는다.

그래서 결과적으로 우리는 아래 3가지가 있어야

**'인터넷 통신'** 이라는 것을 할 수 있게 된다는 것이다.

![https://blog.kakaocdn.net/dn/1EoR0/btq98vqkWBu/1tFIvPEsT9LfTTcKoMQ4y0/img.png](https://blog.kakaocdn.net/dn/1EoR0/btq98vqkWBu/1tFIvPEsT9LfTTcKoMQ4y0/img.png)

물론 네이버를 IP주소로 찾아가는 사람은 없으니, 아래 DNS 서버까지 세팅해 주면 인터넷이 된다.