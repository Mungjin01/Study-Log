
## 너비 우선 탐색(BFS, Breadth-First Search)

: 다차원 배열에서 각 칸을 방문할 때 너비를 우선으로 방문하는 알고리즘

: 루트 노드(혹은 다른 임의의 노드)에서 시작해서 인접한 노드를 먼저 탐색하는 방법

쉽게 말하면

시작 정점으로부터 가까운 정점을 먼저 방문하고 멀리 떨어져 있는 정점을 나중에 방문하는 순회 방법(깊게 탐색하기 전에 넓게 탐색)

너비 우선 탐색을 하기 위해서는 방문한 정점들을 차례로 저장한 후 꺼낼 수 있는 자료 구조인 큐가 필요하다.

⇒ 정점이 방문될 때마다 큐에 방문된 정점을 삽입하고 더 이상 방문할 인접 정점이 없는 경우 큐의 앞에서 정점을 꺼내서 그 정점과 인접한 정점들을 모두 차례대로 방문

```cpp
#include<iostream>
#include<queue>
using namespace std;
int map[10][10]={0};
int visit[10]={0};
queue<int> q;
int num;
 
void bfs(int v){
    cout<<v<<" ";
    q.push(v);
    while(!q.empty()){
        int node = q.front();
        q.pop();
        for(int i=0;i<num;i++){
            if(map[node][i]==1 && visit[i]==0){
                visit[node]=1;
                cout<<i<<" ";
                q.push(i);
            }
        }
    }
}
int main(){
    cin>>num;
    while(1){
        int v1,v2;
        cin>>v1>>v2;
        if(v1==-1&&v2==-1)
            break;
        map[v1][v2]=map[v2][v1]=1;
    }
    bfs(1);
    return 0;
}
```

![BFS1.jpg](https://i.esdrop.com/d/f/AfOYjCl4ON/QkLgSfaR9N.jpg)

**너비 우선 탐색(BFS)의 시간 복잡도**

- 인접 리스트로 표현된 그래프: O(N+E)
- 인접 행렬로 표현된 그래프: O(N^2)

깊이 우선 탐색(DFS)과 마찬가지로 그래프 내에 적은 숫자의 간선만을 가지는 희소 그래프(Sparse Graph) 의 경우 인접 행렬보다 인접 리스트를 사용하는 것이 유리하다.