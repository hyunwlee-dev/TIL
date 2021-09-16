---
layout: post
title: "TIL 백준 중급 알고리즘"
author: "hyunwlee"
---

## 1. BOJ 10282 해킹

> Category

백준, [해킹](https://www.acmicpc.net/problem/10282), 다익스트라 알고리즘, 골드5

<img src="https://github.com/hyunwlee-dev/TIL/blob/8db94c0013a4d98cbc0348e10ef6a0b494bfafb9/images/boj_hacking.png?raw=true" style="zoom:50%;"/>  



> 시간복잡도

다익스트라 시간복잡도: O(|M||log|N|)

> 풀이

다익스트라 문제이며, 주의할 점으로는 들어오는 input값 a, b에서 `a가 b를 의존하는 경우 b가 감염되면 a도 감염된다.` 라는 글을 보고 인접리스트에 from과 to를 바꿔서 넣어줘야 한다.

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class 해킹 {
    static class Node implements Comparable<Node>
    {
        int end;
        int weight;
        public Node(int end, int weight)
        {
            this.end = end;
            this.weight = weight;
        }
        @Override
        public int compareTo(Node n) {
            return (this.weight - n.weight);
        }
    }
    static List<Node>[] graph;
    static int[] dist;
    static boolean[] check;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int num = Integer.parseInt(br.readLine());
        if (num == 0)
            return;
        for (int i = 0; i < num; i++)
        {
            StringTokenizer st = new StringTokenizer(br.readLine());
            int n = Integer.parseInt(st.nextToken());
            int d = Integer.parseInt(st.nextToken());
            int start = Integer.parseInt(st.nextToken());
            graph = new ArrayList[n + 1];
            for (int j = 0; j < n + 1; j++)
                graph[j] = new ArrayList<>();
            dist = new int[n + 1];
            Arrays.fill(dist, Integer.MAX_VALUE);
            check = new boolean[n + 1];
            for (int j = 0; j < d; j++)
            {
                st = new StringTokenizer(br.readLine());
                int a = Integer.parseInt(st.nextToken());
                int b = Integer.parseInt(st.nextToken());
                int s = Integer.parseInt(st.nextToken());
                graph[b].add(new Node(a, s));
            }
            int[] answer = dijkstra(start);
            System.out.println(answer[0] + " " + answer[1]);
        }
    }
    public static int[] dijkstra(int start)
    {
        int cntInfectedNode = 0;  // check를 총 몇번하는지 카운트
        int lastNode = start;			// 방문한 노드 최신화
        PriorityQueue<Node> pq = new PriorityQueue<>();
        pq.offer(new Node(start, 0));
        dist[start] = 0;
        while (!pq.isEmpty())
        {
           Node node = pq.poll();
           int current = node.end;
           if (check[current])
               continue;
           check[current] = true;
           ++cntInfectedNode;
           lastNode = current;
           for (Node n : graph[current])
           {
               if (!check[n.end] && dist[n.end] > dist[current] + n.weight)
               {
                   dist[n.end] = dist[current] + n.weight;
                   pq.offer(new Node(n.end, dist[n.end]));
               }
           }
        }
        return (new int[] {cntInfectedNode, dist[lastNode]});
    }
}
```