# Directed Graph에서의 Cycle
| on the same path, node has to be visited again.

기본적인 코드는 지금까지 봤던 것과 동일함
```
for i in range(1, v):
  if not vis[i]:
    if dfs(i):
      return True
```
`i`는 1번 노드에서 10번 노드라고 가정한다. v=10

이전 dfs로 돌아갈때, `pathVisited[i]=1` -> `pathVisited[i]=0`으로 바꿔야 한다. 
- 강의에서는 omit, 토해낸다고 표현함
  
![스크린샷 2025-04-09 오전 12 27 56](https://github.com/user-attachments/assets/3cdd3ddc-095f-4b00-9e15-6d872004a79d)

그림에서 dfs(3)까지 가는데, 4, 5, 6 노드의 pathVisited 값을 1에서 0으로 바꿔야 하고, dfs(3)는 노드 7로 접근할 수 있기 때문에 아직은 'omit' 하지 않는다.

![스크린샷 2025-04-09 오전 12 30 05](https://github.com/user-attachments/assets/612fc01d-4d36-4f36-af4a-41dc6368e6f8)

dfs(7)을 호출해서 vis, pathvis 0 -> 1로 바꾼다. 7의 인접 노드 5로 이동하려고 할 때, vis[5]=1이고, pathvis[5]=0이라 이미 방문한 노드인데, 같은 경로도 아니라는 걸 알 수 있다.
-> dfs(1)은 최종적으로 `False` 리턴, vis[i]==0을 만족할 때까지 i 증가
이렇게 되면 위의 for문에서 i가 갱신된다. -> 시작 노드가 1에서 8로 바뀐다.

![스크린샷 2025-04-09 오전 12 36 06](https://github.com/user-attachments/assets/f703e61e-727d-487a-be6b-7e334d1753fd)

- vis 리스트에서 1부터 7까지 모두 1인 상태라 if 조건을 만족하지 못한다. -> 노드 스킵하고 다른 노드를 dfs 호출 시작 노드로 설정
- vis[8]=0이기 때문에 dfs(8)이 호출되면서 다시 한 번 dfs 탐색이 시작된다.

![스크린샷 2025-04-09 오전 12 39 03](https://github.com/user-attachments/assets/17ec0fd1-e803-401a-acfc-bfc8c5405e6e)

- 10번 노드를 vis, pathvis 모두 1로 처리하고, 인접 노드인 8을 확인한다.
- vis[8], pathvis[8]이 모두 1이므로 사이클이 존재한다.
- 최종적으로 True를 리턴하기 때문에 9, 10으로 i를 갱신할 필요 없이 반복문이 종료된다.

## Code
```
class Solution {

  private:
    bool dfsCheck(int node, vector<int> adj[], int vis[], int pathVis[]){
        vis[node]=1;
        pathVis[node]=1;
        
        for(auto it : adj[node]){
            if(!vis[it]){
                if(dfsCheck(it, adj, vis, pathVis)==true){
                    return true;
                }
            }
            else if(pathVis[it]){
                return true; 
                //if the adjacent node has been previously visited,
                // it has to be visited on the same path.
            }
        }
        pathVis[node] = 0; //omit node
         
        return false;
        
    }

  public:
    bool isCyclic(int V, vector<vector<int>> adj[]) {
        // code here
        int vis[V] = {0};
        int pathVis[V] = {0};
        
        for(int i = 0; i<V; i++){
            if(!vis[i]){
                if(dfsCheck(i, adj, vis, pathVis)==true) return true;
            }
        }
    }
};

 
```
