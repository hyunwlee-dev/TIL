---
layout: post
title: "TIL 백준 중급 알고리즘, 42seoul(push_swap)"
author: "hyunwlee"
---

## 1. <span style="background-color:lightgrey">BOJ 1043 거짓말</span>

> Category

백준, [거짓말](https://www.acmicpc.net/problem/1043), 그래프, 골드4

<img src="https://github.com/hyunwlee-dev/TIL/blob/7a769999c3840f51a7cc4b3c91f0a280b4382f04/images/lie.png?raw=true" style="zoom:50%;" />  



> 시간복잡도

처음풀이: 인접리스트를 구하는 과정에서 O(N ^ 2) + BFS에서 (O(|V|+|E|)) = O(N ^ 2)?


> 풀이

`처음풀이`는 Union-find라는 개념을 모른채 그래프 문제이므로 탐색을 돌려 분야별로 영역 표시를 해주었다. (boj 섬의 개수 인강을 들었을때 처럼)


```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class 거짓말 {
    static int[] dist;
    static boolean[] check;
    static List<Integer>[] list; // 인접 리스트
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());
        check = new boolean[n + 1];
        dist = new int[n + 1];
        list = new ArrayList[n + 1];
        List<Integer>[] partyList = new ArrayList[m];
        for (int i = 0; i < m; i++)
            partyList[i] = new ArrayList<>();
        for (int i = 0; i < n + 1; i++)
            list[i] = new ArrayList<>();
        List<Integer> liarList = new ArrayList<>();
        st = new StringTokenizer(br.readLine());
        int liarNum = Integer.parseInt(st.nextToken());
        for (int i = 0; i < liarNum; i++)
            liarList.add(Integer.parseInt(st.nextToken()));
        for (int i = 0; i < m; i++)
        {
            st = new StringTokenizer(br.readLine());
            int participant = Integer.parseInt(st.nextToken());
            for (int j = 0; j < participant; j++)
                partyList[i].add(Integer.parseInt(st.nextToken()));
        }
        insertElementsToAdjacentList(partyList);
        int marking = 0;
        for (int liar : liarList)
        {
            if (dist[liar] == 0)
                BFS(liar, ++marking);
        }

        int answer = 0;
        for (List<Integer> party : partyList)
        {
            boolean canLie = true;
            for (int p : party)
                if (dist[p] != 0)
                {
                    canLie = false;
                    break;
                }
            if (canLie)
                ++answer;
        }
        System.out.println(answer);
    }
    public static void BFS(int start, int marking)
    {
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(start);
        dist[start] = marking;
        while (!queue.isEmpty())
        {
            int temp = queue.poll();
            for (int i : list[temp])
            {
                if (dist[i] != 0)
                    continue;
                queue.offer(i);
                ++dist[i];
            }
        }
    }

    public static void insertElementsToAdjacentList(List<Integer>[] partyList)
    {
        for (int i = 0; i < partyList.length; i++)
            for (int j = 0; j < partyList[i].size(); j++)
                for (int l = 0; l < partyList[i].size(); l++)
                    if (j != l)
                        list[partyList[i].get(j)].add(partyList[i].get(l));
    }
}

```

`나중풀이`는 시간 복잡도는 비슷하지만 여러 자료구조가 필요없는 Union-find를 공부하며 다시 풀어냈다.

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;
import java.util.StringTokenizer;

public class 거짓말
{
    static int[] parent;
    static boolean[] knowPeople = new boolean[51];
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());

        // Union-Find 초기화
        parent = new int[n + 1];
        for (int i = 1; i < n + 1; i++)
            parent[i] = i;

        // 진실을 아는 사람 knowPeople
        st = new StringTokenizer(br.readLine());
        int knowPeopleNum = Integer.parseInt(st.nextToken());
        for (int i = 0; i < knowPeopleNum; i++)
            knowPeople[Integer.parseInt(st.nextToken())] = true;

        // 파티 정보를 읽으면서 같은 파티에 있는 사람 Union
        List<Integer>[] people = new ArrayList[m];
        for (int i = 0; i < m; i++)
            people[i] = new ArrayList<>();
        int first = 0;
        int second;
        for (int i = 0; i < m; i++)
        {
            st = new StringTokenizer(br.readLine());
            int peopleNum = Integer.parseInt(st.nextToken());
            if (peopleNum > 0)
            {
                first = Integer.parseInt(st.nextToken());
                people[i].add(first);
            }
            for (int j = 1; j < peopleNum; j++)
            {
                second = Integer.parseInt(st.nextToken());
                people[i].add(second);
                union(first, second);
                first = second;
            }
        }

        // 진실을 아는 사람들의 parent 는 같이 파티를 참여 했으므로 진실을 아는 사람들
        for (int i = 1; i < knowPeople.length; i++)
            if (knowPeople[i])
                knowPeople[find(i)] = true;
        int answer = 0;
        int parent;
        for (int i = 0; i < m; i++) {
            if (people[i].size() > 0) {
                parent = find(people[i].get(0));
                if (!knowPeople[parent])
                    ++answer;
            }
        }
        // 거짓말 할 수 있는 파티 최대 수 출력
        System.out.println(answer);
    }

    public static int find(int x)
    {
        if (parent[x] == x)
            return (parent[x] = x);
        else
            return (find(parent[x]));
    }

    public static boolean union(int first, int second)
    {
        first = find(first);
        second = find(second);

        if (first != second)
        {
            if (first > second)             //  어떤 기준으로 Union(결합) 할껀데?
                parent[first] = second;     //  ㄴ 연결되어 있다면 node가 작은걸로
            else
                parent[second] = first;
            return (true);
        }
        return (false);
    }
}
```

아직 Union-Find가 어색하여 같은 유형의 많은 문제를 풀어보는 것이 좋을 듯 하다.

---



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

4. ~~<strong>Another simple version</strong>~~

- [x] ARG="<span style="color:blue">1 5 2 4 3</span>"; ./push_swap $ARG | ./checker_Mac $ARG <span style="color:red">result line: ~12</span>
- [x] ARG="<span style="color:blue">5 random</span>"; ./push_swap $ARG | ./checker_Mac $ARG <span style="color:red">result line: ~12</span> 
  - // 검증하기 전에 이 테스트를 여러 순열로 두 번 반복해야 합니다.

5. <strong>Middle version</strong>

- [ ] ARG="<span style="color:blue">100 random</span>"; ./push_swap $ARG | ./checker_Mac $ARG 

  <span style="color:red">result line: </span>

  - [ ] 700 미만: 5점

  - [ ] 900 미만: 4점
  - [ ] 1100 미만: 3점
  - [x] 1300 미만: 2점
  - [ ] 1500 미만: 1점

6. <strong>Advannced version</strong>

- [ ] ARG="<span style="color:blue">500 random</span>"; ./push_swap $ARG | ./checker_Mac $ARG 

  <span style="color:red">result line: </span>

  - [ ] 5500 미만: 5점
  - [ ] 7000 미만: 4점
  - [ ] 8500 미만: 3점
  - [x] 10000 미만: 2점
  - [ ] 11500 미만: 1점

---

#### 5개 짜리

- 3개 짜리로 만드는 것이 목표입니다.
- `for (a 노드 개수) if (a 노드 값 == max || a 노드 값 == min) pa() else ra() 3개짜리 정렬() b stack에서 a stack으로 다시 넘겨주기`
- => b stack으로 가장 큰수와 가장 작은 수 2개를 넘겨주어서 3개 정렬 후 2개를 다시 넘겨줍니다.



java로 TC를 구하기 위해서 BackTracking을 이용하여 순열, 그리고 랜덤으로 순열 하나를 출력하는 프로그램

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;
import java.util.Random;

public class TEST
{
    static int[] arr;
    static boolean[] check;
    static int cnt = 0;
    static List<String> list = new ArrayList<>();
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        arr = new int[n];
        check = new boolean[n + 1];
        Random r = new Random();
        DFS(0, n);
        System.out.println(list.get(r.nextInt(factorial(n))));
    }

    public static void DFS(int depth, int n)
    {
        if (depth == n)
        {
            ++cnt;
            StringBuilder sb = new StringBuilder();
            for (int i : arr)
                sb.append(i + " ");
            list.add(sb.toString());
            return;
        }

        for (int i = 1; i <= n; i++)
        {
            if (check[i])
                continue;
            check[i] = true;
            arr[depth] = i;
            DFS(depth + 1, n);
            check[i] = false;
        }
    }

    public static int factorial(int n)
    {
        if (n == 1)
            return (1);
        return (n * factorial(n - 1));
    }
}
```

checker_Mac 쉘 책점 프로그램