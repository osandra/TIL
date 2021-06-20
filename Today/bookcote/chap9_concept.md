### chap9. `Graph`
---
#### **`다익스트라 알고리즘`**
특정한 노드에서 출발하여 다른 노드로 가는 각각의 최단경로를 구해주는 알고리즘. (음의 간선이 없을 경우 사용)

최단 거리 테이블을 사용하여 출발 노드에서 해당 노드까지의 최단 거리 값을 설정함.

처음엔 최단 거리 테이블 값을 무한대로 초기화 한 뒤, 방문한 노드와 연결된 노드에 한해서만 해당 인덱스 값을 업데이트한다. 따라서 처음 출발노드로의 거리는 0으로 설정.

업데이트의 경우, 기존의 값보다 방금 방문한 노드의 거리 값 + 가중치가 작을 때만 실행함.

즉 정점 집합부터 시작해 순서대로 가장 작은 거리 값을 가진 노드를 선택하고, 선택한 노드와 연결된 노드를 업데이트한다. 모든 노드를 방문할 때까지 업데이트 과정을 반복하면 각 단계마다 하나의 정점은 시작 정점으로부터 자신으로까지의 최단 경로 값이 결정된다. 결과적으로 모든 노드가 시작 정점으로부터의 최단 경로 값을 가지게 딘다.

최단 거리 테이블에서 가장 작은 값을 선형 탐색하면 노드 개수만큼 선형 탐색하는 시간이 걸리므로, 힙 자료 구조를 이용해 최단거리 관련 정보를 저장하면 시간 복잡도 단축된다.

- 우선순위 큐: 가장 높은 데이터를 가장 먼저 삭제. 파이썬에서 제공하는 힙 라이브러리는 min heap을 지원하므로 가장 값이 낮은 데이터가 먼저 나온다. 
따라서 (거리, 노드) 순으로 큐에 삽입하면 거리가 낮은 순서대로 pop된다.


```python
import heapq
import sys

input = sys.stdin.readline
INF = int(1e9)
n,m = map(int, input().split())
start = int(input()) # 시작노드 번호

# graph: 각 노드에 연결된 (노드, 가중치)를 순서대로 저장함
graph = [[] for _ in range(n+1)]
distance = [INF] * (n+1)

#모든 간선 정보 입력받기
for _ in range(m):
    a,b,c = map(int, input().split())
    graph[a].append((b,c))

def dijkstra(start):
    q = []
    heapq.heappush(q,(0,start)) 
    #0(거리) start(노드 인덱스) 거리가 짧은 순대로 정렬됨(최소 힙)
    distance[start] = 0 # 시작노드의 거리는 0
    while q: #큐가 비어있지 않는 동안
        dist,now = heapq.heappop(q) 
        if distance[now] < dist: # 기존에 있는 해당 노드의 최단 거리보다 길다면, 볼 필요가 없음
            continue # 이유: 이전 업데이트에서 거리가 갱신되서 push 되었으나 이후 더 짧은 거리로 갱신된 후 push 되었으면 이미 이 전에서 해당 노드 관련 업데이트를 진행했으니 읽을 필요가 없음.
        #업데이트
        for j in graph[now]:
            cost = distance[now]+ j[1]
            if cost < distance[j[0]]: #경로 값이 갱신될 때만 push
                distance[j[0]] = cost
                heapq.heappush(q,(cost,j[0])) #heapq는 우선순위 큐니까 q 가 낮은 순서대로 나감(pop)
dijkstra(start)

#최단 거리 출력 한 번 최단거리로 속한 이후엔 업데이트가 없으니까
for i in range(1,n+1):
    if distance[i] == INF:
        print("INFINITY")
    else:
        print(distance[i])

```
---
<br>

#### **`플로이드워셜 알고리즘`**

모든 지점에서 다른 모든 지점까지의 최단 경로를 모두 구해야 하는 경우.
모든 노드에 대하여 다른 모든 노드로 가는 최단 거리 정보를 저장하기에 `2차원 리스트`에 최단거리 정보를 저장한다.

1,2,3,4,5 노드가 있다. 만약 3번 노드 -> 5번 노드로 가는 최단 경로를 찾을 때 1번 노드를 거쳐가는 경우, 2번 노드를 거쳐가는 경우, 4번 노드를 거쳐가는 경우, 아무런 노드도 거치지 않고 3번->5번으로 바로 가는 경우 등을 확인해 최단경로를 알아내는 방법이다.

> 즉 이는 3->5로 가는 최소 비용과 3->특정 노드->5로 가는 비용을 비교하여 더 작은 값으로 갱신해가는 방법이다.


```python
INF = int(1e9) #무한을 의미하는 값
# 노드 , 간선 개수 입력 받기
n = int(input())
m = int(input())

# 2차원 그래프의 모든 값 INF로 초기화
graph = [[INF] * (n+1) for _ in range(n+1)] 
#for i in range(n+1)에서 i 안 쓰니까 _ 로 생략 가능

#자기 자신에서 자기 자신으로 가는 비용은 0으로 초기화
for a in range(1,n+1):
    for b in range(1,n+1):
        if a==b:
            graph[a][b] = 0
            
#각 각선에 대한 정보를 입력받아 해당 값으로 그래프 초기화
for _ in range(m):
    a,b,c = map(int, input().split())
    graph[a][b] = c #a->b의 cost는 c

#점화식에 따라 플로이드 워셜 알고리즘 진행
#k는 거쳐가는 노드임 즉 1번 노드부터 끝 노드까지 자신을 거쳐가는 경로에서 최소비용이면 업데이트 하는 방식
for k in range(1,n+1):
    for a in range(1,n+1):
        for b in range(1,n+1):
            graph[a][b] = min(graph[a][b],graph[a][k]+graph[k][b])
            #k를 지나는 경우, k를 지나지 않는 경우

# 이제 그래프엔 모든 경로 최소 거리가 적혀있음
# [2][4]에는 2번 노드에서 4번 노드로 가는 최단 거리

# 그래프 값 촐력: 만약 경로가 없으면 INFINITY 출력, 경로 있으면 비용 출력
for a in range(1,n+1): #1번 노드부터 끝 노드까지
    for b in range(1,n+1):
        if graph[a][b] == INF:
            print("INFINITY",end=" ")
        else:
            print(graph[a][b], end = " ")
    print()
```



*+++*
```python
# a 라는 값이 b보다 크면 a 값으로를 b값으로 설정하고자 할 때 2번 방법으로 작성하면 한 줄로 가능

# 1)
if b > a:
    a = b

# 2
a = max(a,b)
```

#### 출처: 이것이 취업을 위한 코딩 테스트다 with 파이썬