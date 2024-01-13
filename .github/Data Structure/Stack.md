## **Stack (스택)**

### 특징

데이터가 일렬로 나열되어 저장되는 선형(linear) 자료구조이다.

한쪽 끝에서 삽입, 삭제가 이루어지는 후입선출(LIFO, Last in First Out) 구조를 가진다.

스택에 데이터가 삽입될 위치를 Top이라고 한다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/1eec8832-fa93-4c7d-9472-9f11a4a4a946/89f879f4-da9f-4acd-b79f-eb0f253ecf19/Untitled.png)

### 기본 연산

- push : 스택의 top에 데이터를 삽입
- pop : 스택의 top에 위치한 요소를 제거
- isEmpty : 스택이 비어있는지 확인
- isFull : 스택이 꽉 찼는지 확인
- peek or top : 스택의 top에 위치한 요소를 반환

### 시간복잡도

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/1eec8832-fa93-4c7d-9472-9f11a4a4a946/9b818387-2c23-45ce-81d0-6c4111b33893/Untitled.png)