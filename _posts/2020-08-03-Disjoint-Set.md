---
layout: post
title: "Disjoint Set"
author: "nbqu"
categories: algorithm
tags: [disjoint_set]
image: develop.jpg
---

## Disjoint Set
&nbsp;&nbsp;&nbsp;&nbsp;**서로소 집합**이라고도 한다. 처음으로 이 자료구조를 접할 때는 알고리즘 시간에 Kruskal Algorithm을 다룰 때였다. 간단하게 Kruskal Algorithm을 설명하자면, edge들을 최소 비용 순으로 정렬한 뒤 모든 node들이 cycle 없이 연결 될 때 까지 선택하는 것이다. 

&nbsp;&nbsp;&nbsp;&nbsp;자연어 처리에서 적용을 한다면, 동의어 집합을 구성할 때 Disjoint Set을 활용하면 좋을 것 같다는 생각이 든다. 키워드를 root node로 설정해서, 키워드와 동의어(혹은 의미가 유사한)들을 이어나가는 것이다.
## 기본 연산

 - **``MakeSet(X)``** X로 구성된 새로운 집합을 리턴한다.
 - **``Union(S, T)``** S ∪ T를 리턴한다.
 - **``Find(X)``** X ∈ S를 만족하는 집합 S(즉, root node)를 리턴한다.

&nbsp;&nbsp;&nbsp;&nbsp;``Union(S, T)``연산을 할 때에는, 한 집합의 root node를 다른 집합의 root node에 포인터를 연결해야한다. 코퍼스가 작을 때에는 아무래도 상관 없겠지만, 그렇지 않을 때에는 집합을 합칠 때에 신경써야 할 부분이 있다.

&nbsp;&nbsp;&nbsp;&nbsp;두 개의 집합을 합칠 때에, 포인터를 어떤 방향으로 연결하느냐에 따라 ``Find(X)``의 효율성이 달라진다고 볼 수 있다. 이를테면 두 개의 집합(트리)가 있을 때, 크기가 큰 집합을 크기가 작은 집합에 붙이면 반대로 붙였을 때 보다 합집합의 높이가 커질 수 있다. 이렇게 되면 집합의 root node를 찾는 시간이 증가하게 될 것이다.

![img1](https://user-images.githubusercontent.com/31720981/89159852-0fd59180-d5ab-11ea-92d6-156f1fa05b3f.jpeg)

> (5)를 (1)에 붙이면 높이가 2가 되지만, (1)을 (5)에 붙이면 높이가 3이 되어 (4)의 root node를 찾는데 시간이 더 소요된다.

![img2](https://user-images.githubusercontent.com/31720981/89159861-12d08200-d5ab-11ea-8eba-d8a7bef65fa6.png)
그래서 효율적인 Union 함수는 위와 같이 쓸 수 있다.

## Path Compression
&nbsp;&nbsp;&nbsp;&nbsp;부모관계가 복잡해지면 그에 따라서 트리의 구성이 비효율적이게 변할 수 있다. 이 때 ``Find(X)`` 함수를 실행시키면서 경로를 압축할 수 있다. root node를 찾으면서, 하위의 node를 root로 바로 연결시키는 것이다.

![img3](https://user-images.githubusercontent.com/31720981/89159863-1401af00-d5ab-11ea-8d10-859d3df4555e.png)
> 위는 ``Find(9)``를 실행시킨 모습이다. (9)에서부터 부모에 있는 노드들(6, 8)을 root와 바로 연결시켰다. 이로써 트리의 높이는 4에서 2로 줄어들었다!

![img4](https://user-images.githubusercontent.com/31720981/89159869-149a4580-d5ab-11ea-888e-03313db90528.png)

### Reference
- *"Data Structrues & Their Algorithms", Harry R. Lewis, Larry Denenberg, 1991*
- [*[자료구조]Union-Find: Disjoint Set의 표현*](https://bowbowbow.tistory.com/26)
- [Disjoint Set](https://ratsgo.github.io/data%20structure&algorithm/2017/11/12/disjointset/)