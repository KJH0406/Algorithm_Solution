# [그리디] 주유소 - 14719(Silver III)

### [문제 링크](https://www.acmicpc.net/problem/13305)

<br>

### ✔️ 분류

그리디

<br>

### ✅ 리뷰

기본 **그리디** 문제이다.

구현상의 리뷰 보다는 알고리즘 형식을 파악하는데 도움이 되는 포인트를 하나 리뷰하고자 한다.

<mark>1. **문제를 잘 읽고 관련 알고리즘 힌트를 파악하자!** </mark>

문제에서 **'가장 큰 순서대로', '가장 작은 순서대로', ‘최소의 비용’, ‘최대 이익’** 등과 같은 기준을 알게 모르게 제시할때가 있다.<br>
그리디 알고리즘의 가장 큰 특징이 **현재 상황에서 가장 좋은 상황을 선택하는 것**이다. <br>

이 문제의 지문에서도 <br>

> 각 도시에 있는 주유소의 기름 가격과, 각 도시를 연결하는 도로의 길이를 입력으로 받아 제일 왼쪽 도시에서 제일 오른쪽 도시로 > 이동하는 **최소의 비용**을 계산하는 프로그램을 작성하시오. <br>

**최소의 비용**이라는 힌트가 존재하여 문제 유형이 **그리디**임을 보다 빠르게 파악할 수 있었다.

<br>

### ✔️ 풀이 코드

```python
n = int(input()). # 도시의 수
roads = list(map(int, input().split()))  # 도로의 수
cities = list(map(int, input().split()))  # 도시 기름 값 배열

min_price = cities[0]  # 가장 싼 기름 값
res = 0

for i in range(len(roads)):
    road = roads[i]
    city = cities[i + 1]
    res += min_price * road
		# 만약 더 싼 가격의 기름 값이 존재한다면 교체
    if min_price > city:
        min_price = city
print(res)
```
