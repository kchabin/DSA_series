# BFS of Graph
Given a connected undirected graph containing V vertices, represented by a 2-d adjacency list adj[][], where each adj[i] represents the list of vertices connected to vertex i. Perform a Breadth First Search (BFS) traversal starting from vertex 0, visiting vertices from left to right according to the given adjacency list, and return a list containing the BFS traversal of the graph.

Note: Do traverse in the same order as they are in the given adjacency list. 

Input: adj[][] = [[2, 3, 1], [0], [0, 4], [0], [2]]
![image](https://github.com/user-attachments/assets/a30b934f-ef60-4033-93c7-bf96d5fb5704)

Output: [0, 2, 3, 1, 4]
Explanation: Starting from 0, the BFS traversal will follow these steps: 
Visit 0 → Output: 0 
Visit 2 (first neighbor of 0) → Output: 0, 2 
Visit 3 (next neighbor of 0) → Output: 0, 2, 3 
Visit 1 (next neighbor of 0) → Output: 0, 2, 3, 
Visit 4 (neighbor of 2) → Final Output: 0, 2, 3, 1, 4


2d matrix 형태의 인접 리스트가 주어질 때, adj matrix의 인덱스가 노드 번호, 각 인덱스의 1차원 리스트가 인접 노드 리스트가 된다.
BFS는 level-wise traversal 기법이므로 시작 노드인 0에서 1만큼 떨어져 있는 2, 3, 1을 방문하고 2만큼 떨어진 level2의 4를 방문한다.

BFS를 구현하려면 queue 자료 구조와 visited array가 필요하다.
1. 시작 노드를 방문 처리하고, 큐에 넣는다. 이 문제에서는 시작 노드가 0이므로 `vis[0]=1`이고, collections의 deque를 사용해서 큐를 구현하면 시작 노드를 큐에 넣는 건 `queue = deque([start])` 로 표현할 수 있다.
2. 큐가 빌 때까지 다음 과정을 반복한다.
   - 큐에서 노드를 하나 꺼낸다
   - 꺼낸 노드의 모든 인접 노드를 확인한다.
   - 방문하지 않은 인접 노드를 큐에 넣고 방문 표시한다.
  

  
```
from collections import queue

class Solutions:

  def bfs(self, adj):
    result = [] #결과 저장 배열
    vis = [0] * len(adj) #방문 여부
    start = 0

    queue = deque([start]) #큐에 시작 노드를 넣는다.
    vis[start] = 1

    while queue:
      node = queue.popleft()
      result.append(node)

      for neighbor in adj[node]:
          if not vis[neighbor]:
            queue.append(neighbor)
            vis[neighbor] = 1
    return result
```

# DFS of Graph
DFS(깊이 우선 탐색)는 재귀와 백트래킹 개념을 사용하는 탐색 기법이다. 
![image](https://github.com/user-attachments/assets/249d0ff9-0543-4821-93ba-0ec390003744)
이 그래프의 인접 리스트를 표현하면 다음과 같다.
- 1 -> [2, 3]
- 2 -> [1, 5, 6]
- 3 -> [1, 4, 7]
- 4 -> [3, 8]
- 5 -> [2]
- 6 -> [2]
- 7 -> [3, 8]
- 8 -> [4, 7]

### recursive traversal
![image](https://github.com/user-attachments/assets/35c6fdb3-c3ce-4cb0-90a7-3e222d642eba)

노드 1의 인접 노드 2, 3을 탐색할 때, bfs는 2, 3을 동시에 탐색하지만 dfs는 먼저 노드 2와 2의 인접 노드들까지 다 탐색 한 후에 돌아와서(backtracking) 노드 3를 탐색한다.
### pseudo code - bfs
```
dfs(node)
{
  vis[node]=1
  result.add(node)

  for i in arr[node]:
    if !vis[i]:
      dfs(i)
}
```
## DFS of graph
Given a connected undirected graph containing V vertices represented by a 2-d adjacency list adj[][], where each adj[i] represents the list of vertices connected to vertex i. Perform a Depth First Search (DFS) traversal starting from vertex 0, visiting vertices from left to right as per the given adjacency list, and return a list containing the DFS traversal of the graph.

Note: Do traverse in the same order as they are in the given adjacency list.

Input: adj[][] = [[2, 3, 1], [0], [0, 4], [0], [2]]

Output: [0, 2, 4, 3, 1]
Explanation: Starting from 0, the DFS traversal proceeds as follows:
Visit 0 → Output: 0 
Visit 2 (the first neighbor of 0) → Output: 0, 2 
Visit 4 (the first neighbor of 2) → Output: 0, 2, 4 
Backtrack to 2, then backtrack to 0, and visit 3 → Output: 0, 2, 4, 3 
Finally, backtrack to 0 and visit 1 → Final Output: 0, 2, 4, 3, 1

```
class Solution:
  def check(self, node, vis, result, adj):
    vis[node]=1
    result.append(node)

    for n in adj[node]:
      if not vis[n]:
        self.check(n, vis, result, adj)

  def dfs(self, adj):
    result = []
    vis = [0] * len(adj) #노드 방문 여부 기록 adj 리스트의 길이가 노드의 개수가 된다.
    start = 0 #문제에서 시작 노드는 0

    self.(start, vis, result, adf)

    return result
```

dfs는 시작 노드를 방문처리한 다음 해당 노드의 방문하지 않은 인접 노드로 재귀 호출한다는 점
bfs는 queue를 사용하고, 이 큐가 빌 때까지 큐에서 노드를 뽑아서 인접 노드들을 확인하고, 방문하지 않은 노드는 큐에 넣고 방문처리한다는 점을 잘 기억해야 한다.

두 알고리즘 모두 visited array가 필요하다. 파이썬에서는 리스트나 set을 사용한다. 근데 일단 나는 코드를 무작정 외우는 것보다 동작 과정을 이해하고 의사코드를 만든 뒤 그걸 파이썬으로 구현하기 위해 애썼다...

~~이해 못하면 안 외워짐~~
