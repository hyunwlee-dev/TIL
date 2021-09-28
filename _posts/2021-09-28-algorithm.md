---
layout: post
title: "TIL java collections framework"
author: "hyunwlee"
---

__들어가기 앞서__

요즘들어 주 언어인 자바로 다양한 알고리즘을 풀어보며 의구심이 들었다.

1. 나는 어떤 자료구조를 직접 구현할 수 있을까? => LinkedList, Queue, Stack, Tree Traversal정도..
2. 1번에서 여러 자료구조들을 짤수 있다고 했는데 자바에서 구현된 방식으로 가능하다는 것인가? => X (단순하게 짤 정도..)
3. 자바를 주언어로 하면서 Collection안을 들여다 본적이 있나? => 없다.

개발자라면 가장 기초가 되어야하는 자료구조를...



내가 정리할 부분은 크게 3가지로 나뉠것 같다.

1. 자료구조, 추상 데이터 타입 설명
2. 자료구조 분류, 탄생

3. 자바의 자료구조 어떤식으로 구현되어 있는지 (<strong>ADT</strong> 와 <strong>DS</strong>), 직접 구현까지도



## 1. <span style="background-color:lightsteelblue">자료구조</span>

<span style="background-color:lightgreen"><strong>DS - Data Structure</strong>></span>

1. 데이터 값의 모임
2. 데이터 간의 관계
3. <span style="background-color:lightgray">데이터에 적용할 수 있는 함수나 명령, how</span>

신중히 선택한 자료구조는 보다 효율적인 알고리즘을 사용할 수 있게 한다.

<span style="background-color:lightgray">자료구조 선택문제는 대개 추상 자료형의 선택으로부터 시작하는 경우가 많다.</span>

효과적으로 설계된 자료구조는 실행시간 혹은 메모리 용량과 같은 자원을 최소한으로 사용하면서 연산을 수행하도록 해준다.

---

<span style="background-color:lightgreen">__ADT - abstract data type__</span>

1. 자료구조의 특징, 속성, operations 설명
2. How에 대해서는 다루지 않는다, 구현 X

```
Question) 면접에서 질문 자료구조 Stack에 관하여 말씀해주세요.

Answer 1) ADT
- FILO 먼저 들어온애가 나중에 나간다.
- push, pop operation이 있다. stack 사이즈 operation, 
	stack 꼭대기에 있는 확인 operation등이 있다.

이런 답변은 ADT를 설명한 것으로 볼 수 있다.
왜냐하면, 스택의 내부적으로 어떻게 구현되어 있는지에 대해서 설명을 하지 않았기 때문에.
구현체가 있어야 한다.

Answer 2) DS
1. Array
2. Linked List
Array로 Stack을 구현한다 했을 때, size n개를 두고..
Linked List로 Stack을 구현한다 했을 때..

그럼 실제로 스택을 구현하게 되면 어떻게 구현을 할것인지 말을 하면 
=> Data Structure가 되는 것
```

---

<span style="background-color:lightgreen">__DS vs. ADT 실무에서 어떻게 적용될까?__</span>

예) 자바과목 A학점이상 맞은 학생 관리하는 자료구조? 알파벳 순서로 하고 싶음

<strong>ADT</strong>:  Alphabetical Ordered

operations

- add(사람 이름)
- getAllStudents(): 알파벳 순서대로 반환
- remove(사람 이름)

<strong>DS</strong>

구현? -> 어떻게 할지? -> 내부적으로 어떻게 sorting 알고리즘을 뭘쓸지 binary search할지.. 막 넣은다음에 중복을 지우고 sorting을 할지..

---

<span style="background-color:lightgreen">__Feat. java__</span>

```
Java	// 설계의 관점에서 되게 도움이 될것이다.
└─── Interface -> ADT
└─── Class -> DS
```



---

## 2. <span style="background-color:lightsteelblue">자료구조 분류</span>

<span style="background-color:lightgreen"><strong>분류</strong></span>

<strong>형태에 따른 자료구조</strong>

<strong>1. 선형 자료구조(Linear Data Structure)</strong>

데이터가 일렬로 연결된 형태

흔히 쓰는 int[] 배열 같은것

선형 자료구조는 대표적으로 <strong>List</strong>, <strong>Queue</strong>, <strong>Deque</strong>가 있다.



<strong> 2. 선형 자료구조(NonLinear Data Structure)</strong>

일렬로 나열된 것이 아닌, 각 요소가 여러 개의 요소와 연결 된 형태를 생각하면 된다.

<strong>Graph</strong>, <strong>Tree</strong>가 있다.



<strong>3. 집합 자료구조</strong>

두 가지 분류에 속하지 않는 <strong>set</strong> 자료구조가 있다.

이 집합의 경우는 데이터가 연결 된 형식이 아니다.

---

## 3. <span style="background-color:lightsteelblue">Java Collcetions FrameWork</span> 

<strong>Java Collections</strong>

Java에서 비슷한 류의 데이터들을 쉽게 다루기 위해 모아놓은 것들을 가공 및 처리할 수 있도록 지원하는 자료구조이다.

<strong>Framework</strong>

어떤 문제를 해결하기 위한 구조의 뼈대가 되는 기본 구조를 의미한다.

<strong>Java Collections</strong> + <strong>Framework</strong>

일정 타입의 데이들이 모여 쉽게 가공할 수 있도록 지원하는 자료구조들의 뼈대가 되는 기본 구조



자바에서 기본 구조라 한다면 ADT <strong>Interface</strong>가 떠올라야 한다.

인터페이스가 자체 기본 뼈대(추상 구조)만 구성되어 있기 때문이다.



이렇듯 자바에서 제공하는 Collection은 크게 3가지 인터페이스로 나뉘어있다.

크게 List(리스트), Queue(큐), Set(집합)으로 나뉘어 있다.

<img src="https://github.com/hyunwlee-dev/TIL/blob/a09c8eba70090af5cc291f157b009fe321ccc9e7/images/dataStructure/collections.png?raw=true" style="zoom:30%;" />



Collection을 구현한 클래스 및 인터페이스들은 모두 java.util 패키지에 있다.



<span style="background-color:lightgreen">__Iterable__</span>

Collection Interface 상위에 Iterable은 무엇일까?

<span style="background-color:lightgray">Iterable에서 제공하는 class들은 모두 객체형태로 내부 구현 또한 대게 Object[] 배열 형태가 아니라 각각의 객체를 갖고 움직인다.</span>

그래서 객체의 데이터들은 모두 순회하면서 출력하려면 사용자들이 각각의 데이터 순회 방법을 알거나 하나씩 get() 같은 메소드를 통해 데이터를 하나씩 꺼내와야 한다.

하지만 Iterable에서는 인터페이스를 보면 알겠지만 <strong>for-each</strong>를 제공한다.

즉, Iterable인터페이스를 쓰는 모든 클래스들은 기본적으로 for-each 문법을 쉽게 사용할 수 있다.

한마디로 반복자로 구현되어 나오게 하는 것이다.



추가적으로

> __Why doesn't Map extend Collection?__

This was by design. We feel that mappings are not collections and collections are not mappings. Thus, it makes little sense for Map to extend the Collection interface (or vice versa).

If a Map is a Collection, what are the elements? The only reasonable answer is "Key-value pairs", but this provides a very limited (and not particularly useful) Map abstraction. You can't ask what value a given key maps to, nor can you delete the entry for a given key without knowing what value it maps to.

Collection could be made to extend Map, but this raises the question: what are the keys? There's no really satisfactory answer, and forcing one leads to an unnatural interface.

Maps can be *viewed* as Collections (of keys, values, or pairs), and this fact is reflected in the three "Collection view operations" on Maps (keySet, entrySet, and values). While it is, in principle, possible to view a List as a Map mapping indices to elements, this has the nasty property that deleting an element from the List changes the Key associated with every element before the deleted element. That's why we don't have a map view operation on Lists.

---

## 4. <span style="background-color:lightsteelblue">List</span> 

<span style="background-color:lightgreen">List Interface</span>

대표적인 선형 자료구조로 주로 순서가 있는 데이터를 목록으로 이용할 수 있도록 만들어진 인터페이스이다.

int[] arr = new int[10];는 제한적인 10공간 외에는 더 이상 사용하지 못하게 된다.

범위를 넘어서게 되면 <strong>IndexOutOfBoundsException</strong>라는 에러가 발생한다.

이러한 단점을 보완하여 List를 통해 구현된 클래스들은 '동적 크기'를 갖으며 배열처럼 사용할 수 있게 된다.

<strong>배열의 기능 + 동적 크기 할당</strong>이 합쳐져 있다고 보면 된다.



<span style="background-color:lightgreen">List Interface Operations</span>

<img src="https://github.com/hyunwlee-dev/TIL/blob/57d42aba0af328a9df4e5741619a8f2ad3ec2a4c/images/dataStructure/listInterface.png?raw=true" style="zoom:50%;" />



<span style="background-color:lightgreen">List Interface를 구현하는 클래스</span>

1. ArrayList
2. LinkedList
3. Vector
   - Stack



<strong>ArrayList</strong>

<strong>Object[]</strong> 배열을 사용하면서 내부 구현을 통해 동적으로 관리한다.

우리가 흔히 쓰는 primitive 배열과 유사한 형태라고 보면 된다.

즉, 최상위 타입인 Object 타입으로 배열을 생성하여 사용하기 때문에 access elements에서는 탁월한 성능을 보이나, 중간의 요소가 삽입, 삭제가 일어나는 경우 그 뒤의 요소들은 한 칸씩 밀어야 하거나 당겨야 하기 때문에 삽입, 삭제에서는 비효율적인 모습을 보인다.



<strong>LinkedList</strong>

<strong>데이터(item)</strong>와 주소로 이루어진 클래스를 만들어 서로 연결하는 방식이다.

데이터와 주소로 이루어진 클래스를 Node(노드)라고 하는데, 각 노드는 <strong>노드(prev)</strong>와 <strong>노드(next)</strong>를 연결하는 방식인 것이다. 즉, 이중연결 리스트이다.

이렇다보니 요소를 검색해야 할 경우 노드(first)부터 원하는 노드까지 방문을 해야한다는 점에서 성능이 떨어지나 해당 노드를 삭제, 삽입해야 할 경우는 링크를 끊거나 연결만 해주면 되기 때문에 삽입, 삭제에서는 효율성이 높다.



<strong>Vector</strong>

ArrayList의 상위버전이다.

Object[]배열을 사용하며 요소 접근에서 빠른 성능을 보인다.

Collection Framework가 생기기 전부터 지원하던 클래스이다.

<strong>동기화</strong>를 지원한다. 쓰레드가 동시에 데이터에 접근하려하면 순차적으로 처리하도록 한다. 그렇다보니 멀티 쓰레드에서는 안전하지만, 단일 쓰레드에서도 동기화를 하기 때문에 ArrayList에 비해 성능이 약간 느리다.



<strong>Stack</strong>

Vector클래스를 상속받고 있고, 모두 Vector에 있는 메소드를 이용하여 구현되고 있어 크게 다를 바가 없다.

---

## 5. <span style="background-color:lightsteelblue">Queue</span> 

선형 자료구조로 주로 순서가 있는 데이터를 기반으로 FIFO를 위해 만들어진 인터페이스이다.

Queue를 상속하고 있는 Deque이라는 Interface도 있다.

둘 다 같은 부류인데 Queue는 한쪽 방향으로만(단방향) 삽입, 삭제가 가능한 반면, Deque은 Double ended Queue라는 의미로 양쪽 삽입, 삭제가 가능하다. Node(first)와 Node(last)에서 접근 가능한 양방향 큐이다.



<span style="background-color:lightgreen"><strong>Queue/Deque Interface Operations</strong></span>

<img src="https://github.com/hyunwlee-dev/TIL/blob/f5830abf135db003fb5e2b4abd254cd08b761660/images/dataStructure/queueInterface.png?raw=true" style="zoom:45%;" />



<span style="background-color:lightgreen"><strong>Queue/Deque Interface를 구현하는 클래스</strong></span>

1. LinkedList
2. ArrayDeque
3. PriorityQueue



LinkedList__

LinkedList는 List를 구현하기도 하지만, Deque도 구현한다. 그리고 Deque Interface는 Queue Interface를 상속받는다.

즉, LinkedList는 3가지 용도로 쓰일 수 있다.

1. List
2. Deque
3. Queue

<strong>원리 자체가 크게 다르지 않기 때문에 linkedList 하나에 다중 인터페이스를 포함하고 있는 것이다.</strong>



<strong>ArrayDeque</strong>

반대로 ArrayList처럼 Object[] 배열로 구현되어 있는 것은 ArrayDeque이다. 



<strong>priorityQueue</strong>

데이터 우선순위에 기반하여 우선순위가 높은 데이터가 먼저 나온다.

따로 정렬방식을 지정하지 않는다면 낮은 숫자가 높은 우선순위를 갖는다. (Wrapper 클래스 객체는 Number을 상속받고 있기 때문에 내부적으로 Arrays.sort() 함수에 의해 낮은순서로 정렬된다.)

데이터들 중 최댓값 순으로 혹은, 최솟값 순을 꺼내 매우 유용하게 사용될 수 있다.

다만, 사용자가 정의한 객체를 타입으로 쓸 경우 반드시 Compartor 또는 Comparable을 통해 정렬 방식을 구현해주어야 한다.

---

## 6. <span style="background-color:lightsteelblue">Set</span>

1. 데이터를 중복해서 저장할 수 없음
2. 입력 순서대로의 저장 순서를 보장하지 않는다.

다만 LinkedHashSet은 입력 순서대로의 저장순서를 보장하고 있다.



기본적으로 List계열은 index(Node)로 관리하기 때문에 add()같은 메서드를 쓰면 순서대로 저장되어있다.

Queue계열 또한 PriorityQueue를 제외하고는 기본적으로 입력한 순서대로 객체가 연결되어있다.

하지만 Set의 경우는 일반적으로 입력받은 순서와 상관없이 데이터를 집합시키기 때문에 입력받은 순서를 보장할 수 없다.



물론 순서 보장이 안된다는 불편함을 개선하기 위해 만들어져 있는 것이 LinkedHashSet이다.

만약 데이터 중복을 허용하고 싶지 않은데 입력 순서를 보장받고 싶다면 LinkedHashSet을 사용면 된다.



그리고 Queue와 유사하게 Set을 상속받고 있는 SortedSet Interface도 있다.

<span style="background-color:lightgreen"><strong>Set Interface Operation</strong></span>

<img src="https://github.com/hyunwlee-dev/TIL/blob/bb294321373aeab6aea7b6f78386c1f5dd1a2861/images/dataStructure/setInterface.png?raw=true" style="zoom:45%;" />



<span style="background-color:lightgreen"><strong>Set Interface를 구현하는 클래스</strong></span>

1. HashSet
2. LinkedHashSet
3. TreeSet



<strong>HashSet</strong>

hash에 의해 데이터의 위치를 특정시켜 해당 데이터를 빠르게 색인(serach)할 수 있게 만든 것이다.

즉, Hash기능과 Set컬렉션이 합쳐진 것이 HashSet이다.

 그렇기 때문에 삽입, 삭제, 색인이 매우 빠른 컬렉션 중 하나다.



<strong>LinkedHashSet</strong>

add() 메서드를 통해 요소들을 넣은 순서대로 연결한다.

즉, LinkedList의 첫번째 요소부터 차례대로 출력하면 입력했던 순서대로 출력된다.

ex) 페이지를 열 때 만약 해당 페이지가 중복되는 경우 cache는 다시 적재할 필요는 없지만, 새로운 페이지를 할당해야할 경우 최근에 사용되지 않은 cache를 비우고자 할 때, 가장 오래된 cache를 비우는 것이 현명할 것이다.

이를 LRU(Least Recently Used Algorithm)이라고 하는데, 가장 오래된 cache를 비우는 것이 현명할 것이다.

(현실적으로는 LinkedHashMap을 씀, 이해를 돕기위해 사용)



<strong>TreeSet</strong>

Set Interface를 상속받은 <strong>SortedSet Interface</strong>를 구현하고 있다.

<strong>가중치에 따른 순서대로 정렬</strong>되어 보장한다는 것이다.

중복되지 않으면서 특정 규칙에 의해 정렬된 형태의 집합을 쓰고 싶을 때 쓴다.

정렬된 형태로 있다보니 특정 구간의 집합요소들을 탐색할 때 매우 유용하다.

---

## 7. <span style="background-color:lightsteelblue">적절한 자료구조 사용</span>



<strong>List</strong>

- ArrayList
- LinkedList
- Vector
- Stack

<strong>Queue</strong>

- Queue (LinkedList)
- PriorityQueue
- Deque (LinkedList)
- ArrayDeque

<strong>Set</strong>

- HashSet
- LinkedHashSet
- TreeSet

<img src="https://github.com/hyunwlee-dev/TIL/blob/4712a79eea7c70e5c0724a46b937ab29bc924608/images/dataStructure/CollectionFlowChart.png?raw=true"/>













