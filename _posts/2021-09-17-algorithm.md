---
layout: post
title: "TIL 백준 중급 알고리즘, 42seoul(push_swap)"
author: "hyunwlee"
---

## <span style="background-color:#1D6A96; color:white">BOJ 1339 단어 수학</span>

<span style="background-color:#85B8CB; color:white"><strong> Category </strong></span>

백준, [단어 수학](https://www.acmicpc.net/problem/1339), 그리디 알고리즘, 골드4

<img src="https://github.com/hyunwlee-dev/TIL/blob/096a799e4c30ece6d403d96a804fc0b6bcda2066/images/wordDictionary.png?raw=true" style="zoom:50%;" />  



<span style="background-color:#85B8CB; color:white"><strong> 시간복잡도 </strong></span>

1. BruteForce: O(N(단어의 개수 1 <= N <= 10)! * M(알파벳 개수 1 <=M <= 26)) => 통과는 하지만 효율성↓  
2. <strong>Greedy: O(N ^ 2)</strong>



<span style="background-color:#85B8CB; color:white"><strong> 풀이 </strong></span>

BruteFoce  

1. HashSet - 중복 알파벳을 방지하기 위해 사용
2. HashMap - 알파벳의 value를 매핑
3. BackTracking을 이용한 순열(Permutation)을 이용하여 각 알파벳의 모든 경우 수를 구한 후 최대로 큰 값을 출력

```
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class 단어 수학 {
    static int[] arr;
    static boolean[] check;
    static int answer = 0;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        Set<Character> set = new HashSet<>();
        Map<Integer, Character> map = new HashMap<>();
        Map<Character, Integer> wordGrade = new HashMap<>();
        StringTokenizer st;
        List<String> list = new ArrayList<>();
        for (int i = 0; i < n; i++)
        {
            st = new StringTokenizer(br.readLine());
            String s = st.nextToken();
            list.add(s);
            for (char c : s.toCharArray())
                set.add(c);
        }
        int idx = 0;
        for (char c : set)
        {
            map.put(idx++, c);
            wordGrade.put(c, 0);
        }
        arr = new int[set.size()];
        check = new boolean[11];
        dfs(0, map, wordGrade, list);
        System.out.println(answer);
    }
    // BackTracking을 이용한 순열(Permutaion)
    public static void dfs(int depth, Map<Integer, Character> map, Map<Character, Integer> wordGrade, List<String> list)
    {
        if (depth == arr.length)
        {
            for (int i = 0; i < arr.length; i++)
                wordGrade.put(map.get(i), arr[i]);
            int total = 0;
            for (String s : list)
            {
                int sum = 0;
                for (char c : s.toCharArray()) {
                    sum = sum * 10 + wordGrade.get(c);
                }
                total += sum;
            }
            answer = Math.max(answer, total);
            return;
        }
        for (int i = 0; i < 10; i++)
        {
            if (check[i])
                continue;
            check[i] = true;
            arr[depth] = i;
            dfs(depth + 1, map, wordGrade, list);
            check[i] = false;
        }
    }
}
```
별로다.. 더 효율적인 방법 있을까?

---
Greedy  

input으로 들어오는 단어의 알파벳을 각 자리 수의 위치마다 1로 나타내어 Mapping후 최대힙 PriorityQueue에 넣고 꺼내어 Mapping만 해주면 간단하게 해결이 되는 문제다.

예외를 있으리라 생각되어 안될 것 같았지만 <strong>들어오는 단어가 10개 이하이기 때문에 이 방법이 가능하다.</strong>

만약 10개 이하 조건이 없다면, 단어가 11개 들어올 수 있는 상황이 된다면, 

B C A (letter 1)
A (2)
​A (3)
​A (4)
​A (5)
​A (6)
​A (7)
​A (8)
​A (9)
​A (10)
​A (11)

이러한 경우에는 A의 grade > C의 grade이 이득게 되지만, 위에서 말했다시피 단어 갯수는 최대 10개이기 때문에 이러한 예외를 신경쓰지 않아 된다.


```
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class 단어 수학 {
    static class Words implements Comparable<Words>
    {
        char c;
        int value;
        public Words(char c, int value)
        {
            this.c = c;
            this.value = value;
        }
        @Override
        public int compareTo(Words w)
        {
            return -(this.value - w.value); // 내림 차순
        }
    }
    public static void main(String[] args) throws IOException {
      BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
      int n = Integer.parseInt(br.readLine());
      Map<Character, Integer> charValueMap = new HashMap<>(); // ex) letter = ABAC 일 경우, [{'A', 1010}, {'B', 100}, {'C', 1}]
      PriorityQueue<Words> pq = new PriorityQueue<>();        // charValueMap의 value가 큰 순으로
      Map<Character, Integer> charGradeMap = new HashMap<>(); // charValueMap의 value가 클수록 높은 grade [{'A', 9}, {'B', 8}, {'C', 7}]
      List<String> wordList = new ArrayList<>();
      for (int i = 0; i < n; i++)
      {
          String s = br.readLine();
          wordList.add(s);
          for (int j = 0; j < s.length(); j++)
          {
              int length = (int)Math.pow(10 , (s.length() - 1 - j));
              if (charValueMap.containsKey(s.charAt(j)))
              {
                  int temp = charValueMap.get(s.charAt(j));
                  temp += length;
                  charValueMap.put(s.charAt(j), temp);
              }
              else
                charValueMap.put(s.charAt(j), length);
          }
      }
      for (Map.Entry<Character, Integer> m : charValueMap.entrySet())
          pq.offer(new Words(m.getKey(), m.getValue()));
      int grade = 9;
      while (!pq.isEmpty())
      {
          Words w = pq.poll();
          charGradeMap.put(w.c, grade--);
      }
      int answer = 0;
      for (String s : wordList)
      {
          int seung = s.length() - 1;
          int sum = 0;
          for (char c : s.toCharArray()) {
              sum += charGradeMap.get(c) * (int) Math.pow(10, seung--);
          }
          answer += sum;
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

1. <span style="background-color:#FFC2C3; color:white"><strong>Error management</strong></span>

- [x] list of integers 중 한 원소가 integer가 아닐 경우 <span style="color:red">result: Error</span>

2. <span style="background-color:#FFC2C3; color:white"><strong>Identity test</strong></span>

- [x] ./push_swap <span style="color:blue">42</span> <span style="color:red">result:</span>
- [ ] ./push_swap <span style="color:blue">0 1 2 3</span> <span style="color:red">result: </span>
- [ ] ./push_swap <span style="color:blue">0 1 2 3 4 5 6 7 8 9</span> <span style="color:red">result: </span>

---

Error Management에 적합하기 위해서 헤더에 validation구조체를 선언해주도록 하였다.

- is_digit과 is_integer는 input이 들어오는대로 검출 할 수있어 따로 check_validation함수로 빼놓기로 결정했다.

```
typedef struct s_validation
{
    int is_sorted;	// 정렬되어 있는 상태인가?
    int is_duplicated;	// stack들어 있는 원소가 중복인 상태인가?
    // int is_digit;	// 원소가 정수인가?
    // int is_integer;	// 원소 범위가 Integer인가?
}              t_validation;
```



- 환경변수에 랜덤으로 값을 넣어주는 명령어

```
ARG=`ruby -e "puts (0..10).to_a.shuffle.join(' ')"`; ./push_swap $ARG
```

