---
layout: post
title: "TIL 백준 중급 알고리즘, 42seoul(push_swap)"
author: "hyunwlee"
---

## 1. BOJ 1339 단어 수학

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
3. 가방을 허용무게 기준으로 보다 같거나 작은것들을 우선순위 큐에 집어넣는다. (pq는 가격에 대해 내림차순으로)
4. pq가 비어있지 않으면, 가방마다 pq의 root를 더해나간다.



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
## 2. 42seoul(push_swap)

