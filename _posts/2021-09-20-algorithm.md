---
layout: post
title: "42seoul(push_swap)"
author: "hyunwlee"
---

## <span style="background-color:#E81E25; color:white">42seoul(push_swap)</span>

#### <span style="background-color:#FE7773; color:white">동료 학습 방법</span>

mchun

#### <span style="background-color:#FE7773; color:white">학습 목표</span>

최적화

5. <span style="background-color:#FFC2C3; color:white"><strong>Middle version</strong></span>

ARG="<span style="color:blue">100 random</span>"; ./push_swap $ARG | ./checker_Mac $ARG 

<span style="color:red">result line: </span>

- [ ] 700 미만: 5점

- [ ] 900 미만: 4점
- [ ] 1100 미만: 3점
- [x] 1300 미만: 2점
- [ ] 1500 미만: 1점

6. <strong><span style="background-color:#FFC2C3; color:white"><strong>Advance version</strong></span></strong>

ARG="<span style="color:blue">500 random</span>"; ./push_swap $ARG | ./checker_Mac $ARG 

<span style="color:red">result line: </span>

- [ ] 5500 미만: 5점
- [ ] 7000 미만: 4점
- [ ] 8500 미만: 3점
- [x] 10000 미만: 2점
- [ ] 11500 미만: 1점

---

## <span style="background-color:#E81E25; color:white">명령어 수를 줄이기 위한 최적화 아이디어</span>

<span style="background-color:#FE7773; color:white"><strong>명령어 수를 줄이기 위한 최적화 아이디어 [1]</strong></span>

정렬은 되고 있으나 정렬에 필요한 명령어 수가 너무 많다. 따라서 명령어를 바로 출력하는 것이 아니라 문자열을 linkedList로 구현해 놓았던 deque에 삽입하기전 <strong>불필요한 명령어들을 없애기</strong>로 아이디어를 냈다.

<script src="https://gist.github.com/hyunwlee-dev/8bcf0e7a88484de5d3c5cdef6b08cd01.js"></script>

이에 추가적으로 필요한 함수들

```
void        init_deques_s(t_deque_s *s_deque);
t_node_s    *creat_node_s(t_node_s *l_link, t_node_s *r_link, char *s, int idx);
void        add_rear_s(t_deque_s *deque, char *s);
char        *delete_rear_s(t_deque_s *deque);	// stack처럼 최근 것을 peek()하여 다음들어오는 인자와 비교하기 위해서 rear에서 뽑아낸다.
char        *get_front_s(t_deque_s *deque);
void        clear_s(t_deque_s *deque);
void        print_all_s(t_deque_s *deque);
```

코딩을 마쳤다.  

Node 구조체에 필요한 data의 타입이 달라져 (int -> char *) 기능적으로 중복인 함수들을 만들게 되었다. 

내 코드 되게 별로다. 

자바였으면 제네릭스를 사용해서 들어오는 요소의 데이터 타입을 이용하여 짰을테고..

사실 c언어의 경우 (void *)주소를 이용하여 다양한 데이터 타입을 받도록 하는게 맞지만,, 처음에 ADT와 DS를 설계를 안하고 무지성으로 짜게 되어 유지보수하는데에 더 시간이 들 것같아 기능적으로 중복적인 함수를 만들기로 결정하였다. 반성한다..



그럼 결과는 이제 잘 나오나?

아니다. 불필요한 명령어를 줄여도 cutLine에 한 참 부족했다.

<span style="background-color:#FE7773; color:white"><strong>명령어 수를 줄이기 위한 최적화 아이디어 [2]</strong></span>

답답한 마음에 동료 mchun에게 조언을 구하고 내 로직을 말했더니 <strong>pivot을 "적절히" 선택</strong>하지 않아서 그런것 같다고 말해주었다. 내일은 pivot 2곳을 linkedList에서 1/3, 2/3 지점으로 정할 것이 아니라 어떻게 설정 할것인지 깊게 생각해볼 것이다.

