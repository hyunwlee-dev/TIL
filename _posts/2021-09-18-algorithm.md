---
layout: post
title: "TIL 백준 중급 알고리즘, 42seoul(push_swap)"
author: "hyunwlee"
---

## 1. <span style="background-color:lightgrey">BOJ 1202 보석 도둑</span>

> Category

백준, [보석 도둑](https://www.acmicpc.net/problem/1202), 그리디 알고리즘, 골드2

<img src="https://github.com/hyunwlee-dev/TIL/blob/7791c62ac22e06a86e50570c7b8af8890c09419a/images/jewelryThief.png?raw=true" style="zoom:50%;" />  



> 시간복잡도

그리디: 정렬 O(NlogN)

> 풀이

N과 K는 최대 30만으로 들어올 수 있기 때문에 O(N^2)은 시간초과가 나올게 분명하여 BruteForce는 불가능할 것이라 생각되었다.

이번 문제는 Greedy와 Priority Queue를 사용하여 푸는 문제였다. 

1. 보석을 무게를 기준으로 오름차순으로 정렬한다.
2. 가방도 오름차순으로 정렬 해준다.
3. 가방을 허용 무게 기준으로 보다 같거나 작은것들을 우선순위 큐에 집어넣는다. (pq는 가격에 대해 내림차순으로)
4. pq가 비어있지 않으면, 가방마다 pq의 root를 더해 나간다.



```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class 보석 도둑
{
    static class Jewelry implements Comparable<Jewelry>
    {
        int weight;
        int price;
        public Jewelry(int weight, int price)
        {
            this.weight = weight;
            this.price = price;
        }
        @Override
        public int compareTo(Jewelry j)
        {
            return (this.weight - j.weight);
        }
    }
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int k = Integer.parseInt(st.nextToken());
        Jewelry jewelries[] = new Jewelry[n];
        int[] bags = new int[k];
        for (int i = 0; i < n; i++)
        {
            st = new StringTokenizer(br.readLine());
            jewelries[i] = new Jewelry(Integer.parseInt(st.nextToken()), Integer.parseInt(st.nextToken()));
        }
        for (int i = 0; i < k; i++)
            bags[i] = Integer.parseInt(br.readLine());
        Arrays.sort(jewelries);
        Arrays.sort(bags);
        PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());
        int j = 0;
        long answer = 0;
        for (int i = 0; i < k; i++)
        {
            while (j < n && jewelries[j].weight <= bags[i])
                pq.offer(jewelries[j++].price);
            if (!pq.isEmpty())
                answer += pq.poll();
        }
        System.out.println(answer);
    }
}

```
## 2. <span style="background-color:lightgrey">42seoul(push_swap)</span>

#### 동료 학습 방법: 개인

#### 학습 목표: 예외처리

1. ~~<strong>Error management</strong>~~

- [x] list of integers 중 한 원소가 integer가 아닐 경우 <span style="color:red">result: Error</span>
- [x] list of integers의 값이 Integer보다 클 경우 <span style="color:red">result: Error</span>
- [x] duplicates가 존재할 경우 <span style="color:red">result: Error</span>
- [x] stdin에서 받은 명령어가 존재하지 않는 명령어일 경우 <span style="color:red">nothing</span>

- [x] <span style="background:lightpink">정렬된 상태이면 종료</span>

2. ~~<strong>Identity test</strong>~~

- [x] ./push_swap <span style="color:blue">42</span> <span style="color:red">result:</span>
- [x] ./push_swap <span style="color:blue">0 1 2 3</span> <span style="color:red">result: </span>
- [x] ./push_swap <span style="color:blue">0 1 2 3 4 5 6 7 8 9</span> <span style="color:red">result: </span>

3. ~~<strong>Simple version</strong>~~

- [x] ARG="<span style="color:blue">2 1 0</span>"; ./push_swap $ARG | ./checker_Mac $ARG <span style="color:red">result line: 2~3</span>

4. <strong>Another simple version</strong>

- [ ] ARG="<span style="color:blue">1 5 2 4 3</span>"; ./push_swap $ARG | ./checker_Mac $ARG <span style="color:red">result line: ~12</span>
- [ ] ARG="<span style="color:blue">5 random</span>"; ./push_swap $ARG | ./checker_Mac $ARG <span style="color:red">result line: ~12</span> 
  - // 검증하기 전에 이 테스트를 여러 순열로 두 번 반복해야 합니다.

5. <strong>Middle version</strong>

- [ ] ARG="<span style="color:blue">100 random</span>"; ./push_swap $ARG | ./checker_Mac $ARG 

  <span style="color:red">result line: </span>

  - [ ] 700 미만: 5점

  - [ ] 900 미만: 4점
  - [ ] 1100 미만: 3점
  - [ ] 1300 미만: 2점
  - [ ] 1500 미만: 1점

6. <strong>Advannced version</strong>

- [ ] ARG="<span style="color:blue">500 random</span>"; ./push_swap $ARG | ./checker_Mac $ARG 

  <span style="color:red">result line: </span>

  - [ ] 5500 미만: 5점
  - [ ] 7000 미만: 4점
  - [ ] 8500 미만: 3점
  - [ ] 10000 미만: 2점
  - [ ] 11500 미만: 1점

---

