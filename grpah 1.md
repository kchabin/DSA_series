# Types of Graph
![image](https://github.com/user-attachments/assets/93a3f087-6417-4ce5-809a-ca8a74a5cdc7)
### Node / Vertex
그래프 구성 요소 중 '원'이 정점으로, node 혹은 vertex라고 불린다.
그림에 나와있는 것처럼 노드에 넘버링을 할 수 있고, 어떤 순서든지 작성할 수 있다.
### edge
connected horizontal line -> edge. 한국어로는 간선이라고 한다.
## Undirected Graph
무방향 그래프는 화살표가 없는 것이 특징이다.
- Undirected Edge(bidirectional)
  - u-v : u->v, u<-v 모두 가능

## Directed Graph
방향 그래프는 directed edge로 구성돼있다.
위 그림에서, 노드 1에서 노드 4로는 갈 수 있지만, 노드 4에서 노드 1로는 올 수 없다. 방향이 정해져 있기 때문이다.

# Cycle
그래프에서 순환, Cycle을 찾을 수 있다.
아래 예시는 무방향 그래프로, 사이클이 존재한다.
 
<img src="https://github.com/user-attachments/assets/6a948b19-856b-43f1-b07d-bb6c317a8981" width="500">

1. 1 - 2 - 3 - 5 - 4
2. 1 - 2 - 5 - 4
3. 2 - 3 - 5
   
<img src="https://github.com/user-attachments/assets/bf26c864-33f6-44d6-b721-254d7fc2dadf" width="500">

위 예시의 방향 그래프는 화살표를 따라 움직이기 때문에 사이클을 찾을 수 없다. 이러한 그래프를 DAG(Directed Acyclic Graph)라고 한다.
directed graph에서 반대방향으로 가는 edge를 두 노드 사이에 가질 수 있기 때문에 아래와 같이 4에서 1로 가는 간선을 표시해주면 방향 그래프에서도 사이클을 찾을 수 있게 된다.
![image](https://github.com/user-attachments/assets/a37c0e77-9622-4bf6-bf85-f3221add0a9c)


![image](https://github.com/user-attachments/assets/4c7ee7ba-cdd1-474a-aa70-b7d44abc4476)

이렇게 닫혀있지 않은 그래프도 그래프이다. 그래프는 open structure일 수 있다.
이진 트리도 그래프의 구성 요소인 node, edge가 존재하기 때문에 그래프의 일종이다.

# Path
![image](https://github.com/user-attachments/assets/a2ad1096-7234-4e27-b4eb-7f6efe02ca21)
path는 각각에 도달 가능한 노드들을 포함하고 있는 것을 의미한다.
위 그래프에서 1 2 3 5는 path가 될 수 있지만, 두 노드 사이에 edge가 없거나, 이미 등장한 노드가 다시 나오는 경우는 path로 볼 수 없다.

# Degrees in Graph
그래프의 차수는 노드로 들어오거나 노드에서 나가는 edge의 개수를 의미한다.
![image](https://github.com/user-attachments/assets/06a7573c-6543-4f97-b3e4-4dd9503ba563)
무방향 그래프에서는 전체 차수가 edge 개수에 2를 곱한 값이 된다. 이유는 간선 하나가 노드 두 개를 연결하기 때문이다.

![image](https://github.com/user-attachments/assets/00ce9c7f-9c21-4f49-86aa-75965b908cc0)
방향 그래프에서는 Indegree, Outdegree를 나눠서 계산한다. 들어오는 방향인 edge, 나가는 방향 edge를 각각 계산하면 된다.

# edge weight(가중치)
edge마다 특정한 비용(weight)이 있을 수 있다.
- 도로 네트워크 : 거리가 가중치
- 네트워크 그래프 : 데이터 전송 시간이 가중치
![image](https://github.com/user-attachments/assets/706df8f8-9a0a-4e0c-b635-5e74b8f8e57f)
Unit Weight은 1로 간주하는데, 이건 모든 간선의 가중치를 1로 고정한다는 뜻. 즉, 모든 경로의 이동 비용이 동일하다고 가정하는 것이다.

- SNS 친구 관계: 친구 관계를 그래프로 나타낼 때, 모든 친구 관계는 동등한 중요도를 가짐 → 모든 간선의 가중치를 1로 설정
- 미로 찾기: 한 칸씩 이동할 때 모든 이동의 비용이 동일하다고 가정 → Unit weight = 1
- 최단 경로 탐색: BFS(너비 우선 탐색)에서 모든 간선의 가중치를 동일하게 1로 두고 경로를 탐색
