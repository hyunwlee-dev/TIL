---
layout: post
title: "TIL 백준 중급 알고리즘, 42seoul(push_swap)"
author: "hyunwlee"
---

## <span style="background-color:#1D6A96; color:white">BOJ 1202 보석 도둑</span>

<span style="background-color:#85B8CB; color:white"><strong> Category</strong></span>

백준, [보석 도둑](https://www.acmicpc.net/problem/1202), 그리디 알고리즘, 골드2

<img src="https://github.com/hyunwlee-dev/TIL/blob/7791c62ac22e06a86e50570c7b8af8890c09419a/images/jewelryThief.png?raw=true" style="zoom:50%;" />  



<span style="background-color:#85B8CB; color:white"><strong>시간복잡도</strong></span>

그리디: 정렬 O(NlogN)



<span style="background-color:#85B8CB; color:white"><strong>풀이</strong></span>

N과 K는 최대 30만으로 들어올 수 있기 때문에 O(N^2)은 시간초과가 나올게 분명하여 BruteForce는 불가능할 것이라 생각되었다.

이번 문제는 Greedy와 Priority Queue를 사용하여 푸는 문제였다. 

1. 보석을 무게를 기준으로 오름차순으로 정렬한다.
2. 가방도 오름차순으로 정렬 해준다.
3. 가방을 허용 무게 기준으로 보다 같거나 작은것들을 우선순위 큐에 집어넣는다. (pq는 가격에 대해 내림차순으로)
4. pq가 비어있지 않으면, 가방마다 pq의 root를 더해 나간다.



```
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
  - [ ] 1500 미만: 1점

6. <span style="background-color:#FFC2C3; color:white"><strong>Advance version</strong></span>

- [ ] ARG="<span style="color:blue">500 random</span>"; ./push_swap $ARG | ./checker_Mac $ARG 

  <span style="color:red">result line: </span>

  - [ ] 5500 미만: 5점
  - [ ] 7000 미만: 4점
  - [ ] 8500 미만: 3점
  - [ ] 10000 미만: 2점
  - [ ] 11500 미만: 1점

---

