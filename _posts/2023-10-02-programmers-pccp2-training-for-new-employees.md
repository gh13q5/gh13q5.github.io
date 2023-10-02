---
title: "[PCCP 모의고사 2회] 신입사원 교육 (C++)"
author: gh13
date: 2023-10-02 14:26:00 +0800
categories: [프로그래머스, PCCP 모의고사]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/15009/lessons/121688>
{: .prompt-info }

## 풀이

신입 사원들의 능력치를 오름차순으로 표현한 `우선순위 큐`를 이용했다. 가장 앞에 있는 2명의 능력치를 더하고 다시 우선순위 큐에 넣어주는 코드를 number만큼 반복한 후, 우선순위 큐에 있는 모든 능력치의 합을 반환해 풀 수 있었다.

## 코드

```cpp
#include <string>
#include <vector>
#include <queue>

using namespace std;

int solution(vector<int> ability, int number) {
    priority_queue<int, vector<int>, greater<int>> pq(ability.begin(), ability.end()); 
    for(int i = 0; i < number; i++){
        int sum = 0;
        for(int j = 0; j < 2; j++)
            sum += pq.top(),    pq.pop();
        pq.push(sum),   pq.push(sum);
    }
    
    int answer = 0;
    while(!pq.empty()){
        answer += pq.top();
        pq.pop();
    }
    return answer;
}
```
