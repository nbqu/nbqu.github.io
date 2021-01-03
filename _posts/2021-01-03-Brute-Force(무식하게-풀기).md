---

layout: post
title: "알고리즘 문제해결 전략 : Brute Force(무식하게 풀기)"
author: "nbqu"
categories: algorithm
tags: [algorithm, BOJ]
image: develop.jpg

---
### 무식하게 풀기?
처음에는 '무식하게 풀기'에서 '무식하게' 라는 단어가 주는 인상 때문에 자칫하면 X밥같은 방법으로 여겼었다. <br>
![jotbab](./210103/jotbab.jpeg)
<br>사실 무식하게 풀기는, 경우의 수를 일일이 나열하면서 답을 찾기 때문에 무식하다 말하는 것이다. 하지만 경우의 수를 나열하는 방법은 전혀 무식하지 않다.<br>
우리에게 주어진 자원은 유한하지 않다. 브루트 포스가 아무리 빠른 계산능력을 바탕으로 수행하는 알고리즘이라지만 모든 경우의 수를 정말 '무식하게' 다 다루는 것은 비효율적이다. 그래서 어떻게 하면 경우의 수를 세어보되 효율적으로 셀 수 있는지 고민해볼 필요가 있다.<br><br>

### 경우의 수 줄이기
[14889 : 스타트와 링크](https://www.acmicpc.net/problem/14889)
짝수 명(N <= 20)의 전체 멤버를 둘로 나누어 축구 경기를 하는데, 두 팀간 능력치 차이를 최소화하여 팀을 짜는 방법을 구하는 문제이다.
> 첫 번째 풀이 (시간초과)
```python
def solve(team, n):
    ans = 9999
    if len(team) == n // 2:
        return compute(team, n)

    for i in range(n):
        if i not in team:
            ans = min(ans, solve(team + [i], n))

    return ans
```
이 방법으로 하면 최대 $$ _{20}P_{10} $$번 계산하게 된다. 순서가 상관없기 때문에 [0, 1, 2, 3]과 [2, 1, 0. 3]은 같은 경우이지만 다르게 계산해서 시간초과가 났다.<br>
> 두 번째 풀이 (시간초과)
```python
def solve(team, n):
    ans = 9999
    if len(team) == n // 2:
        return compute(team, n)

    for i in range(n):
        if i not in team:
            if not team or (team and i > team[-1]):
                ans = min(ans, solve(team + [i], n))

    return ans

print(solve([], n))
```
개선한 결과 최대 약 18만번 계산으로 줄일 수 있게 되었다. 그런데 여기에는 한 팀이 [0, 1, 2, 3]일 때와 [4, 5, 6, 7]을 다른 경우로 계산하는데 이 것 또한 중복으로 보아야 한다. 따라서 (start팀 = 0이 있는 팀), (link팀 = 0이 없는 팀)으로 지정해준 뒤 계산해야 두 번 계산 하는 것을 방지할 수 있다.

> 세 번째 풀이(AC, 7900ms)
```python
def solve(team, n):
    ans = 9999
    if len(team) == n // 2:
        return compute(team, n)

    for i in range(n):
        if i not in team:
            if not team or (team and i > team[-1]):
                ans = min(ans, solve(team + [i], n))

    return ans
print(solve([], n))
```
이제부터는 알고리즘적으로는 최대 약 9만 번 계산으로 최대한 단축했지만, 실행시간이 너무 많이 걸리는 것 같았다. 생각해보니
 ```python
if i not in team
```
와 같은 if statement O(n)으로 동작하고 있어서 풀필요한 잡계산을 만들었다. 그래서 이것을
```python
if not team[i]:  
    team[i] = True  
  ans = min(ans, solve(team, n, member+1, i))  
    team[i] = False
```
와 같이 O(1)로 동작하도록 수정하였고, 그 결과 실행시간을 두 배로 단축시킬 수 있었다(3024ms).<br>
### 결론
브루트 포스는 기본적으로 재귀함수를 이용하여 경우의 수를 탐색한다. 이 때 기저 사례를 잘 파악하고, 중복으로 세는 경우를 찾아내서 이를 제거해주어야 한다. 머릿 속으로 생각 했을 때는 어렵지 않지만, 코드로 구현하는 것은 또 다른 문제인 것 같다...


> Written with [StackEdit](https://stackedit.io/).
