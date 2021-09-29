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

정렬은 되고 있으나 정렬에 필요한 명령어 수가 너무 많다. 따라서 명령어를 바로 출력하는 것이 아니라 문자열을 linkedList로 구현해 놓았던 deque에 삽입하기전 불필요한 명령어들을 없애기로 아이디어를 냈다.

```
void	optimize_deque(deque, input_s)
{
	if (deque->size != 0)
		if (strncmp(peek(), "sa", 4) == 0 그리고 strncmp(input_s, "sa") == 0) // 불필요한 경우 peek를 poll()하고 들어온 인자값을 삽입하지 않음
		else if (sa  -> sa)
		else if (sb  -> sb)
		else if (ss  -> ss)
		else if (pa  -> pb)
		else if (pb  -> pa)
		else if (ra  -> rb)
		else if (rb  -> ra)
		else if (ra  -> rra)
		else if (rra -> ra)
		else if (rb  -> rrb)
		else if (rrb -> rb)
		else if (rr -> rr)
		else if (rrr -> rrr)
		else
			offer();	// 불필요한 명령어가 아닐 경우 offer();
		poll();
	else
		offer();	// deque->size가 0이면 peek()할것이 없으므로 offer();
}
```

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

이렇게 코딩을 마쳤다.

하지만 `./push_swap $ARG | wc -l` 한 결과  아직도 명령어를 더 줄여야 했다.

답답한 마음에 동료 mchun에게 조언을 구하고 내 로직을 말했더니 pivot을 "적절히" 선택하지 않아서 그런것 같다고 말해주었다. 내일은 pivot 2곳을 linkedList에서 1/3, 2/3 지점으로 정할 것이 아니라 어떻게 설정 할것인지 깊게 생각해볼 것이다.