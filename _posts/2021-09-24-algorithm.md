---
layout: post
title: "TIL 백준 중급 알고리즘, 42seoul(push_swap)"
author: "hyunwlee"
---

## <span style="background-color:#1D6A96; color:white">BOJ 18116 로봇 조립</span>

<span style="background-color:#85B8CB; color:white"><strong> Category</strong></span>

백준, [로봇 조립](https://www.acmicpc.net/problem/18116), 그래프

<img src="https://github.com/hyunwlee-dev/TIL/blob/f7238cc11efd5091ab6801cac78ff82c9f255651/images/boj_robotAssembly.png?raw=true" style="zoom:50%;" />  

<span style="background-color:#85B8CB; color:white"><strong>시간복잡도</strong></span>

...



<span style="background-color:#85B8CB; color:white"><strong>풀이</strong></span>

I: 부품 2개가 하나의 로봇에 쓰이는 부품이라는 것을 알려줄때, 2개 부품을 Union을 해준다.

Q: 로봇에 쓰인 부품을 cnt배열을 만들어 Union내부에서 부모원소에 들어오는 자식원소 개수 마다 카운트 처리하여 cnt[find[num]]으로 결과를 도출한다.


```
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class 로봇 조립 {
    static int[] parent;
    static int[] cnt;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        parent = new int[1000001];
        cnt = new int[1000001];
        for (int i = 0; i <= 1000000; i++)
        {
            parent[i] = i;
            cnt[i] = 1;
        }
        StringTokenizer st;
        for (int i = 0; i < n; i++)
        {
            st = new StringTokenizer(br.readLine());
            char c = st.nextToken().charAt(0);
            if (c == 'I')
            {
                int first = Integer.parseInt(st.nextToken());
                int second = Integer.parseInt(st.nextToken());
                union(first, second);
            }
            else if (c == 'Q')
            {
                int num = Integer.parseInt(st.nextToken());
                System.out.println(cnt[parent[num]]);
            }
        }
    }
    public static int find(int x)
    {
        if (parent[x] == x)
            return (x);
        else
            return (parent[x] = find(parent[x]));
    }

    public static boolean union(int first, int second)
    {
        first = find(first);
        second = find(second);

        if (first != second)
        {
            if (first > second)
            {
                cnt[second] += cnt[first];
                cnt[first] = 0;
                parent[first] = second;
            }
            else
            {
                cnt[first] += cnt[second];
                cnt[second] = 0;
                parent[second] = first;
            }
            return (true);
        }
        return (false);
    }
}
```

결과: FAIL

10트라이 정도 해봤는데 왜 틀린지 몰라 결국 질문을 올렸다..

도저히 어디가 잘못됬는지 답변이 빨리 달렸으면 좋겠다.

---

## <span style="background-color:#E81E25; color:white">42seoul(push_swap)</span>

#### <span style="background-color:#FE7773; color:white">동료 학습 방법</span>

mchun (동료 학습)

#### <span style="background-color:#FE7773; color:white">학습 목표</span>

최적화

5. <span style="background-color:#FFC2C3; color:white"><strong>Middle version</strong></span>

- [ ] ARG="<span style="color:blue">100 random</span>"; ./push_swap $ARG | ./checker_Mac $ARG 

  <span style="color:red">result line: </span>

  - [ ] 700 미만: 5점

  - [x] 900 미만: 4점
  - [ ] 1100 미만: 3점
  - [ ] 1300 미만: 2점
  - [ ] 1500 미만: 1점

6. <span style="background-color:#FFC2C3; color:white"><strong>Advance version</strong></span>

- [ ] ARG="<span style="color:blue">500 random</span>"; ./push_swap $ARG | ./checker_Mac $ARG 

  <span style="color:red">result line: </span>

  - [x] 5500 미만: 5점
  - [ ] 7000 미만: 4점
  - [ ] 8500 미만: 3점
  - [ ] 10000 미만: 2점
  - [ ] 11500 미만: 1점

---

push_swap을 해결하였다.

subject_push_swap.pdf 문제를 읽어보면 <span style="color:red">여러 랜덤의 100개가 섞여있는 수들 </span> 또는 <span style="color:red">여러 랜덤의 500개의 섞여있는 수들</span> 이 입력으로 들어왔을 때 result line이 안정적으로 <span style="color:red">700개 미만</span>으로 결과를 추리길 원한다는 것을 보자마자 코드에서 문제점이 무엇인지 빨리 알아 차렸어야 했다.

내 코드 sort에서, pivot을 사용했으므로 pivot이 어딘지에 따라 정렬되는 순서가 천차만별일 것이다.

pivot을 최적인 곳을 잡아주지 않았을 때는 랜덤으로 들어오는 입력값에 따라 결과값도 안정적으로 나올 때도 있고 최소 적합해야하는 결과마저 훌쩍 넘어버릴때도 있었다.



최적의 pivot위치를 미리 알고 잡아준다면? 명령어 수를 최적으로 맞출 수 있을 뿐더러 결과 범위가 들쑥날쑥하지도 않는다.



그림 [어떻게 해결 했는지]

두 pivot의 위치를 찾기위해 분할된 크기만큼 malloc을 사용해서 원소값들을 모두 copy시키고 정렬시킨 다음 1/3, 2/3지점의 원소들을 pivot으로 삼았다.

pivot을 찾은 뒤에는 당연히 메모리를 free해주었고 분할 될때마다 새로 할당 -> free를 반복해 준다.