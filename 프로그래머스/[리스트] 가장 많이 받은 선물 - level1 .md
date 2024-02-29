# [유형] 가장 많이 받은 선물 - 258712(level.1)

### [문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/258712)

<br>

### ✔️ 분류

리스트

<br>

### ✅ 리뷰

리스트를 제어하여 구현하는 기초적인 문제였다. 2차원 배열, 인덱스 매핑 등 기초 배열 자료구조를 다루기에 좋은 문제인 것 같다.

### 1. **`enumerate` 함수를 이용하여 인덱스 매핑**

그동안 문제에서 인덱스 매핑이 필요할 때면 그냥 for문을 통해서 key와 value값을 추가했었는데,<br> enumerate 함수를 통하여 보다 간편하게 할 수 있음을 알게되었다.

아래 예시를 통해 보다 쉽게 그 기능을 확인해볼 수 있다.

```python
friends = ['Alice', 'Bob', 'Charlie']

for index, name in enumerate(friends):
    print(index, name)

# 결과
# 0 Alice
# 1 Bob
# 2 Charlie
```

이 예시에서 enumerate(friends)는 0부터 시작하는 인덱스와 friends 리스트의 각 요소를 튜플 형태로 반환한다.<br>
for 루프는 이 튜플을 index, name 변수에 언패킹하여 각각의 값을 사용할 수 있게 한다.

### 2. 변수명의 중요성 강조

처음 문제를 푼 코드를 보면 변수명이 참으로 어지럽게 설정했었다.<br>
이후 리팩토링 과정을 통해 적절한 변수명을 찾고 통일성을 갖추려고 노력했다.
이후 이 코드 리뷰 포스트를 위해 다시 코드를 읽으니 가독성이 정말 차이가 많이 난다는 것을 알게되었다.<br>
변수명의 가장 앞에 핵심적인 용어들 즉, **friends**\_dict, **gift**\_exchange, **next**\_month_gifts 등을 설정하고 <br>
뒤에 세부 설명을 통해 해당 변수가 뜻하는 바를 정확하게 캐치 할 수 있었다.
<br>

### ✔️ 풀이 코드

```python
def solution(friends, gifts):
    answer = 0

    n = len(friends)  # 친구의 수

    # 친구 목록을 인덱스로 매핑
    friends_dict = {friend: idx for idx, friend in enumerate(friends)}

    # 주고받은 선물 기록
    gift_exchange = [[0] * n for _ in range(n)]

    # 각 친구별로 준 선물 수, 받은 선물 수, 선물 지수를 관리
    gift_stats = [[0, 0] for _ in range(n)]  # [준 선물 수, 받은 선물 수]

    # 선물 기록을 바탕으로 데이터를 업데이트
    for gift in gifts:
        giver, receiver = gift.split(' ')
				# 이름을 통해 인덱스 추출
        giver_idx, receiver_idx = friends_dict[giver], friends_dict[receiver]

        gift_exchange[giver_idx][receiver_idx] += 1
        gift_stats[giver_idx][0] += 1  # 준 선물 수 증가
        gift_stats[receiver_idx][1] += 1  # 받은 선물 수 증가

    # 다음 달에 받을 선물 수를 계산
    next_month_gifts = [0 for _ in range(n)]

    for i in range(n):
        for j in range(i + 1, n):
            # 선물 지수 계산
            gift_index_i = gift_stats[i][0] - gift_stats[i][1]
            gift_index_j = gift_stats[j][0] - gift_stats[j][1]

            # 선물 교환 기록과 선물 지수에 따라 결정
            if gift_exchange[i][j] > gift_exchange[j][i]:
                next_month_gifts[i] += 1
            elif gift_exchange[i][j] < gift_exchange[j][i]:
                next_month_gifts[j] += 1

            else:  # 선물을 주고받은 기록이 같거나 없는 경우
                if gift_index_i > gift_index_j:
                    next_month_gifts[i] += 1
                elif gift_index_i < gift_index_j:
                    next_month_gifts[j] += 1

    # 가장 많은 선물을 받을 친구가 받을 선물의 수를 반환
    answer = max(next_month_gifts)

    return answer
```
