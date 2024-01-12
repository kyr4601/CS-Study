## 그래프 (Graph)

### 정의 및 특징

- **정점(V, vertex)**과 정점들을 연결하는 **간선(E, edge)**으로 이루어진 자료구조 → G = (V, E)
- 현실세계의 사물이나 개념 간의 **연결 관계**를 수학적 모델로 단순화하여 표현한 것
- 특징
    - 네트워크 모델이다
    - 트리는 그래프의 일종임, 하지만 그래프는 트리와는 달리 정점마다 간선이 있을 수도 있고 없을 수도 있으며, 루트 노드와 부모와 자식이라는 개념이 존재하지 않는다
    - 순회는 DFS(깊이우선탐색)나 BFS(너비우선탐색)로 이루어진다
    

### 용어

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad4423d4-83c8-483d-8639-74d9c4884bef/2d658d87-0afc-44d7-a8ac-0d4bd3992fb8/Untitled.png)

- **정점 (vertex)** = 노드(node) : 데이터가 저장되는 그래프 기본 원소
- **간선 (edge)** : 정점을 연결하는 선, link,branch라고도 부름
- **인접 정점 (adjacent vertex)** : 간선에 의해 직접 연결된 정점 (0과 2는 서로 인접 정점)
- **단순 경로 (simple path)** : 동일한 간선을 지나지 않는 경로, 경로 중에서 반복되는 정점이 없는 경로 ( 0→3→2→1 은 단순경로 )
- **차수 (degree)** : 무방향 그래프에서, 한 정점에 인접한 정점의 수 (0의 차수는 3)
- **진출 차수 (out-degree)** : 방향 그래프에서, 한 정점에서 외부로 나가는 간선의 수
- **진입 차수 (in-degree)** : 방향 그래프에서, 외부에서 한 정점으로 들어오는 간선의 수
- **단순 경로 (Simple Path)** : 같은 정점을 두 번 이상 가지 않는 경로
- **사이클 (cycle)** : 단순 경로에서 시작 정점과 종료 정점이 동일한 경우

### 종류

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad4423d4-83c8-483d-8639-74d9c4884bef/8b9a353b-2b5f-43d6-9e9c-5ce8c36de118/Untitled.png)

- **무방향 그래프 (Undirected Graph)** : 방향이 없는 간선들로 이루어진 그래프
- **방향 그래프 (Directed Graph)** : 방향이 존재하는 간선들로 이루어진 그래프
- **가중치 그래프 (Weighted Graph)** : 가중치(비용)를 갖는 간선들로 이루어진 그래프
- **연결 그래프 (Connected Graph)** : 임의의 두 노드를 선택했을 때 경로가 존재하는 그래프, 트리가 대표적인 예
- **비연결 그래프(Disconnected Graph)** : 임의의 두 노드를 선택했을 때 경로가 존재하지 않는 경우가 있는 그래프
- **완전 그래프 (Complete Graph)** : 모든 정점끼리 서로 인접 연결돼있는 그래프
- **부분 그래프 (SubGraph)** : 본래의 그래프의 일부 정점 및 간선으로 이루어진 그래프 (부분 집합과 유사한 개념)
- **순환 그래프 (Cycle Graph)** : 단순 경로에서 시작 정점과 종료 정점이 동일한 경우(사이클)가 존재하는 그래프
- **비순환 그래프 (Acyclic Graph)** : 사이클이 없는 그래프

### Graph 구현 방식

> 정점 개수 : V개, 간선 개수 : E개
> 
- **인접 행렬 (Adjacency Matrix)** : 2차원 배열을 이용하는 방식
    - V x V 2차원 배열 A에 정보를 저장한다.
    - Vi, Vj를 연결하는 간선이 존재한다면 A[i][j] = 1, 존재하지 않는다면 A[i][j] = 0
    - 가중치 그래프의 경우, 1 대신 가중치 정보를 저장한다.
    
    !https://github.com/Seogeurim/CS-study/raw/main/contents/data-structure/img/%EA%B7%B8%EB%9E%98%ED%94%84.002.jpeg
    
- **인접 리스트 (Adjacent List)** : 연결 리스트를 이용하는 방식
    - V개의 Linked List로 그래프 정보를 저장한다.
    - 가중치 그래프의 경우, (정점 정보, 가중치 정보)를 함께 저장한다.
    
    !https://github.com/Seogeurim/CS-study/raw/main/contents/data-structure/img/%EA%B7%B8%EB%9E%98%ED%94%84.003.jpeg
    
- **인접 행렬** vs **인접 리스트**
    - 인접 행렬
        - 😫 모든 정점들의 연결 여부를 저장해, **O(V^2)**의 메모리 공간을 요구
        - 😊 두 노드의 연결 여부를 확인하기 위해, **O(1)**의 시간이 필요
        - Dense graph(간선이 많음)를 표현할 때 적절
    - 인접 리스트
        - 😊 연결된 간선의 정보만을 저장하여, **O(V+E)**의 메모리 공간을 요구
        - 😫 두 노드의 연결 여부를 확인하기 위해, **O(V)**의 시간이 필요
        - Sparse graph(간선이 적음)를 표현할 때 적절
            
            ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad4423d4-83c8-483d-8639-74d9c4884bef/cffcda41-bd24-460a-84be-98747916e255/Untitled.png)