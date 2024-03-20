# DB 3주차

# 캐시

## Cache DB 란?

데이터를 빠르게 조회할 수 있는 곳.

- 원본 데이터는 다른 곳에 있고 그것으로부터 copy된 데이터가 Cache에 존재한다.
- 일반 DB는 디스크IO를 기반으로 동작하고 효율을 높이기 위해 memory를 사용한다.
- in-memory DB는 모든 데이터를 메모리에서 다루기 때문에 조회속도가 빠르다.

## Cache 구조 및 전략

캐시를 사용할 때 어떻게 배치할 것인지에 대한 캐싱 전략이 필요하다. 

이에 따라 성능에 영향을 끼치기 때문에 상황에 맞게 적절한 전략을 사용해줘야 한다.

올바른 캐싱 전략을 선택하는 것이 성능 향상의 핵심이다. 

### Look-Aside(Cache-Aside) 읽기 전략

![Untitled](DB%203%E1%84%8C%E1%85%AE%E1%84%8E%E1%85%A1%20c76515daf2ea4633975202c81e0955b1/Untitled.png)

![Untitled](DB%203%E1%84%8C%E1%85%AE%E1%84%8E%E1%85%A1%20c76515daf2ea4633975202c81e0955b1/Untitled%201.png)

1. Cache에서 데이터가 있는지 체크한다.
2. Cache에 Data가 있으면 `Cache hit` → Data를 Application에 return 한다.
    
    Cache에서 Data가 없으면`Cache miss` → Application은 DB에 쿼리를 날려서 데이터를 읽고 Application에 Data를 반환한 뒤 Cache에 Data를 저장한다.
    

**특징**

---

- 읽기에 상당히 적합하다.
- 반복적인 호출에 적합하다.
- 캐시 시스템 자체에 대한 장애 대비 구성이 가능하다.
    
    ex) Cache DB인 Redis가 죽더라도 DB에서 데이터를 가져올 수 있다.
    
    - But Redis와 같은 Cache에 Connection이 상당히 많았다면 Redis가 죽고 난 뒤 DB에 일시적으로 Connection이 상당히 발생하므로 DB에 부하가 올 수 있다.
- Cache Store 와 Data Store 가 가지고 있는 데이터가 서로 다를 수 있다.
- 초기 조회 시 무조건 Data Store를 호출해야 하므로 단건 호출 빈도가 높은 서비스에 부적합하다.
- 초기에 데이터가 DB에만 저장되어 있어 처음에 cache miss가 많아지기 때문에 성능 저하의 가능성이 있다.
    
    미리 DB에서 캐시로 데이터를 넣어주는 작업이 필요하다. → Cache Warming
    

### Write-Back 쓰기 전략

![Untitled](DB%203%E1%84%8C%E1%85%AE%E1%84%8E%E1%85%A1%20c76515daf2ea4633975202c81e0955b1/Untitled%202.png)

![Untitled](DB%203%E1%84%8C%E1%85%AE%E1%84%8E%E1%85%A1%20c76515daf2ea4633975202c81e0955b1/Untitled%203.png)

Cache에 데이터를 몰아두고 DB에 한번에 배치작업으로 write 한다.

**특징**

---

- DB에 대한 전체 쓰기를 줄일 수 있기 때문에 비용을 감소할 수 있다.
- 단 캐시가 장애가 났을 경우 Data가 유실될 수 있다.
- 불필요한 리소스가 저장된다.
- Write Back 패턴의 경우 Cache Store가 데이터 저장소 역할을 하면서 동시에 Data Store에 Write 부하를 줄이기 위한 Queue의 역할도 한다.
    - Database의 부하를 경감 시킬 수 있는 장점이 있다.
    - 대체로 쓰기 작업이 많은 경우 적용을 권장한다.
- **Write Back 장점**
    - 데이터베이스의 일시적인 다운타임을 허용하거나 장애에 대응할 수 있다.
    - 쓰기 성능을 향상시키고 쓰기 작업량이 많은 워크 로드에 적합하다.
- **Write Back 단점**
    - Cache Store에서 Data Store로 데이터를 전송하기 전에 장애 발생 시 데이터 분실 발생 위험이 있을 수 있음
    - 자주 사용되지 않는 데이터가 저장되어 리소스 낭비, Write 작업에 부하 발생할 수 있음 ****
        
        ****TTL***을 꼭 사용하여 사용되지 않은 데이터를 삭제해야 한다
        
    

# Redis

## Redis란

Remote Dictionary Server의 약자로서, “키-값” 구조의 비정형 데이터를 관리하기 위한 오픈 소스 기반의 비관계형(NoSQL) 인메모리 DB, cache, message broker 등 의 용도로 사용.

Redis는 캐시 시스템으로서 모든 데이터를 메모리에 저장하고 조회합니다. 즉, 인메모리 데이터베이스 입니다. 다른 인메모리 디비들과의 가장 큰 차이점은 레디스의 다양한 자료구조 입니다.

![https://miro.medium.com/max/700/1*tMiZs3RCrmxLGiFZgWRP6g.png](https://miro.medium.com/max/700/1*tMiZs3RCrmxLGiFZgWRP6g.png)

이렇게 다양한 자료구조를 지원하게 되면 개발의 편의성이 좋아지고 난이도가 낮아진다는 장점이 있습니다.

예를들어, 어떤 데이터를 정렬을 해야하는 상황이 있을 때, DBMS를 이용한다면 DB에 데이터를 저장하고, 저장된 데이터를 정렬하여 다시 읽어오는 과정은 디스크에 직접 접근을 해야하기 때문에 시간이 더 걸린다는 단점이 있습니다. 하지만 이 때 Redis를 이용하고 레디스에서 제공하는 Sorted-Set이라는 자료구조를 사용하면 더 빠르고 간단하게 데이터를 정렬할 수 있습니다.

![https://miro.medium.com/max/700/1*zArWVI0y5u_WVj0gktm92Q.png](https://miro.medium.com/max/700/1*zArWVI0y5u_WVj0gktm92Q.png)

NoSQL로서 Key-Value 타입의 저장소인 레디스(Redis, Remote Dictionary Server)의 주요 특징은 아래와 같습니다.

- 영속성을 지원하는 인메모리 데이터 저장소
- 읽기 성능 증대를 위한 서버 측 복제를 지원
- 쓰기 성능 증대를 위한 클라이언트 측 샤딩(Sharding) 지원
- 다양한 서비스에서 사용되며 검증된 기술
- 문자열, 리스트, 해시, 셋, 정렬된 셋과 같은 다양한 데이터형을 지원. 메모리 저장소임에도 불구하고 많은 데이터형을 지원하므로 다양한 기능을 구현

그래서 최종적으로 Redis를 한 문장으로 정의하면 아래와 같습니다.

> Redis는 고성능 키-값 저장소로서 문자열, 리스트, 해시, 셋, 정렬된 셋 형식의 데이터를 지원하는 NoSQL이다.
> 

## Redis 특징 및 장단점

1. 성능

모든 Redis 데이터는 메모리에 저장되어 대기 시간을 낮추고 처리량을 높인다.

평균적으로 읽기 및 쓰기의 작업 속도가 1ms로 디스크 기반 데이터베이스보다 빠르다.

2. 유연한 데이터 구조

Redis의 데이터는 String, List, Set, Hash, Sorted Set, Bitmap, JSON 등 다양한 데이터 타입을 지원한다.따라서, 애플리케이션의 요구 사항에 알맞은 다양한 데이터 타입을 활용할 수 있다.

3. 개발 용이성

Redis는 쿼리문이 필요로 하지 않으며, 단순한 명령 구조로 데이터의 저장, 조회 등이 가능하다.또한, Java, Python, C, C++, C#, JavaScript, PHP, Node.js, Ruby 등을 비롯한 다수의 언어를 지원한다.

4. 영속성

Redis는 영속성을 보장하기 위해 데이터를 디스크에 저장할 수 있다. 서버에 치명적인 문제가 발생하더라도 디스크에 저장된 데이터를 통해 복구가 가능하다.

> `Redis 영속성`
레디스는 지속성을 보장하기 위해 데이터를 DISK에 저장할 수 있습니다. 
서버가 내려가더라도 DISK에 저장된 데이터를 읽어서 메모리에 로딩을 합니다.

데이터를 DISK에 저장하는 방식
RDB(Snapshotting) 방식
> 
> - 순간적으로 메모리에 있는 내용을 DISK에 전체를 옮겨 담는 방식
> 
> AOF (Append On File) 방식
> 
> - Redis의 모든 write/update 연산 자체를 모두 log 파일에 기록하는 형태

5. 싱글 스레드 방식

Redis는 싱글 스레드 방식을 사용하여 한 번에 하나의 명령어만을 처리한다. 따라서 연산을 원자적으로 처리하여 Race Condition(경쟁 상태)가 거의 발생하지 않는다.하지만, 멀티 스레드를 지원하지 않기 때문에 시간 복잡도가 O(n)인 명령어의 사용은 주의해서 사용해야 한다.

# MongoDB

MongoDB는 기존에 사용하던 RDB의 확장성과 신속성 문제로 개발한 Database이다.

MongoDB는 문서 지향적인 NoSQL 데이터베이스로, Document라는 형식의 자료구조를 사용하며, 대량의 비정형 데이터를 저장하고 처리하는 데 쓰인다.

NoSQL은 관계형 데이터베이스에서 하지 못했거나 어려운 것들을 해결하기 위해 등장했다.

- 데이터 가시성이 뛰어남
- Join 없이 조회가 가능하므로 응답 속도가 빠르다.
- 스키마가 유연해서 데이터 모델을 App의 요구사항에 맞게 데이터를 수용할 수 있다.
- 스키마 설계를 못하면 성능 저하가 발생한다.

## MongoDB의 장점

- Document 지향 Database이다.
- 데이터 중복이 발생할 수 있지만 접근성과 가시성, 읽기 성능이 좋다.
    - 생성, 수정, 삭제 시 각 컬렉션에 반영해야 하는 단점이 있다.
- 스키마 설계가 어렵지만, 스키마가 유연해서 Application의 요구사항에 맞게 데이터를 수용할 수 있다.
- 다양한 종류의 Index를 제공한다.
- 응답 속도가 빠르다.

## MongoDB 개념

아래 그림은 RDBMS와 대응되는 MongoDB의 개념들이다.

![https://blog.kakaocdn.net/dn/bqqSg9/btrURkI0bj7/Cv8vUK0OhLjixrQkRMR34k/img.png](https://blog.kakaocdn.net/dn/bqqSg9/btrURkI0bj7/Cv8vUK0OhLjixrQkRMR34k/img.png)

## MongoDB 구조

### 1. 기본 Database

MongoDB는 아래 3개의 기본 Database를 제공한다. 

- admin
    - 인증과 권한 부여 역할을 한다.
    - shutdown 등 명령어는 해당 DB에 대한 접근이 필요하다.
- local
    - replication에 필요한 oplog와 같은 컬렉션을 저장
    - instance 진단을 위한 startup_log와 같은 정보를 저장
    - 복제 대상에서 제외된다.
- config
    - sharded cluster에서 각 shard의 정보를 저장한다.

### 2. Collection의 특징

- 동적 스키마를 가지기 때문에 스키마를 수정하려면 값을 추가/수정/삭제만 하면 된다.
    - 스키마라는 개념이 없다고 보는 것이 좋을 것 같다.
- Collection 단위로 Index를 생성할 수 있다.
- Collection 단위로 Shard를 나눌 수 있다.
    - 즉, Index나 Sharding Key 등의 활용을 하기 위해서는 Schema를 어느정도 유지를 해줘야 한다.

### 3. Document의 특징

- 저장할 때 BSON(Binary-JSON) 형태로 저장한다.
    - JSON보다 텍스트 기반 구문 분석이 빠르다. (처리가 빠르다.)
    - 공간이 효율적이다.
- 모든 Document에는 "_id" 필드가 있고, 없이 생성하면 고유한 ObjectId를 저장한다.
- 생성 시 상위 구조인 Database나 Collection이 없다면, 먼저 생성하고 Document를 생성한다.
- 최대 크기는 16MB로 고정되어 있다.

# 트랜잭션

## **트랜잭션(Transaction)이란?**

**쪼개질 수 없는 업무처리의 단위**

트랜잭션(Transaction)은 데이터베이스의 상태를 변환시키는 하나의 논리적 기능을 수행하기 위한 작업의 단위 또는 한꺼번에 모두 수행되어야 할 일련의 연산들을 의미한다.

**특징**

1. 트랜잭션은 데이터베이스 시스템에서 병행 제어 및 회복 작업 시 처리되는 작업의 논리적 단위이다.
2. 사용자가 시스템에 대한 서비스 요구 시 시스템이 응답하기 위한 상태 변환 과정의 작업단위이다.
3. 논리적인 작업 셋을 모두 완벽하게 처리(`Commit`)하거나 또는 처리하지 못할 경우에는 원 상태로 복구(`Rollback`)해서 작업의 일부만 적용되는 현상이 발생하지 않게 만들어주는 기능이다.

### 예시 (계좌이체)

ATM으로 계좌이체를 한다고 생각해보면,

1. A 은행에서 출금하여 B은행으로 송금하려고 합니다.
2. 송금 중, 알 수 없는 오류가 발생하여 A은행 계좌에서 돈은 빠져 나갔지만 B은행의 계좌에 입금되지 않았습니다.
3. 이와 같은 상황을 막기 위해 거래가 성공적으로 모두 끝나야 이를 완전한 거래로 승인하고, 거래 도중 뭔가 오류가 발생했을 때는 이 거래를 처음부터 없었던 거래로 완전히 되돌리는 것입니다.

이렇게 거래의 **안전성을 확보하는 방법이 트랜잭션**입니다. 데이터베이스에선 테이블에서 데이터를 읽어 온 후 다른 테이블에 데이터를 입력하거나 갱신, 삭제하는데 처리 도중 오류가 **발생하면 모든 작업을 원상태로 되돌립니다**. 데이터베이스에선 처리 과정이 모두 성공했을 때만 최종적으로 데이터베이스에 반영합니다.

내 계좌의 잔액에서 이체한 금액만큼 빼는 일과, 상대 계좌의 잔액에서 해당 금액만큼 더하는 일은 쪼개어져서는 안됩니다. 두가지 명령 중 하나만 실행되서는 안되며 하나의 업무로 함께 진행되어야 하는 일이기때문입니다.

즉, 더이상 쪼갤 수 없기 때문에 일부만 동작해선 안된다는 것이 트랜잭션의 핵심입니다.

부분 작업들 여러개가 모여진 이러한 트랜잭션을 처리하기 위해 데이터베이스는 명령어를 활용한다.

### COMMIT

- 지금까지 작업한 내용을 DB에 영구적으로 저장한다.
- transaction을 종료한다.

### ROLLBACK

- 지금까지 작업들을 모두 취소하고 transaction 이전 상태로 되돌린다.
- transaction을 종료한다.

### AUTOCOMMIT

- 각각의 쿼리에 자동으로 transaction 처리를 해주는 개념
- 쿼리가 성공하면 자동으로 commit 한다.
- 실행 중에 문제가 생기면 rollback 한다.
- MySQL에서는 default로 autocommit이 enabled 되어 있다.
- MySQL에서 START TRANSACTION 실행을 하면 그 동시에 autocommit은 off된다.
- COMMIT/ROLLBACK과 함께 transaction이 종료되면 원래 autocommit 상태로 돌아간다.

## **트랜잭션(Transaction) 상태**

![https://github.com/Songwonseok/CS-Study/raw/main/Database/images/Transaction-1.png](https://github.com/Songwonseok/CS-Study/raw/main/Database/images/Transaction-1.png)

- **Active**: 트랜잭션이 실행 중이며 동작 중인 상태
- **Partially Committed**: 트랜잭션이 실행되고 데이터 변경을 DB에 적용 전 메모리 공간에만 변경해놓은 상태.
- **Committed**: 트랜잭션이 정상적으로 완료 상태. 즉 실제로 메모리에에서 DB에 데이터를 쓴 상태. Rollback 불가능.
- **Failed**: 오류로 트랜잭션 실패 상태.
- **Aborted**: 트랜잭션이 취소 상태. 트랜잭션 실행 이전 데이터로 돌아감.

## **트랜잭션(Transaction)의 ACID 속성**

ACID(Atomicity, Consistency, Isolation, Durability)는 데이터베이스 트랜젝션이 안전하게 수행된다는 것을 보장하기 위한 트랜잭션의 성질을 말한다.

**1) 원자성(`Atomicity`)**

- 트랜잭션과 관련된 일이 **모두 수행되었거나 되지 않았거나**를 보장하는 특징이다.
- all or nothing
- Commit 이전에는 디스크에 쓰지 않고 **메모리 버퍼에만 저장**해 두었다가 중간에 실패하면 디스크에 반영하지 않는 방식

**2) 일관성(`Consistency`)**

- 트랜잭션이 완료된 다음의 상태에서도 트랜잭션이 일어나기 전의 상황과 **동일하게** 데이터의 일관성을 보장해야 한다.
- 그래서 constraints, trigger 등을 통해 DB에 정의된 rules을 transaction이 위반했다면 rollback 해야 한다.

**3) 격리성(`Isolation`)**

- 각각의 트랜잭션이 서로 간섭 없이 **독립적으로 수행**되는 것을 말한다.
- 복수의 병렬 트랜잭션은 서로 격리되어 마치 순차적으로 실행되는 것처럼 작동되어야 하고, 데이터베이스는 여러 사용자가 같은 데이터에 접근할 수 있어야 한다.
- 여러 종류의 `isolation level`을 제공한다.

**4) 지속성(`Durability`)**

- 성공적으로 수행된(=커밋된) 트랜잭션은 **영구적으로** 데이터베이스에 작업의 **결과가 저장**되어야 한다.
- 시스템 장애가 발생해도 원래 상태로 복구하는 기능이 있다. (체크섬, 저널링, 롤백)

## Concurrency Control

ACID 중 '`Isolation`'은 복수의 트랜잭션을 동시에 실행할 경우, 이상 현상이 일어나지 않도록 한다. 이것을 보장하기 위한 중요한 속성이 `Serializability`과 `Recoverable`이다.

DBMS의 `concurrency control`(동시성 제어)이 이 두 가지를 제공해야 한다.

### 1. Serializability

위의 ACID 중 Isolation에 이런 설명이 있었다.

> 복수의 병렬 트랜잭션은 서로 격리되어 마치 순차적으로 실행되는 것처럼 작동되어야 하고, 데이터베이스는 여러 사용자가 같은 데이터에 접근할 수 있어야 한다.
> 

마치 순차적으로 실행되는 것처럼 ⇒ **Serializability**

내용에 들어가기에 앞서 예시를 위해 용어를 정리하고 가자.

```
R1(A) : A에 대한 트랜잭션1 Read 작업
W1(A) : A에 대한 트랜잭션1 Write 작업
operation : R1(A)와 같은 작업
schedule : 여러 transaction들이 동시에 실행될 때 각 transaction에 속한 operation들의 실행 순서

```

### Serial schedule

> transaction들이 겹치지 않고 한 번에 하나씩 실행되는 schedule
> 
> 
> 즉, **식당**에 자리가 하나라서 합석하지 않고 한 명이 식사가 끝나면 다음 손님이 앉아 식사하는 것이다.
> 

### [ 장점 ]

직렬화의 장점은 하나씩 수행되기 때문에 순서와 정합성이 보장된다는 것이다.

### [ 단점: 성능 ]

하지만 R1(A)와 W1(A) 작업은 디스크 I/O 작업이다. 이 오래 걸리는 작업을 하는동안 CPU는 놀고 있기 때문에 낭비된다.

즉, 좋은 성능을 낼 수 없다.

현실에서는 사용할 수 없다.

### non-serial schedule

> transaction들이 겹쳐서 실행되는 schedule
> 
> 
> 즉, **식당**에 자리를 늘려서 한 명이 식사 중이더라도 다음 손님은 다른 자리에 앉아서 식사할 수 있다.
> 

### [ 장점 : 성능 ]

이번에는 병렬적으로 처리하기 때문에 **동시성**이 높아진다.

그래서 같은 시간에 더 많은 transaction을 처리할 수 있다.

R1(A)와 W1(A) 작업와 같은 오래 걸리는 디스크 I/O 작업을 하는 동안 트랜잭션2와 같은 다음 작업을 수행할 수 있다.

### [ 단점: 이상 현상 ]

transaction이 어떤 형태로 겹쳐 실행되는지에 따라 의도치 않은 결과가 나올 수 있다.

⇒ 트랜잭션을 겹쳐서 실행하면서(non-serial schedule) 성능을 높이고, 이상 현상은 없게 만들자

⇒ serial schedule과 동일한 결과를 내면서 non-serial schedule를 실행하자!

**병렬적으로 처리하면서 순차적으로 실행한 것과 결과가 동일하게 할 수 있는 방법은** 

**conflict serializable**이다.

### Conflict serializable ⭐️

> Conflict serializable = serial schedule과 conflict equivalent할 때
> 
> 
> non-serial schedule이지만 Conflict serializable일 때 정상적인 결과를 낼 수 있다.
> 

### Conflict

> 두 개의 operation이 세 가지 조건을 모두 만족하면 conflict라고 한다.
> 

```
1. operation이 서로 다른 트랜잭션 소속
2. operation이 같은 데이터에 접근
3. 최소 하나는 write operation
```

### [ 예시 ]

아래 예시에서는 R2(B)와 W1(B), W2(B)와 R1(B), W2(B)와 W1(B)가 conflict다.

![https://github.com/devSquad-study/2023-CS-Study/raw/main/DB/img/conflict_ex1.png](https://github.com/devSquad-study/2023-CS-Study/raw/main/DB/img/conflict_ex1.png)

복수의 트랜잭션이 동시에 처리될 때 conflict operation은 실행 순서가 바뀌면 실행 결과도 바뀐다.

예를 들어 W2(B)와 R1(B)를 보자.

- B 계좌 기존 잔액 : 100만 원 / 트랜잭션2는 30만 원 입금.
- **기존)** W2(B) -> R1(B) : B에 130만 원 쓰기. **B에서 130만 원 읽음.**
- **순서가 바뀜)** R1(B) -> W2(B) : **B에서 기존 잔액 100만 원 읽기.** B에서 130만 원 쓰기.
- 순서가 바뀌면 값이 달라진다.

### Conflict equivalent

> 두 개의 schedule이 두 가지 조건을 모두 만족하면 conflict equivalent
> 

```
1. 두 schedule은 같은 transaction을 가진다.
2. 두 schedule 내에 conflict operation의 실행 순서가 동일하다.
```

### [ 예시 ]

1. Sched 1과 Sched 2는 같은 transaction을 가지고 conflict operation의 실행 순서가 동일하다. **(= conflict equivalent)**
2. Sched 2는 트랜잭션이 순차적으로 실행된다. (=serial schedule)

![https://github.com/devSquad-study/2023-CS-Study/raw/main/DB/img/conflict_equivalent_ex1.png](https://github.com/devSquad-study/2023-CS-Study/raw/main/DB/img/conflict_equivalent_ex1.png)

💡 즉, **serial schedule과 conflict equivalent일 때 conflict serializable이라고 한다.**

그럼 RDBMS는 이상 현상 없이 데이터의 무결성을 위해 **conflict serializable한 non-serial schedule를 허용**할 것이다.

그럼 이것을 어떻게 구현할까?

schedule이 conflict serializable하도록 보장하는 **프로토콜**을 적용하면 된다.

### 2. Recoverability

> 트랜잭션이 실패했을 때의 회복 가능성
> 

한 트랜잭션이 rollback 했을 때 다른 트랜잭션이 영향을 받지 않는 것 또한 isolation에 있어서 중요하다.

### Unrecoverable schedule

> 롤백해도 이전 상태로 회복 불가능한 스케줄 ( DBMS가 허용하면 안된다)
> 
> 
> schedule 내에서 commit된 transaction이 rollback된 transaction이 write 했던 데이터를 읽은 경우
> 

### [ 예시 ]

rollback2를 하려고 한다.

`R2(B) - W2(B)`가 한 작업을 `R1(B) - W1(B)`가 작업했기 때문에 트랜잭션2도 rollback을 해야한다.

하지만 이미 트랜잭션1은 커밋했기 때문에 **durability(지속성)** 속성 때문에 rollback할 수 없다.

트랜잭션2가 작업이 완료되지 않은 상태에서 트랜잭션1이 읽는 경우가 **DIRTY READ** (더티 리드)이다.

> DIRTY READ (더티 리드): 어떠한 트랜잭션에서 처리한 작업이 완료되지 않았음에도 불구하고 다른 트랜잭션에서 볼 수 있게 되는 현상.
> 

![https://github.com/devSquad-study/2023-CS-Study/raw/main/DB/img/unrecoverable_schedule_ex1.png](https://github.com/devSquad-study/2023-CS-Study/raw/main/DB/img/unrecoverable_schedule_ex1.png)

### recoverable schedule

> 자신이 읽은 데이터를 write한 transaction이 먼저 commit, rollback 되기 전까지 commit하지 않는 스케줄
> 

### [ 예시 ]

트랜잭션 2번이 먼저 commit/rollback한다.

![https://github.com/devSquad-study/2023-CS-Study/raw/main/DB/img/recoverable_schedule_ex1.png](https://github.com/devSquad-study/2023-CS-Study/raw/main/DB/img/recoverable_schedule_ex1.png)

### [ cascading rollback ]

위의 예시로 이야기하면 트랜잭션2가 rollback을 하면 트랜잭션1은 트랜잭션2의 결과를 읽어왔기 때문에 의존성을 지니고 있다.

그래서 트랜잭션1도 함께 rollback한다. 이것이 **cascading rollback**이다.

이것은 비용이 크다. 그래서 **cascadeless schedule**이 등장한다.

### cascadeless schedule

> 데이터를 write한 transaction이 commit/rollback된 뒤에만 데이터를 읽을 수 있는 스케줄
> 
> 
> 즉, commit되지 않은 write한 데이터는 **읽지** 않는다.
> 

하지만 또 다른 이슈 발생 가능

**식당**에서 떡볶이 가격을 5000원이라고 생각해보자.

동시에 사장님(트랜잭션1)은 4000원으로 쓰려고 하고, 점장님(트랜잭션2)는 3000원으로 쓰려고 한다.

이 상태에서 트랜잭션1을 롤백하면 기존 가격인 5000원으로 돌아온다. 그러면 점장님(트랜잭션2)가 설정한 3000원 가격은 그대로 사라진다.

![https://github.com/devSquad-study/2023-CS-Study/raw/main/DB/img/cascadeless_schedule_ex1.png](https://github.com/devSquad-study/2023-CS-Study/raw/main/DB/img/cascadeless_schedule_ex1.png)

그래서 **strict scedule** 등장!

### strict scedule

> commit되지 않은 write한 데이터는 읽지도 쓰지도 않는다.
> 

좀 더 엄격한 스케줄이 나왔다.

- rollback할때 recovery가 쉬움 (tx 이전 상태로 되돌려놓기만 하면 됨)

### 정리

![https://github.com/devSquad-study/2023-CS-Study/raw/main/DB/img/recoverable_schedule.png](https://github.com/devSquad-study/2023-CS-Study/raw/main/DB/img/recoverable_schedule.png)

**[16. 트랜잭션을 병행으로 처리하려고 할 때 발생할 수 있는 문제는? 그리고 이를 해결하기 위한 해결방안은 무엇인가요?](https://goodbyeanma.tistory.com/80#16.%20%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98%EC%9D%84%20%EB%B3%91%ED%96%89%EC%9C%BC%EB%A1%9C%20%EC%B2%98%EB%A6%AC%ED%95%98%EB%A0%A4%EA%B3%A0%20%ED%95%A0%20%EB%95%8C%20%EB%B0%9C%EC%83%9D%ED%95%A0%20%EC%88%98%20%EC%9E%88%EB%8A%94%20%EB%AC%B8%EC%A0%9C%EB%8A%94%3F%20%EA%B7%B8%EB%A6%AC%EA%B3%A0%20%EC%9D%B4%EB%A5%BC%20%ED%95%B4%EA%B2%B0%ED%95%98%EA%B8%B0%20%EC%9C%84%ED%95%9C%20%ED%95%B4%EA%B2%B0%EB%B0%A9%EC%95%88%EC%9D%80%20%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%EC%9A%94%3F-1)**
 

**동시의 하나의 데이터가 갱신될 때 하나의 갱신이 누락될 수 있고** 

**하나의 데이터 갱신이 끝나지 않은 시점에서** 
 
**다른 트랜잭션이 해당 데이터를 조회할 수 있습니다.**

**또한 두 트랜잭션이 동시에 실행될 때 데이터베이스가 일관성이 없는** 
 
**모순된 상태로 남는 문제가 발생할 수 있으며**

**두 트랜잭션이 하나의 레코드를 갱신할 때 하나의 트랜잭션이 롤백하면 다른 하나의 트랜잭션** 
 
**마저 롤백이 되는 문제가 발생할 수 있습니다.**

**이를 해결하기 위해선 "로킹 제어 기법"을 사용하면 됩니다.**

**어떤 트랜잭션이 특정 db의  데이터를 사용할 때 db의 일정 부분을 lock 시키고** 
 
**트랜잭션이 완료될 때 해당 부분을 unlock 시키는 것입니다.**