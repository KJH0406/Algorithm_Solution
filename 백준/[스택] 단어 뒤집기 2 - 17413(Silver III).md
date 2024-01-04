# [Silver III] 단어 뒤집기 2 - 17413

### [문제 링크](https://www.acmicpc.net/problem/17413)

<br>

### ✔️ 분류

자료 구조, 구현, 스택, 문자열

<br>

### ✅ 리뷰

각 조건에 따라 문자열을 조작하는 문제였다. <br>
기초적인 **스택**의 개념을 담고있는 것 같다. <br>
구현에 있어서 큰 어려움은 없었지만 **리스트 안 원소를 다루는데 있어 배운점**이 한 가지 있다. <br>
S라는 리스트안에서 각 원소를 추출하여 규칙에 따라 처리를 하고자 하였는데, 무심코 추출한다는 것에 포커싱이 되어 pop을 사용하여 처리를 하였다. <br>
하지만 각 원소는 조건에 따라 처리하여 ans변수에 더하여 결과 값을 출력하면 될 뿐 될 뿐 굳이 **기존의 배열(S)을 변형시킬 이유가 전혀없는 문제**였다. <br>

**1. 인덱스를 사용하면 리스트가 변경되더라도 인덱스를 재조정할 필요가 없다.** <br>
pop은 요소를 제거한 후에 리스트를 조정해야 한다. <br>
하지만 인덱스를 사용하면 리스트가 변경되더라도 인덱스를 재조정할 필요가 없기 때문에 이번 문제에서는 리스트의 각 원소를 처리하는데 있어 <br>
index를 사용하여 각 자리에 위치한 원소를 조건에 따라 처리하는 것이 조금 더 효율적이었을 것 같다는 생각이 들었다.

```python
for i in range(len(S)):
    item = S[i]
```

<br>

### ✔️ 풀이 코드

```python
S = list(input())
flag = 0
ans = ''
stack = []

while S:
    item = S.pop(0)  # 문자 추출
    if item == '<':  # 괄호 안은 뒤집지 않음.
        if stack:
            while stack:
                ans += stack.pop()
        ans += item  # 여는 괄호 추가
        flag = 1  # 괄호 열음 표시
        while True:
            sub_item = S.pop(0)  # 괄호 열은 후 닫을 때 까지 문자 추출
            if sub_item == '>':  # 닫는 괄호일 때
                ans += sub_item  # 닫는 괄호 추가
                flag = 0  # 괄호 닫음 표시
                break  # 문자 추출 종료
            else:
                ans += sub_item  # 괄호 닫을 때 까지 문자 추출
    # 공백일 때
    elif item == ' ' and stack:
        # 스택에 있는 단어 결과에 더함
        while stack:
            ans += stack.pop()
        ans += ' '
    else:
        stack.append(item)

if stack:
    while stack:
        ans += stack.pop()

print(ans)
```
