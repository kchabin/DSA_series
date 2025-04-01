# Connected Component
그래프에서 `Connected Component`란, 내부의 모든 노드가 서로 연결되어 있지만, 다른 노드들과는 연결되지 않은 하나의 독립적인 부분 그래프(subgraph)를 의미한다.

![image](https://github.com/user-attachments/assets/5a4c45d2-21d6-4339-bbbc-717f16c3a1a6)
위 예시에서는 총 4개의 connected component가 존재하고, 각 컴포넌트가 모여 single graph를 구성한다.

### Applying -> Traversal
노드 방문 여부를 체크하는 배열 vis의 사이즈를 11로 한다.

![image](https://github.com/user-attachments/assets/a133ddc6-8c84-4851-aced-df1cca8db5d3)

```
for ( i=1 -> 10 ) {
  if (!vis[i])
    traversal(i)
}
```

i=1일 때, 노드 1에 접근해서 방문하지 않았으면 해당 노드를 방문하고, 연결된 노드를 순회(Traversal)한다. 1, 2, 3, 4가 연결된 컴포넌트이기 때문에 노드 1을 방문하면 `vis[1], vis[2], vis[3], vis[4]`의 값이 0에서 1로 바뀐다.
i=5가 되면, 아직 방문하지 않았기에, 노드 1에 접근했을 때처럼 해당 노드와 연결된 노드들을 방문처리한다.
