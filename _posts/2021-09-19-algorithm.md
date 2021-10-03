---
layout: post
title: "TIL 백준 중급 알고리즘, 42seoul(push_swap)"
author: "hyunwlee"
---

## <span style="background-color:#1D6A96; color:white">BOJ 1043 거짓말</span>

<span style="background-color:#85B8CB; color:white"><strong> Category</strong></span>

백준, [거짓말](https://www.acmicpc.net/problem/1043), 그래프, 골드4

<img src="https://github.com/hyunwlee-dev/TIL/blob/7a769999c3840f51a7cc4b3c91f0a280b4382f04/images/lie.png?raw=true" style="zoom:50%;" />  



<span style="background-color:#85B8CB; color:white"><strong>시간복잡도</strong></span>

처음풀이: 인접리스트를 구하는 과정에서 O(N ^ 2) + BFS에서 (O(|V|+|E|)) = O(N ^ 2)



<span style="background-color:#85B8CB; color:white"><strong>풀이</strong></span>

<strong>처음풀이</strong>는 Union-find라는 개념을 모른채 그래프 문제이므로 탐색을 돌려 분야별로 영역 표시를 해주었다. (boj 섬의 개수 인강을 들었을때 처럼)

<script src="https://gist.github.com/hyunwlee-dev/6e6914dc3ac4b19cc222de4910ccd307.js"></script>



<strong>나중풀이</strong>는 시간 복잡도는 비슷하지만 여러 자료구조가 필요없는 Union-find를 공부하며 다시 풀어냈다.

line(47 ~ 49)에서 이해 하는데 꽤나 걸렸다. 디버깅도 돌려보고 출력도 여러번 찍어보았다.

<strong>[TC]</strong>

```
6 5
1 6
2 4 5
2 1 2
2 2 3
2 3 4
2 5 6
// result : 0
```

<strong>union(int first, int second) 함수</strong>를 통해 parent[Math.<strong>max</strong>(first, second)] = Math.<strong>min</strong>(first, second)  특징을 깊이 생각해보길 바란다.

<script src="https://gist.github.com/hyunwlee-dev/d6adcd3e11d7b2d02961973d513f85dc.js"></script>

아직 Union-Find가 어색하여 같은 유형의 많은 문제를 풀어보는 것이 좋을 듯 하다.

---

## <span style="background-color:#E81E25; color:white">42seoul(push_swap)</span>

#### <span style="background-color:#FE7773; color:white">동료 학습 방법</span>

개인

#### <span style="background-color:#FE7773; color:white">학습 목표</span>

예외처리

1. ~~<span style="background-color:#FFC2C3; color:white"><strong>Error management</strong></span>~~

- [x] list of integers 중 한 원소가 integer가 아닐 경우 <span style="color:red">result: Error</span>
- [x] list of integers의 값이 Integer보다 클 경우 <span style="color:red">result: Error</span>
- [x] duplicates가 존재할 경우 <span style="color:red">result: Error</span>
- [x] stdin에서 받은 명령어가 존재하지 않는 명령어일 경우 <span style="color:red">nothing</span>

- [x] <span style="background:pink">정렬된 상태이면 종료</span>

2. ~~<span style="background-color:#FFC2C3; color:white"><strong>Identity test</strong></span>~~

- [x] ./push_swap <span style="color:blue">42</span> <span style="color:red">result:</span>
- [x] ./push_swap <span style="color:blue">0 1 2 3</span> <span style="color:red">result: </span>
- [x] ./push_swap <span style="color:blue">0 1 2 3 4 5 6 7 8 9</span> <span style="color:red">result: </span>

3. ~~<span style="background-color:#FFC2C3; color:white"><strong>Simple Version</strong></span>~~

- [x] ARG="<span style="color:blue">2 1 0</span>"; ./push_swap $ARG | ./checker_Mac $ARG <span style="color:red">result line: 2~3</span>

4. <strong><span style="background-color:#FFC2C3; color:white"><strong>Another  simple version</strong></span></strong>

- [ ] ARG="<span style="color:blue">1 5 2 4 3</span>"; ./push_swap $ARG | ./checker_Mac $ARG <span style="color:red">result line: ~12</span>
- [ ] ARG="<span style="color:blue">5 random</span>"; ./push_swap $ARG | ./checker_Mac $ARG <span style="color:red">result line: ~12</span> 
  - // 검증하기 전에 이 테스트를 여러 순열로 두 번 반복해야 합니다.

5. <span style="background-color:#FFC2C3; color:white"><strong>Middle version</strong></span>

- [ ] ARG="<span style="color:blue">100 random</span>"; ./push_swap $ARG | ./checker_Mac $ARG 

  <span style="color:red">result line: </span>

  - [ ] 700 미만: 5점

  - [ ] 900 미만: 4점
  - [ ] 1100 미만: 3점
  - [ ] 1300 미만: 2점
  - [x] 1500 미만: 1점

6. ~~<span style="background-color:#FFC2C3; color:white"><strong>Advance version</strong></span>~~

- [ ] ARG="<span style="color:blue">500 random</span>"; ./push_swap $ARG | ./checker_Mac $ARG 

  <span style="color:red">result line: </span>

  - [ ] 5500 미만: 5점
  - [ ] 7000 미만: 4점
  - [ ] 8500 미만: 3점
  - [x] 10000 미만: 2점
  - [ ] 11500 미만: 1점

#### <span style="background-color:#FE7773; color:white">최적화 5개 짜리 요소가 들어오는 경우</span>

- 3개 짜리로 만드는 것이 목표입니다.
- <strong>for (a 노드 개수) if (a 노드 값 == max || a 노드 값 == min) pa() else ra() 3개짜리 정렬() b stack에서 a stack으로 다시 넘겨주기</strong>
- => b stack으로 가장 큰수와 가장 작은 수 2개를 넘겨주어서 3개 정렬 후 2개를 다시 넘겨줍니다.



java로 TC를 구하기 위해서 BackTracking을 이용하여 순열, 그리고 랜덤으로 순열 하나를 출력하는 프로그램

<script src="https://gist.github.com/hyunwlee-dev/fdd9d25890fe0bdb8189c32c30c3bc21.js"></script>

- checker_Mac 쉘 책점 프로그램 만들기 추천