---
layout: post
title: "TIL 백준 중급 알고리즘, 42seoul(push_swap)"
author: "hyunwlee"
---

## 1. BOJ 1339 단어 수학

> Category

백준, [단어 수학](https://www.acmicpc.net/problem/1339), 그리디 알고리즘, 골드4

<img src="https://github.com/hyunwlee-dev/TIL/blob/096a799e4c30ece6d403d96a804fc0b6bcda2066/images/wordDictionary.png?raw=true" style="zoom:50%;" />  



> 시간복잡도

1. BruteForce: O(N(단어의 개수 1 <= N <= 10) ^ 2 * M(알파벳 개수 1 <=M <= 26)) => 통과는 하지만 효율성↓

2. <span style="color:red">Greedy: O(N ^ 2)</span>

> 풀이

BruteFoce  

1. HashSet - 중복 알파벳을 방지하기 위해 사용

2. HashMap - 알파벳의 value를 매핑
3. BackTracking을 이용한 조합(Combination)을 이용하여 각 알파벳의 모든 경우수를 구한 후 최대로 큰 값을 출력

```markdown
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
        DFS(0, map, wordGrade, list);
        System.out.println(answer);
    }

    public static void DFS(int depth, Map<Integer, Character> map, Map<Character, Integer> wordGrade, List<String> list)
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
            DFS(depth + 1, map, wordGrade, list);
            check[i] = false;
        }
    }
}
```

별로다.. 더 효율적인 방법 있다!

---
Greedy  

input으로 들어오는 단어의 알파벳을 각 자리 수의 위치마다 1로 나타내어 Mapping후 PriorityQueue에 넣어 꺼내어 Mapping만 해주면 간단하게 해결이 되는 문제다.

예외를 있으리라 생각되어 안될 것 같았지만 <span style="color:red">들어오는 단어가 10개 이하이기 때문에 이 방법이 가능할 것이라 생각 되었다.</span>

만약 10개 이하 조건이 없다면, 단어가 11개 들어올 수 있는 상황이 된다면, 

B C A (letter 1)

​	   A (2)

​       A (3)

​       A (4)

​       A (5)

​       A (6)

​       A (7)

​       A (8)

​       A (9)

​       A (10)

​       A (11)

이러한 경우에는 A의 grade > C의 grade이 이득을 본다. 하지만, 10개 이하이기 때문에 이러한 그리디 방법이 통할 수 있다.


```markdown
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
            return -(this.value - w.value);
        }
    }
    public static void main(String[] args) throws IOException {
      BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
      int n = Integer.parseInt(br.readLine());
      Map<Character, Integer> charValueMap = new HashMap<>();
      PriorityQueue<Words> pq = new PriorityQueue<>();
      Map<Character, Integer> charGradeMap = new HashMap<>();
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

## 2. 42seoul(push_swap)

1. 예외 처리

1) 음수?? 0??
2) 중복처리 => Brute Force

Error 를 출력해야할 경우:

- list of integers 중 한 원소가 integer가 아닐 경우
- list of integers의 값이 Integer보다 클 경우
- duplicates가 존재할 경우
- stdin에서 받은 명령어가 존재하지 않는 명령어일 경우.

PUSH_SWAP_ARG=`ruby -e "puts (1..10).to_a.shuffle.join(' ')"`; echo $PUSH_SWAP_ARG


1. Memory Leaks
2. Error management
   In this section, we'll evaluate the push_swap's error management.
   If at least one fails, no points will be awarded for this section. Move to the next one.

- Run push_swap with non numeric parameters. The program must display "Error".
- Run push_swap with a duplicate numeric parameter. The program must display "Error".
- Run push_swap with only numeric parameters including one greater than MAXINT. The program must display "Error".
- Run push_swap without any parameters. The program must not display anything and give the prompt back.

non 숫자, 중복 숫자, MAXINT -> ERROR
non Parameter -> nothing