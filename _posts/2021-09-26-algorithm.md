---
layout: post
title: "TIL java basic, 백준 중급 알고리즘"
author: "hyunwlee"
---

## 1. <span style="background-color:lightsteelblue">Java Tree Traversal (InOrder, PostOrder, PreOrder)</span>

트리를 탐색하는데 효율적으로 하는 3가지 방법에 대해서 알아보자.

- __InOrder 중위__
- __PreOrder 전위__
- __PostOrder 후위__
- <span style="color:gray">레벨 순서 순회</span>

배열이나 연결리스트처럼 선형으로 된 자료구조보다 트리에서 탐색은 많이 까다롭다.

__순회__

```
for (String s : sList)
{
	s.toString();
}
```

배열을 index 0부터 길이 만큼 반복하는 것이 Iteration이라고 불리는 순회를 하는 것이다.

여기서 나는 일반적인 배열이나, 리스트는 순회할 줄 알면서 트리에 대해 순회는 잘 해내는가?

아니다. 좀 생소하다. <span style="color:grey">_트리와 그래프를 밑바닥부터 능숙하게 구현할수 있는 능력을 갖추면 도움이 될 것이다.)_</span>

<span style="color:grey">_트리가 그래프의 한 부분이고 트리가 그래프보다 좀 더 단순하다._</span>

트리의 순회는 위에서 말했듯이 여러 순회방법이 존재한다.



1. <span style="background-color:lightgreen">__InOrder 중위 순회__</span>

Left, Current, Right

```
public void inOder(Node node)
{
	if (node == null)
		return ;
	inOrder(node.left);
	System.out.println(node.data);
	inOrder(node.right);
}
```

2. <span style="background-color:lightgreen">__PreOrder 전위 순회__</span>

Current, Left, Right

```
public void preOrder(Node node)
{
	if (node == null)
		return ;
	System.out.println(node.data);
	preOrder(node.left);
	preOrder(node.right);
}
```

3. <span style="background-color:lightgreen">__PostOrder 후위 순회__</span>

Left, Right, Current

```
public void postOrder(Node node)
{
	if (node == null)
		return ;
	postOrder(node.left);
	postOrder(node.right);
	System.out.println(node.data);
}
```



<span style="background-color:lightgreen">__순회 순서__</span>

```
        (1)
        / \
      (2) (3)
      / \
    (4) (5)

Inorder   (Left, Root, Right): 4 2 5 1 3
Preorder  (Root, Left, Right): 1 2 4 5 3
Postorder (Left, Right, Root): 4 5 2 3 1
```

[__자세한 BinaryTreeTraversal 코드__](https://github.com/hyunwlee-dev/problem-solving/blob/master/datastructure/tree/Binary_Tree_Traversal.md){:target="_blank"}

---

## 2. <span style="background-color:lightsteelblue">BOJ 2263 트리의 순회</span>

> Category

백준, [트리의 순회](https://www.acmicpc.net/problem/2263), 분할 정복, 골드3

<img src="https://github.com/hyunwlee-dev/TIL/blob/fdc991f388d5b26d47b85afe210ba42a38d95ae7/images/tree/treeTraversal.png?raw=true" style="zoom:50%;" />  




> 풀이

<span style="background-color:lightgreen">트리 순회</span>에 대한 규칙성을 가지고 <span style="background-color:lightsteelblue">__분할 정복__</span>으로 풀어내는 문제였으며 분할이 되어가는 과정을 직접 그려나가면서 해결하려 했다.



<img src="https://github.com/hyunwlee-dev/TIL/blob/258c2c2b5e58b3bbd89b8f1ff183cfb55f3d08f1/images/tree/tree5.png?raw=true" style="zoom:50%;" />

__PreOrder__:   1 2 4 5 7 3 6

__InOrder__:      4 2 7 5 1 3 6

__PostOrder__: 4 7 5 2 6 3 1



__InOrder__와 __PostOrder__ 규칙

- postOrder의 마지막 노드는 트리의 <span style="background-color:red">__루트__</span>이다.

- 루트를 기준으로 InOrder의 왼쪽이 __루트__의 <span style="background-color:lightsteelblue">왼쪽 자식 노드들</span>이고 오른쪽이 <span style="background-color:lightgray">오른쪽 자식 노드들</span>이다.

---

<span style="background-color:black; color:white">  __depth == 0__  </span> 	 recurse(0 ~ 6 /  0 ~ 6)								          <span style="background-color:white"> buf: <u>__1__</u> </span>

__InOrder__:      <span style="background-color:lightsteelblue"> 4 2 7 5 </span><span style="background-color:red">  1 </span><span style="background-color:lightgray"> 3 6 </span>

__PostOrder__: <span style="background-color:lightsteelblue"> 4 7 5 2 </span><span style="background-color:lightgray"> 6 3 </span><span style="background-color:red"> 1 </span>

---

​			<span style="background-color:black; color:white">  __depth == 1__  </span> 	__left_nodes__ recurse(0 ~ 4 /  0 ~ 4)	     		<span style="background-color:white"> buf: __1  <u>2</u>__ </span>

​			__InOrder__:      <span style="background-color:lightsteelblue"> 4 </span><span style="background-color:red"> 2 </span><span style="background-color:lightgray"> 7 5  </span>

​			__PostOrder__: <span style="background-color:lightsteelblue"> 4 </span><span style="background-color:lightgray"> 7  5 </span><span style="background-color:red"> 2 </span>

---

​						~~<span style="background-color:black; color:white">  __depth == 2__  </span> 	__left_nodes__ recurse(0 ~ 0 /  0 ~ 4)~~	    	<span style="background-color:white"> buf: __1  2  <u>4</u>__ </span>

​						~~__InOrder__:      <span style="background-color:lightgreen"> 4 </span>~~

​						~~__PostOrder__: <span style="background-color:lightgreen"> 4 </span>~~

---

​						<span style="background-color:black; color:white">  __depth == 2__  </span> 	__right_nodes__ recurse(2 ~ 3 /  1 ~ 2) 		<span style="background-color:white"> buf: __1  2  4  <u>5</u>__ </span>

​						__InOrder__:      <span style="background-color:lightsteelblue"> 7 </span><span style="background-color:red"> 5 </span>

​						__PostOrder__: <span style="background-color:lightsteelblue"> 7 </span><span style="background-color:red"> 5 </span>

​									~~<span style="background-color:black; color:white">  __depth == 3__  </span> 	__left_nodes__ recurse(2 ~ 2 /  1 ~ 1)~~ 	   <span style="background-color:white"> buf: __1  2  4  5  <u>7</u>__ </span>

​									~~__InOrder__:      <span style="background-color:lightgreen"> 7 </span>~~

​									~~__PostOrder__: <span style="background-color:lightgreen"> 7 </span>~~

​									~~<span style="background-color:black; color:white">  __depth == 3__  </span> 	__right_nodes__ recurse(4 ~ 3 /  2 ~ 1)~~ 

---

​			<span style="background-color:black; color:white">  __depth == 1__  </span>    __right_nodes__ recurse(5 ~ 6 / 4 ~ 5)				buf: __1  2  4  5  7  <u>3</u>__ 

​			__InOrder__:      <span style="background-color:red"> 3 </span><span style="background-color:lightsteelblue"> 6 </span>

​			__PostOrder__:  <span style="background-color:lightsteelblue"> 6 </span><span style="background-color:red"> 3 </span>

​						~~<span style="background-color:black; color:white">  __depth == 2__  </span> 	__left_nodes__ recurse(5 ~ 4 /  4 ~ 3)~~

​									~~<span style="background-color:black; color:white">  __depth == 3__  </span> 	__left_nodes__ recurse(6 ~ 6 /  4 ~ 4)~~ 	   <span style="background-color:gold"> buf: __1  2  4  5  7  3  <u>6</u>__ </span>

​									~~__InOrder__:      <span style="background-color:lightgreen"> 6 </span>~~

​									~~__PostOrder__: <span style="background-color:lightgreen"> 6 </span>~~

---

재귀를 많이 풀어보아 머리에 상태트리가 바로 그려진다면 어느정도 어렵지는 않았겠지만 재귀는 어느정도는 많이 풀어봤다고 착각하고 있는 나에게 있어 이번 생소한 분할정복 재귀 문제를 만나 너무 어려웠고 고생했다. 나중에 또 풀어보면 틀릴지도 모른다. 반복 숙달이 중요한 것 같다. 어려웠지만 성장해가고 있는 느낌은 확실하게 들었다.


```
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class 트리의 순회 {
    static int n;
    static int[] inArr;
    static int[] postArr;
    static int[] preArr;

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        n = Integer.parseInt(br.readLine());
        inArr = new int[n];
        postArr = new int[n];
        preArr = new int[n];
        StringTokenizer st = new StringTokenizer(br.readLine());
        for (int i = 0; i < n; i++)
            inArr[i] = Integer.parseInt(st.nextToken());
        st = new StringTokenizer(br.readLine());
        for (int i = 0; i < n; i++)
            postArr[i] = Integer.parseInt(st.nextToken());
        solution(0, n - 1, 0, n - 1);
    }

    public static void solution(int inS, int inE, int postS, int postE) {
        if (inS > inE || postS > postE)
            return;
        if (inS == inE || postS == postE)
        {
            System.out.print(postArr[postE] + " ");
            return;
        }
        System.out.print(postArr[postE] + " ");
        int rootIdx = inS;
        for (int i = inS; i <= inE; i++)
        {
            if (inArr[i] == postArr[postE])
            {
                rootIdx = i;
                break;
            }
        }
        solution(inS, rootIdx - 1, postS, postS + rootIdx - inS - 1);
        solution(rootIdx + 1, inE, postS + rootIdx - inS, postE - 1);
    }
}
```


