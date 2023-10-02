---
title: "[PCCP 모의고사 2회] 카페 확장 (C++)"
author: gh13
date: 2023-10-02 14:24:00 +0800
categories: [프로그래머스, PCCP 모의고사]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/15009/lessons/121689>
{: .prompt-info }

## 풀이

`deque`를 이용한 손님 대기열을 만들어, `deque의 크기가 가장 클 때를 가게에서 가장 많은 손님이 대기할 때`라고 상정하고 풀어주었다. deque에는 해당 손님이 주문한 음료를 받고 나가는 시간이 담겨 있고, 규칙은 다음과 같다.  

한 손님이 가장 오래 기다리고 있는 손님(deque의 맨앞)이 나가기 전에 들어온다면, deque에 기다리는 손님이 나가는 시간과 현재 들어온 손님이 음료를 기다리는 시간을 더한 값을 넣어준다. 만약 가장 오래 기다리고 있는 손님이 나간 후에 들어온 손님이라면, 가장 최근에 들어온 손님이 나가는 시간과 비교한 후 deque에 값을 넣어준다. 이 때 가장 오래 기다린 손님은 deque에서 제거한다.  

## 코드

```cpp
#include <string>
#include <vector>
#include <queue>

using namespace std;

int solution(vector<int> menu, vector<int> order, int k) {
    deque<int> ready_q;
    ready_q.push_back(menu[order[0]]);
    
    int max = 1,   time = ready_q.front();
    for(int i = 1; i < order.size(); i++){
        if(i * k < ready_q.front())   time += menu[order[i]];
        else{
            time = (i * k < ready_q.back()) ? (ready_q.back() + menu[order[i]]) : (i * k + menu[order[i]]);
            ready_q.pop_front();
        }
        ready_q.push_back(time);
        if(ready_q.size() > max)    max = ready_q.size();
    }
    return max;
}
```
