# [구현, 덱, 큐] 뱀 - 3190(Gold IV)

### [문제 링크]()

<br>

### ✔️ 분류

구현, 시뮬레이션, 덱, 큐

<br>

### ✅ 리뷰

구현을 바탕으로, 큐를 활용하는 문제였다. <br>

다른 사람들의 풀이를 보았을 때 큐를 사용해서 풀은 사람이 많았고, <br>
리스트보다 큐를 사용하는 것이 시간 복잡도 측면에서 유리하다는 글도 보았다. <br>

이에 pop(0)와 popleft()의 수행 속도 차이를 검색해본 결과 아래와 같은 정보를 파악할 수 있었다. <br>

**1. list.pop(0), deque.popleft() 무엇이 다를까?**

리스트의 마지막 원소 삭제의 시간복잡도는 O(1)이지만,
**특정 인덱스의 원소를 삭제하기 위해서는 그 원소 뒤의 모든 원소들을 한 칸씩 옮겨야하기 때문에 시간복잡도는 O(n)**이다. <br>

덱의 **popleft()는 front += 1 의 형태의 연산만을 수행하기 때문에 O(1)의 빠른 복잡도**로 원소 삭제가 가능하다.
<br>

<hr>

근데 사실 내 코드에서는 큐를 사용하는 것과 리스트에서 pop(0)를 사용하는 것과 실행시간이 크게 차이 나지 않았다. <br>
오히려 큐를 사용하여 popleft( )하는 것이 20ms정도 시간이 더 오래 걸렸다.<br>
다른 해설을 보아도 해당 문제의 테스트 케이스 정도의 크기에서 왜 큐를 사용했을 때 더 효율적인지를 고려한 풀이를 보지 못했다. <br>

**이 문제에 대해서 현재 탐구중이며,dfs나 bfs 자료구조를 보다 심층적으로 학습한 후 다시 해당 문제를 리뷰해봐야겠다.**

<br>

### ✔️ 풀이 코드

```python
N = int(input())  # 보드의 크기
board = [[3] * (N + 2)] + [[3] + [0] * N + [3] for _ in range(N)] + [[3] * (N + 2)]  # 보드 생성

K = int(input())  # 사과의 개수
for _ in range(K):
    i, j = map(int, input().split())
    board[i][j] = 1

L = int(input())  # 방향 전환 횟수
direction_info = {}  # 방향 전환 정보
rotation_time = []  # 방향 전환 하는 시간
for _ in range(L):
    X, C = input().split()
    direction_info[int(X)] = C
    rotation_time.append(int(X))

time = 0  # 시간
si, sj = 1, 1  # 뱀의 현 위치
board[si][sj] = 2  # 보드에서 뱀 표시
rotation = [(-1, 0), (0, 1), (1, 0), (0, -1)]  # 북, 동, 남, 서
d = 1  # 처음 에는 오른 쪽을 향해 시작(동쪽[1])

snake_length = [(si, sj)]


while True:
    time += 1
    di, dj = rotation[d]  # 이동 방향에 따른 좌표 설정
    ci, cj = snake_length[-1]
    ni, nj = ci + di, cj + dj  # 다음 이동 좌표

    # 이동 가능한 좌표일 시
    if N >= ni > 0 and N >= nj > 0 and board[ni][nj] < 2:
        # 해당 위치가 빈칸일 때
        if board[ni][nj] == 0:
            # 꼬리 좌표 삭제
            pi, pj = snake_length.pop(0)
            board[pi][pj] = 0  # 빈 칸 표시
        board[ni][nj] = 2  # 뱀 이동
        snake_length.append((ni, nj))

    # 이동 불가능 시 게임 종료
    else:
        break

    # 이동이 끝난 후 회전 해야 하는 시간이라면 회전 시킴.
    if time in rotation_time:
        if direction_info[time] == 'D':
            d = (d + 1) % 4
        else:
            d = (d + 3) % 4

print(time)

```
