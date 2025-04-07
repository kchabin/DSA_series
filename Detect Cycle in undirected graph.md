# Detect a Cycle in an Undirected Graph

## BFS
### Cycle
그래프가 한 노드에서 시작해서, 같은 노드에서 끝나는 게 사이클.
BFS, DFS로 구현할 수 있다.

### BFS
Breadth First Search는 노드를 level-wise로 방문하는 traversal 기법이다. 동시에 같은 level의 노드들을 방문한 뒤, 다음 level로 이동한다.
- 한 노드에서 시작, BFS를 레벨 별로 수행한다.
- 탐색 도중 어느 시점에서든 하나의 노드를 두 번 방문 -> 두 개의 다른 경로를 통해 동일한 노드에 도달했다는 것을 의미한다.
- 이는 그래프에 사이클이 존재한다는 의미이다.
- 그래프가 연결되어 있거나 사이클을 포함하고 있는 경우에만 동일한 노드에 다시 도달할 수 있다.

## Approach
### Initial configuration
- Queue: <출발 노드, 부모 노드> 형태로 큐에 노드 쌍을 추가한다.
  - 예) (2, 1) 은 2번 노드가 현재 노드, 부모 노드가 1번 노드라는 것을 의미한다.
- Visited array: 방문 여부를 확인하는 배열. 0으로 초기화되어있고, 방문한 노드는 1로 표시한다.

### Algorithm steps
- BFS는 큐 자료구조와 visited 배열이 필요하다.
- <source, parent> 노드 쌍을 큐에 추가하고, 해당 노드를 방문한 노드로 마킹한다. 부모 노드는 역방향 탐색 없이 순방향으로만 탐색하기 위해서 필요하다.
- BFS 순회를 시작하고, 큐에서 요소를 매번 출력, 인접 리스트를 사용해 해당 노드의 아직 방문하지 않은 이웃 노드로 이동한다.
- 큐가 비게 되거나, 탐색 중 서로 다른 방향으로 이동했음에도 부모 노드가 아닌데 이미 방문 처리된 노드가 나타날 때까지 이 단계를 반복한다.
  - 이는 그래프에 사이클이 존재한다는 것을 나타낸다.
<img width="817" alt="스크린샷 2025-04-07 오전 12 45 00" src="https://github.com/user-attachments/assets/5ca9993b-3ada-4d97-a704-258d4d6af742" />

위 그림에서 5번 노드를 큐에서 뽑아내서 인접 리스트를 확인하면 부모 노드인 2는 이미 방문 처리되어서 무시하고, 아직 인접하지 않은 노드인 7번 노드로 이동하게 된다. 
이후 비슷한 단계를 반복하다가 6번 노드를 큐에서 출력해서 인접 리스트를 확인하고 이동하려하면 3은 부모 노드라 이미 방문한 노드이기 때문에 갈 수 없다.
다른 인접 노드인 7번 노드는 **부모 노드가 아니지만, 이미 방문 처리된 노드**이므로 위에서 말한 종료 조건에 부합한다. 따라서 현재 예시 그래프에는 사이클이 존재한다.

- 만약 큐가 비게 되고, 특정 조건을 만족하는 노드를 찾지 못했다면 그래프에 사이클이 없다는 의미 -> `false` 리턴
```
#check for connected components in a graph
for i in range(n):
  if not vis[i]:
    if detectCycle(i):
      return true
return false
```
### code
```
#src 는 탐색을 시작할 노드. adj: 그래프의 인접리스트, vis: 노드의 방문 여부 저장
class Solution:
  def detect(self, src, adj, vis):
    vis[src] = 1 #시작 노드 방문 표시
    queue = deque([(src, -1)]) #시작 노드와 부모 노드(-1)를 튜플로 큐에 넣기
    
    while queue:
       
      current_node, parent_node = queue.popleft()

      #node의 인접 노드 확인
      for adj_node in adj[current_node]:
        if not vis[adj_node]:
          vis[adj_node] = 1
          queue.append((adj_node, current_node))  #현재 노드가 부모 노드, 현재 노드의 인접 노드를 현재 노드로 하는 튜플 추가
        #부모 노드가 아닌데 방문 처리된 경우
        elif parent_node != adj_node:
          return True
    return False

# V : 그래프의 노드 수
  def isCycle(self, num_nodes, adj_list):
    visited = [0] * num_nodes
    #return self.detect(1, adj_list, visited)

    #connected components 고려 -> 여러 개의 분리된 컴포넌트로 구성된 경우에도 올바르게 사이클을 감지할 수 있도록 함
    for i in range(num_nodes):
      if not vis[i]:
        if self.detect(i, adj_list, visited):
          return True
    return False
```
parent_node와 adj_node를 비교하는 이유. (6, 3)을 큐에서 꺼내고, 부모 노드가 아닌 7에 접근하는 경우 7이 이미 방문 처리가 되어있는데 6의 부모 노드가 아닌 경우 이 그래프에 사이클이 존재한다는 의미가 됨.

** self는 메서드를 호출하는 Solution 클래스의 인스턴스를 가리킨다.
- `self.detect()`는 `Solution` 클래스의 현재 인스턴스에서 `detect` 메서드를 호출한다.
- `self` 없이 메서드를 호출하면, 파이썬은 `detect`를 전역 함수 또는 다른 범위의 함수로 찾으려고 시도한다.
- `detect`는 클래스 내부에 정의된 메서드이므로, 전역 범위에서 찾을 수 없어 NameError가 발생한다.

## Complexity
### Time Complexity
1. `detect(self, src, adj, vis)`
모든 노드, 간선을 최대 한 번씩 방문 -> sum of degrees -> `O(N+2E)` -> 계수 무시 `O(N+E)`
- 모든 인접 노드의 합은 차수의 합과 같다.
- N은 그래프의 노드 수, E는 간선 수.

2. `isCycle(self, num_nodes, adj_list`
- 모든 노드를 순회하면서 detect 함수 호출.
- 최악의 경우, 모든 노드에 대해 `detect` 함수가 호출됨 -> `O(N*(N+E))`
- 하지만, 각 연결된 요소에 대해서 한 번씩 detect 함수가 실행된다. -> 실제로는 `O(N+E)`
  - 그래프가 하나의 연결된 요소로 구성된 경우, detect 함수는 한 번만 호출됨
  - connected components로 구성된 경우에도, 각 요소에 대해 한 번씩 detect 함수가 호출됨
  
![image](https://github.com/user-attachments/assets/1964ac00-ae3b-41a2-9b69-94bb30cad1e4)

- 요소 1: `O(V1 + E1)`
- 요소 2: `O(V2 + E2)`
- 요소 3: `O(V3 + E3)`
- `O(V1 + E1) + O(V2 + E2) + O(V3 + E3) = O(V1 + V2 + V3 + E1 + E2 + E3) = O(V + E)`
- 전체 시간 복잡도는 `O(V+E)`
### Space Complexity
- visited 리스트: O(N)
- queue : O(N)

# Detect a Cycle in an Undirected Graph using DFS

## pseudo code
```
dfs(node, parent):
  vis[node]=1
  for i in adj[node]:
    if not vis[i]:
      if dfs(i, node):
        return True
    #방문한 노드인데, 부모 노드는 아닌 경우
    elif parent != i:
        return True
  return False 
```

## code
```
class Solution:
  def dfs(self, node, parent, adj, vis):
    #현재 노드 방문 처리
    vis[node] = 1
    for neighbor in adj[node]:
      if not vis[neighbor]:
        if dfs(neighbor, node, adj, vis):
          return True
      elif parent != neighbor:
          return True
    return False
     

  def isCycle(self, num_nodes, adj_list):
    visited = [0] * num_nodes
    #return self.dfs(1, -1, adj, vis)

    #connected components 고려
    for i in range(num_nodes):
      if not visited[i]:
        if self.dfs(i, -1, adj_list, visited):
          return True
    return False
```
![image](https://github.com/user-attachments/assets/2d475708-2b3c-4c95-aad0-f883afed9044)

connected component일 경우 각 컴포넌트의 첫번째 노드에서만 `dfs()`를 호출한다.
dfs(1)을 호출했을 때 2, 3 노드 모두 방문하게 되기 때문에 dfs(1) 다음에 dfs(2), dfs(3)는 넘어가고 바로 노드 4로 간다.

dfs(1, -1, adj, vis), dfs(4, -1, adj, vis), dfs(8, -1, adj, vis) 이런 식으로 호출된다.

모든 노드가 연결된 하나의 그래프라면 dfs(시작노드, -1, adj, vis)만 해도 알아서 모든 노드를 방문하게 된다. dfs()는 한 번만 호출된다.

recursion stack -> O(N)
visited array -> O(N) 
Space Complexity ->> O(N)+O(N) ~ O(N)
Time Complexity -> O(N+2E) + O(N)
- 모든 노드 접근 + dfs 함수는 한번만 호출되거나 각 연결된 컴포넌트에 대해서 한 번씩만 호출되기 때문에 전체적으로는 for loop 하나 정도의 시간 복잡도를 가진다. 
