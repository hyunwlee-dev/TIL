---
layout: post
title: "TIL java basic Tree"
author: "hyunwlee"
---

## 1. Java<span style="background-color:green; color:white"> Tree </span>

<span style="background-color:lightgreen">__트리의 용어__</span>

```
말단 노드: 자식이 없는 노드 (leaf node)

```



<span style="background-color:lightgreen">__트리 자료구조__</span>

__FILE System__: /Users/hyunwlee/Projects/, __검색 엔진__, __라우터 통신 알고리즘__, __DBMS__, __HTML Tag구조__

위의 예시들은 계층 구조(부모-자식 관계)의 대표적 자료구조이다.



<span style="background-color:lightgreen">__Tree 특징__</span>

1. 하나의 루트 노드를 갖는다.
2. 부모 자식 관계가 존재한다.
3. 사이클(cycle)이 존재할 수 없다. <span style="color:grey">_DAG(Directed Acyclic Grap)로 사이클이 존재하지 않는 방향 그래프를 말한다._</span>



<span style="background-color:lightgreen">__트리 vs. 이진트리__</span>

```
class TreeOfNode
{
	int			value;
	Node[]	children; 	// List<node> children
}
```

```
class BinaryTreeOfNode
{
	int			value;
	Node		left;
	Node		right;
}
```

만약 전화번호를 표현할 때 각 노드가 10개의 자식을 갖는 10차 트리(10-ary-tree)를 사용해야 할 것이다.



<span style="background-color:lightgreen">__이진트리 vs. 이진 탐색 트리__</span>

이진탐색트리의 '모든 왼쪽 자식들 <= n < 모든 오른쪽 자식들' 속성은 모든 노드 n에 대해서 반드시 참

즉, 정렬되어 있어야 한다.

​					__이진 탐색 트리가 맞음__												__이진 탐색 트리가 아님__

<img src="https://github.com/hyunwlee-dev/TIL/blob/5ea043340aac2691082d476c1da1fcee5d88f265/images/tree/tree1.png?raw=true" style="zoom:40%;" /><img src="https://github.com/hyunwlee-dev/TIL/blob/5ea043340aac2691082d476c1da1fcee5d88f265/images/tree/tree2.png?raw=true" style="zoom:45%;" />

면접 질문이나 코딩 문제에 트리 문제가 주어지면 이진 탐색 트리일 것이라고 가정해버린다.

이진 탐색 트리인지 아닌지 확실하게 묻도록 하자.



<span style="background-color:lightgreen">__균형 vs. 비균형__</span>

여기서 `완전 이진 트리`를 의미하진 않는다. 균형 트리인지 아닌지 확인하는 방법 중 하나는 '너무 불균형한건 아닌지' 확인하는 것 이상의 의미를 갖는다.

<span style="background-color:lightgrey">O(logN) 시간에 __insert__와 __find__를 할 수 있을 정도로 균형이 잘 잡혀 있지만, 그렇다고 꼭 완벽하게 균형 잡혀 있을 필요는 없다.</span>

__균형트리__: 레드-블랙 트리, AVL 트리



<span style="background-color:lightgreen">__완전 이진 트리(complete binary tree)__</span>

트리의 모든 높이에서 노드가 꽉 차 있는 이진 트리를 말한다.

마지막 level은 꽉 차잇지 않아도 되지만 왼쪽에서 오른쪽으로 채워져야 한다.



<img src="https://github.com/hyunwlee-dev/TIL/blob/dcb81b4092c33bc2ca45e315bcc07b4b9fe37bfd/images/tree/tree3.png?raw=true" style="zoom:40%;"/>



<span style="background-color:lightgreen">__전 이진 트리(full binary tree)__</span>

모든 노드의 자식이 없거나 정확히 두 개 있는 경우

<img src="https://github.com/hyunwlee-dev/TIL/blob/26186566e0d924270d2e571b12de0024e58d08e7/images/tree/tree4.png?raw=true" style="zoom:50%;"/>



<span style="background-color:lightgreen">__포화 이진 트리(perfect binary tree)__</span>

모든 말단 노드는 같은 높이에 있어야 하며, 마지막 단계에서 노드의 개수가 최대가 되어야 한다.

노드의 개수가 정확히 2<sup>k-1</sup>(k는 트리의 높이)개여야 한다.
