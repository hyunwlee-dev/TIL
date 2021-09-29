---
layout: post
title: "TIL 백준 중급 알고리즘"
author: "hyunwlee"
---

## <span style="background-color:#1D6A96; color:white">BOJ 2212 센서</span>

<span style="background-color:#85B8CB; color:white"><strong>Category</strong></span>

백준, [센서](https://www.acmicpc.net/problem/2212), 그리디알고리즘, 골드5

<img src="https://github.com/hyunwlee-dev/TIL/blob/6a77d0123737c627f97865cbd69789e70f7d8528/images/boj2212sensor.png?raw=true" style="zoom:50%;" />  


---

<span style="background-color:#85B8CB; color:white"><strong> 시간복잡도</strong></span>

2초  

1<= N <= 10,000  

[처음 풀이] Sort, BackTracking(조합): O (N * (N ^ 2)(Combination) * N) + 심지어 문제를 이해하지도 못함  

[나중 풀이] Sort, Greedy: O (N * logN)  

---

<span style="background-color:#85B8CB; color:white"><strong> 풀이 </strong></span>

문제를 이해하기 어려웠고, 실제로 문제를 잘못 이해해서 BruteForce로 풀이를 시작했다.  


`처음`에는 집중국을 좌표 위치에 1 ~ K개를 조합(Combination)으로 가능한 위치 모든 경우를 찾아 집중국과 센서 거리의 합의 최소값을 찾아냈다. (⭐️ <strong>문제도 제대로 이해하지 않고, 시간 복잡도 생각을 하지 않고 푼 대참사</strong>) (모든 걸 Brute Force로 풀려 하지마...)  

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class 센서 {
    static List<Integer> list;
    static boolean[] check;
    static int answer = Integer.MAX_VALUE;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        int K = Integer.parseInt(br.readLine());
        // 예외처리
        if (K >= N)
        {
            System.out.println(0);
            return;
        }
        StringTokenizer st = new StringTokenizer(br.readLine());
        // 문제를 잘못이해한 참극 => Set 왜...쓰니?
        Set<Integer> set = new HashSet<>();
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < N; i++)
        {
            int temp = Integer.parseInt(st.nextToken());
            max = Math.max(max, temp);
            set.add(temp);
        }
        check = new boolean[max + 1];
        int idx = 0;
        int[] arr = new int[set.size()];
        for (Integer i : set)
            arr[idx++] = i;
        Arrays.sort(arr);
	      // 1 ~ K개를 조합(Combination)
        for (int i = 1; i <= K; i++)
        {
            list = new ArrayList<>();
            DFS(0, i, max, 0, arr);
        }
        System.out.println(answer);
    }
    // Back Tracking = N과 M 시리즈 중 조합(Combination)
    public static void DFS(int depth, int K, int max, int start, int[] arr)
    {
        if (list.size() == K)
        {
            int sum = 0;
            for (int i = 0; i < arr.length; i++)
            {
                int subMin = Integer.MAX_VALUE;
                for (int l : list)
                    subMin = Math.min(subMin, Math.abs(l - arr[i]));
                sum += subMin;
            }
            answer = Math.min(sum, answer);
            return;
        }

        for (int i = start; i <= max; i++)
        {
            if (check[i])
                continue;
            check[i] = true;
            list.add(i);
            DFS(depth + 1, K, max, i + 1, arr);
            list.remove(depth);
            check[i] = false;
        }
    }
}
```

---


`나중` 에는 그리디로 <strong>O(nlogn)</strong>만에 가능하다는 것을 알게되어 어떻게 풀었는지 설명해보겠다.

1. 집중국(k) >= 센서(n)이면, 집중국이 각각 센서를 담당하면 되므로 출력 (0)후 종료
2. 각 센서 거리 차이를 담은 list (오름차순)
3. list의 0 ~ (n - k)를 합해 출력한다. => 거리가 긴 것은 뛰어 넘을 수 있다.

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class 센서 {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        int k = Integer.parseInt(br.readLine());

        // 예외처리
        if (k >= n)
        {
            System.out.println(0);
            return;
        }
        int[] arr = new int[n];
        StringTokenizer st = new StringTokenizer(br.readLine());
        for (int i = 0; i < n; i++)
            arr[i] = Integer.parseInt(st.nextToken());
        Arrays.sort(arr);
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < arr.length - 1; i++)    // 각 센서별 거리
            list.add(arr[i + 1] - arr[i]);
        Collections.sort(list);
        int answer = 0;
        for (int i = 0; i < n - k; i++)
            answer += list.get(i);
        System.out.println(answer);
    }
}
```

















