# [BFS] 특정 거리의 도시 찾기 - 18352(Silver II)

### [문제 링크](https://www.acmicpc.net/problem/18352)

<br>

### ✔️ 분류

너비 우선 탐색, 데이크스트라, 그래프 이론, 그래프 탐색, 최단 경로

<br>

### ✅ 리뷰

BFS(너비우선탐색)을 활용한 문제 였다. <br>

쉬운 문제 였음에도 불구하고 해당 조건 처리를 제대로 신경쓰지 못하여 메모리 초과가 발생하였고, <br>

구현에 꽤 오랜시간을 투자했다. <br>

이에 메모리 초과가 발생했을 때 코드에서 잘못된 부분을 빠르게 파악하기 위한 포인트를 리뷰하고자 한다. <br>

1. 너무 많은 변수들을 배열 등에 저장할 경우. <br>
2. DFS,BFS 등에서 재귀적 호출을 통해 너무 많은 함수들을 호출할 경우. (무한 루프)

<br>

아마 해당 문제에서 내가 메모리 초과가 발생한 이유는 **방문한 곳에 대한 조건처리를 제대로 하지 못하여** 2번 조건에 해당 했던 것 같다.

<br>

### ✔️ 풀이 코드

<br>

**1. 최적화된 코드**

```python
from collections import deque

N, M, K, X = map(int, input().split())  # 도시의 개수 N, 도로의 개수 M, 거리 정보 K, 출발 도시의 번호 X

roads = [[] for _ in range(N + 1)]  # 간선 정보

for _ in range(M):
    A, B = map(int, input().split())
    roads[A].append(B)

distance = [-1] * (N + 1)  # 거리 정보 : 방문 기록 파악
distance[X] = 0  # 출발지는 방문 처리
queue = deque([X])

while queue:
    current = queue.popleft()
    for city in roads[current]:
        # 만약 방문하지 않았다면
        if distance[city] == -1:
            distance[city] = distance[current] + 1  # 방문 처리
            queue.append(city)

flag = 1

# K거리에 맞는 도시 출력
for i in range(N + 1):
    if distance[i] == K:
        flag = 0
        print(i)

if flag:
    print(-1)
```

<br>

2. 문제점을 제대로 파악하지 못하고 리스트가 아닌 딕셔너리 형태로 구성한 코드

```python
from collections import deque

N, M, K, X = map(int, input().split())

roads = {}

for _ in range(M):
    A, B = map(int, input().split())
    if A in roads:
        roads[A].append(B)
    else:
        roads[A] = [B]

distance = [0] * (N + 1)

queue = deque()
queue.append(X)
distance[X] = 1

start = True

while queue:
    if start == True:
        distance[X] = 0
        start = False
    else:
        distance[X] = 1
    current = queue.popleft()
    if current in roads:
        for city in roads[current]:
            if distance[city] == 0:
                distance[city] = distance[current] + 1
                queue.append(city)

flag = 1
distance[X] = 0

for i in range(N + 1):
    if distance[i] == K:
        flag = 0
        print(i)

if flag:
    print(-1)
```
