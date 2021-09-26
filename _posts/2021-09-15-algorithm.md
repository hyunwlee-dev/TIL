---
layout: post
title: "TIL java basic Priority Queue사용법, 백준 중급 알고리즘"
author: "hyunwlee"
---

## 1. Java Dijkstra에서 Priority Queue의 Comparable

> Priority Queue

우선순위 큐는 Queue인터페이스의 구현체 중의 하나로, 저장한 순서에 관계없이 우선순위(priority)가 높은 것 부터 꺼내게 된다는 특징이 있다. <strong>Priority Queue는 저장공간으로 배열을 사용하며, 각 요소를 '힙(heap)'이라는 자료구조의 형태로 저장한다.</strong> 데이터를 삽입할 때 우선순위를 기준으로 최대힙 또는 최소힙을 구성하고 데이터를 꺼낼 때 루트 노드를 얻어낸다. 루트 노드를 삭제할 때는 빈 루트 노드 위치에 맨 마지막 노드를 삽입한 후 아래로 내려가면서 적절한 자리를 찾아서 옮긴다.

힙은 이진 트리의 한 종류로 가장 큰 값이나 가장 작은 값을 빠르게 찾을 수 있다는 특징이 있다.

<span style="color:red"><strong>Priority Queue는 숫자 뿐만 아니라 객체를 저장할 수 있는데 그럴 경우 각 객체의 크기를 비교할 수 있는 방법을 제공해야 한다.</strong></span>

> Priority Queue 예제 <strong>(숫자)</strong>

```java
import java.util.PriorityQueue;

public class Main {
    public static void main(String[] args) {
        PriorityQueue<Integer> pq = new PriorityQueue();
        pq.offer(5);    // pq.offer(new Integer(5)); 컴파일러가 Integer 로 오토박싱 해준다.
        pq.offer(1);
        pq.offer(4);
        pq.offer(2);
        pq.offer(3);
        System.out.println(pq);
        while (!pq.isEmpty())
            System.out.println(pq.poll());
    }
}
```

컴파일러가 Integer로 오토박싱 해준다.

Integer와 같은 <span style="color:red">Number의 자손들은 자체적으로 숫자를 비교하는 방법을 정의하고 있기 때문에</span> 비교 방법을 지정해 주지 않아도 된다.



> Prioirty Queue 예제 <strong>(객체의 크기를 비교할 수 있는 방법을 제공 X)</strong>

```java
import java.util.PriorityQueue;

public class Main {
    static class Edge
    {
        int to;
        int weight;
        public Edge(int to, int weight)
        {
            this.to = to;
            this.weight = weight;
        }
    }
    public static void main(String[] args) {
        PriorityQueue<Edge> pq = new PriorityQueue();
        pq.offer(new Edge(2, 2));
        pq.offer(new Edge(3, 3));
        pq.offer(new Edge(4, 1));
        pq.offer(new Edge(5, 10));
        pq.offer(new Edge(4, 2));
        pq.offer(new Edge(4, 1));
        pq.offer(new Edge(5, 1));
        pq.offer(new Edge(5, 3));
        for (Edge e : pq)
            System.out.println("End: " + e.to + " Weight: " + e.weight);
    }
}
```

에러메세지:  
<span style="color:red">Exception in thread "main" [java.lang.ClassCastException](https://docs.oracle.com/javase/9/docs/api/java/lang/ClassCastException.html): Main$Edge cannot be cast to java.lang.Comparable</span>  
<span style="color:red">at PriorityQueue.siftUpComparable()</span>  
<span style="color:red">at PriorityQueue.siftUp()</span>  
<span style="color:red">at PriorityQueue.offer()</span>  

```java
public boolean offer(E e) {
  if (e == null)
    throw new NullPointerException();
  modCount++;
  int i = size;
  if (i >= queue.length)
  	grow(i + 1);
  siftUp(i, e);
  size = i + 1;
  return true;
}
```

```java
private void siftUp(int k, E x) {
	if (comparator != null)
		siftUpUsingComparator(k, x, queue, comparator);
	else
		siftUpComparable(k, x, queue);
}
```

```java
private static <T> void siftUpComparable(int k, T x, Object[] es) {
	Comparable<? super T> key = (Comparable<? super T>) x; // ⭐️UpCasting 실패
	while (k > 0) {
	int parent = (k - 1) >>> 1;
	Object e = es[parent];
	if (key.compareTo((T) e) >= 0)
		break;
	es[k] = e;
	k = parent;
	}
	es[k] = key;
}
```



위의 예제는 `Comparable` 인터페이스를 구현한 객체가 아니다.  

우선순위 큐의 `offer`는 큐 한쪽 끝에 엘리먼트를 저장하는데 이때 `다형성`을 이용하여 추가 되는 엘리먼트 객체를 Comparable 인터페이스로 `Up Casting`한다.  

<span style="color:gray">_정확한 에러에 대한 reference를 찾아보고 싶으면 `casting 와일드카드`를 보고  와야겠다._</span>



> Prioirty Queue 예제 <strong>(객체의 크기를 비교할 수 있는 방법을 제공 O)</strong>

```java
import java.util.PriorityQueue;

public class Main {
    static class Edge implements Comparable<Edge>
    {
        int to;
        int weight;
        public Edge(int to, int weight)
        {
            this.to = to;
            this.weight = weight;
        }

        @Override
        public int compareTo(Edge e) {
            return (this.weight - e.weight);
        }
    }
    public static void main(String[] args) {
        PriorityQueue<Edge> pq = new PriorityQueue();
        pq.offer(new Edge(2, 2));
        pq.offer(new Edge(3, 3));
        pq.offer(new Edge(4, 1));
        pq.offer(new Edge(5, 10));
        pq.offer(new Edge(4, 2));
        pq.offer(new Edge(4, 1));
        pq.offer(new Edge(5, 1));
        pq.offer(new Edge(5, 3));
        for (Edge e : pq)
            System.out.println("End: " + e.to + " Weight: " + e.weight);
    }
}

```

---
## 2. BOJ 1916 최소비용 구하기

> Category

백준, [최소비용 구하기](https://www.acmicpc.net/problem/1916), 다익스트라 알고리즘, 골드5

<img src="https://github.com/hyunwlee-dev/TIL/blob/a247fc9b5dcbff8990ac51d9d59cde5474bd016e/images/findMinimumCost.png?raw=true" style="zoom:50%;" />  



> 시간복잡도

다익스트라 시간복잡도: O(|M||log|N|)

> 풀이

Dijkstra는 dp를 활용한 대표적인 최단 경로(Shortest Path) 탐색 알고리즘이다. 하나의 최단 거리를 구할 때 그 이전까지 구했던 최단 거리 정보를 그대로 사용한다.  

weight (버스 비용)이 모두 양수이기 때문에 Dijkstra알고리즘을 활용할 수 있다.

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class 최소비용 구하기 {
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
            return this.weight - n.weight;
        }
    }
    static List<Node>[] graph;
    static boolean[] check;
    static int[] dist;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        int m = Integer.parseInt(br.readLine());
        graph = new ArrayList[n + 1];
        for (int i = 0; i < n + 1; i++)
            graph[i] = new ArrayList<>();
        check = new boolean[n + 1];
        dist = new int[n + 1];
        Arrays.fill(dist, Integer.MAX_VALUE);
        StringTokenizer st;
        for (int i = 0; i < m; i++)
        {
            st = new StringTokenizer(br.readLine());
            int start = Integer.parseInt(st.nextToken());
            int end = Integer.parseInt(st.nextToken());
            int weight = Integer.parseInt(st.nextToken());
            graph[start].add(new Node(end, weight));
        }
        st = new StringTokenizer(br.readLine());
        int start = Integer.parseInt(st.nextToken());
        int end = Integer.parseInt(st.nextToken());
        System.out.println(dijkstra(start, end));
    }
    // 다익스트라 알고리즘
    public static int dijkstra(int start, int end)
    {
        PriorityQueue<Node> pq = new PriorityQueue<>();
        pq.offer(new Node(start, 0));
        dist[start] = 0;
        // check[from] = true; // dijkstra의 특이점 // 첫 노드 방문을 표시 안한다.
        while (!pq.isEmpty())
        {
            Node node = pq.poll();
            int current = node.end;
            if (check[current])
                continue;
            check[current] = true;
            for (Node n : graph[current])
            {
                if (!check[n.end] && dist[n.end] > dist[current] + n.weight)
                {
                    dist[n.end] = dist[current] + n.weight;
                    pq.offer(new Node(n.end, dist[n.end]));
                }
            }
        }
        return (dist[end]);
    }
}
```
