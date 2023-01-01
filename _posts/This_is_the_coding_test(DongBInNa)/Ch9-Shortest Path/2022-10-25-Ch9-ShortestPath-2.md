---

layout: single
title: "Ch9 Shortest Path 9-2"
categories: Algorithm
tag: [Python, Algorithm, 이것이 코딩 테스트다]
toc: true
toc_sticky: true
author_profile: false
sidebar:
    nav: "docs"

---

## ⚡ [문제 1] 미래 도시

* **난이도: 🌕🌕**
* **풀이시간: 40분**
* **시간 제한: 1초**
* **메모리 제한: 128MB**
* **기출: M 기업 코딩 테스트**

**[문제]**

방문 판매원 A는 많은 회사가 모여 있는 공중 미래 도시에 있다. 공중 미래 도시에는 1번부터 N번까지의 회사가 있는데 특정 회사끼리는 서로 도로를 통해 연결되어 있다. 방문 판매원 A는 현재 1번 회사에 위치해 있으며, X번 회사에 방문해 물건을 판매하려 한다. 

공중 미래 도시에서 특정 회사에 도착하기 위한 방법은 회사끼리 연결되어 있는 도로를 이용하는 방법이 유일하다. 또한 연결된 2개의 회사는 양방향으로 이동할 수 있다. 이 때, 다른회사로 이동하기 위한 시간은 정확히 1만큼의 시간이 소요된다.

또한 오늘 A씨는 소개팅에도 참여해야 한다. 소개팅의 상대는 K번 회사에 존재한다. 방문 판매원 A는 X번 회사에 물건을 팔러 가기 이전에 소개팅 상대의 회사에 가서 함께 커피를 마실 예정이다. 따라서 A씨는 1번 회사에서 출발해 K번 회사를 거쳐 X번 회사로 가는 것이 목표다. 이 때 A씨는 가장 빠르게 이동하려고 한다. A씨가 이동하게 되는 최소 시간을 계산하는 프로그램을 작성해라. 이 때 소개팅의 상대방과 커피를 마시는 시간 등은 고려하지 않는다.

**[예시]**
```python
# N=4, X=4, K=5, 회사 간 도로가 7개
(1번, 2번), (1번, 3번), (1번, 4번), (2번, 4번), (3번, 4번), (3번, 5번), (4번, 5번)

# 최소 이동 시간은 3
(1번 - 3번 - 5번 - 4번)
```

**[입력 조건]**

* 첫째 줄에 전체 회사의 개수 N과 경로의 개수 M이 공백으로 구분되어 차례대로 주어진다.(1 <= N, M <= 100)
* 둘째 줄부터 M+1 번째 줄에는 연결된 두 회사의 번호가 공백으로 구분되어 주어진다.
* M+2 번째 줄에는 X와 K가 공백으로 구분되어 차례대로 주어진다.(1 <= K <= 100)

**[출력 조건]**

* 첫째 줄에 방문 판매원 A씨가 K번 회사를 거쳐 X번 회사로 가는 최소 이동 시간을 출력한다.
* 만약 X번 회사에 도달할 수 없다면 -1을 출력한다.

```python  
# 입력 예시 1
5 7
1 2
1 3
1 4
2 4
3 4
3 5
4 5
4 5
# 출력 예시 1
3

# 입력 예시 2
4 2
1 3
2 4
3 4
# 출력 예시 2
-1
```

**[문제 해설]**

* 시간복잡도 $O(N^3)$ 최대 500개 가능, N <= 100 플로이드 워셜 충분
* 기본적인 플로이드 워셜 알고리즘과 비슷하다.
* 최단 거리 문제는 그림으로 먼저 그려보는 것도 좋은 방법

```python

# 교재 풀이

INF = int(1e9) # 무한을 의미하는 값으로 10억을 설정

# 노드의 개수 및 간선의 개수를 입력받기
n, m = map(int, input().split())
# 2차원 리스트(그래프 표현)를 만들고, 모든 값을 무한으로 초기화
graph = [[INF] * (n + 1) for _ in range(n + 1)]

# 자기 자신에서 자기 자신으로 가는 비용은 0으로 초기화
for a in range(1, n + 1):
    for b in range(1, n + 1):
        if a == b:
            graph[a][b] = 0

# 각 간선에 대한 정보를 입력 받아, 그 값으로 초기화
for _ in range(m):
    # A와 B가 서로에게 가는 비용은 1이라고 설정
    a, b = map(int, input().split())
    graph[a][b] = 1
    graph[b][a] = 1

# 거쳐 갈 노드 X와 최종 목적지 노드 K를 입력받기
x, k = map(int, input().split())

# 점화식에 따라 플로이드 워셜 알고리즘을 수행
for k in range(1, n + 1):
    for a in range(1, n + 1):
        for b in range(1, n + 1):
            graph[a][b] = min(graph[a][b], graph[a][k] + graph[k][b])

# 수행된 결과를 출력
distance = graph[1][k] + graph[k][x]

# 도달할 수 없는 경우, -1을 출력
if distance >= INF:
    print("-1")
# 도달할 수 있다면, 최단 거리를 출력
else:
    print(distance)

```


## ⚡ [문제 2] 전보

* **난이도: 🌕🌕🌕**
* **풀이시간: 60분**
* **시간 제한: 1초**
* **메모리 제한: 128MB**
* **기출: 유명 알고리즘 대회**

**[문제]**

어떤 나라에는 N개의 도시가 있다. 그리고 각 도시는 보내고자 하는 메세지가 있는 경우, 다른 도시로 전보를 보내서 다른 도시로 해당 메세지를 전송할 수 있다. 하지만 X라는 도시에서 Y라는 도시로 전보를 보내려면 도시 X -> Y로 가는 통로가 설치되어 있어야 한다. 예를 들어 X에서 Y로 향하는 통로는 있지만, Y에서 X로 향하는 통로가 없다면 Y는 X로 메세지를 보낼 수 없다. 또한 통로를 거쳐 메세지를 보낼 때는 일정 시간이 소요된다.
어느 날 C라는 도시 C에서 위급 상황이 발생해 최대한 많은 도시로 전보를 보내야 한다. 메세지는 도시 C에서 출발해 각 도시 사이에 설치된 통로를 거쳐 최대한 많이 퍼져나갈 것이다. 각 도시의 번호와 통로가 정보로 주어졌을 때, 도시 C에서 보낸 메세지를 받게 되는 도시의 개수는 총 몇 개 이며 도시들이 모두 메세지를 받는 데까지 걸리는 시간은 얼마인지 계산하는 프로그램을 작성해라.

**[입력 조건]**

* 첫째 줄에 도시의 개수 N, 통로의 개수 M, 메세지를 보내고자 하는 도시 C가 주어진다.(1<= N <= 30,000 , 1 <= M <= 200,000 , 1 <= C <= N)
* 둘째 줄부터 M+1 번째 줄에 걸쳐서 통로에 대한 정보 X, Y, Z가 주어진다. 이는 특정 도시 X에서 다른 특정 도시 Y로 이어지는 통로가 있으며, 메세지가 전달되는 시간이 Z라는 의미다.(1 <= X, Y <= N , 1 <= Z <= 1,000)

**[출력 조건]**

* 첫째 줄에 도시 C에서 보낸 메세지를 받는 도시의 총 개수와 총 걸리는 시간을 공백으로 구분해 출력한다.

```python  
# 입력 예시
3 2 1
1 2 4
1 3 2

# 출력 예시
2 4
```

**[문제 해설]**

* 한 도시에서 다른 도시까지의 최단 거리 문제로 전형적인 다익스트라 알고리즘 문제이다.
* N, M의 범위가 크기 때문에 우선순위 큐로 해야 한다.
* 출력 부분은 결국 거리 테이블 값 중 최단 시간 값이다.

```python
# 교재 풀이

import heapq
import sys
input = sys.stdin.readline
INF = int(1e9) # 무한을 의미하는 값으로 10억을 설정

# 노드의 개수, 간선의 개수를 입력받기
n, m, c = map(int, input().split())
# 각 노드에 연결되어 있는 노드에 대한 정보를 담는 리스트를 만들기
graph = [[] for i in range(n+1)]
# 최단 거리 테이블을 모두 무한으로 초기화
distance = [INF]*(n+1)

# 모든 간선 정보를 입력받기
for _ in range(m):
    x, y, z = map(int, input().split())
    graph[x].append((y,z))

def dijkstra(start):
    q = []
    # 시작 노드로 가기 위한 최단 경로는 0으로 설정하여, 큐에 삽입
    heapq.heappush(q, (0, start))
    distance[start] = 0
    while q: # 큐가 비어있지 않다면
        # 가장 최단 거리가 짧은 노드에 대한 정보 꺼내기
        dist, now = heapq.heappop(q)
        # 현재 노드가 이미 처리된 적이 있는 노드라면 무시
        if distance[now] < dist:
            continue
        # 현재 노드와 연결된 다른 인접한 노드들을 확인
        for i in graph[now]:
            cost = dist + i[1]
            # 현재 노드를 거쳐서, 다른 노드로 이동하는 거리가 더 짧은 경우
            if cost < distance[i[0]]:
                distance[i[0]] = cost
                heapq.heappush(q, (cost, i[0]))
# 다익스트라 알고리즘을 수행
dijkstra(c)

#도달할 수 있는 노드의 개수
count = 0
#도달할 수 있는 노드 중에서, 가장 멀리 있는 노드와의 최단거리
max_distance = 0
for d in distance :
  #도달할 수 있는 노드인 경우
  if d != INF:
    count += 1
    max_distance = max(max_distance,d)
#시작 노드는 제외 해야 하므로 count -1 출력
print(count-1,max_distance)

```

## **🍀** 회고
 실제 최단 경로를 모두 출력하는 문제보다는 단순한 최단 거리를 출력하도록 요구하는 문제가 많이 출제된다. 이번 알고리즘은 특히나 힙이라는 자료구조 알고리즘 때문에 이해하는 데에 있어 힘들었다. 코딩 테스트를 준비한다면 이것 또한 다른 알고리즘처럼 코드를 정확하고 제대로 구현할 수 있을 때까지 연습해야 한다. 나중에 알고리즘마다 문제를 집중적으로 풀다 보면 할 수 있을 거라 생각한다.
 