---
title: "[PCCP 모의고사 1회] 운영체제 (C++)"
author: gh13
date: 2023-10-02 15:12:00 +0800
categories: [프로그래머스, PCCP 모의고사]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/15008/lessons/121686>
{: .prompt-info }

## 풀이

program을 `호출시간-우선순위-실행시간 순으로 정렬`한 후, 반복문으로 돌면서 `현재 시간(time 변수)에 실행 가능한 프로그램을 우선순위 큐에` 담았다. 우선순위 큐의 가장 앞에 오는 프로그램이 현재 시간에서 실행되기까지 대기한 시간을 answer 벡터에 더해주고, 실행 시간도 현재 시간에 더해주기를 반복했다.  

이 때, 우선순위 큐에 아무런 프로그램도 들어가 있지 않다면, 현재 시간에 작업할 수 있는 작업이 없다는 것으로 판단하고 다음 작업이 호출되는 시간까지 현재 시간을 건너뛰어주었다. 또한, 우선순위 큐에 들어온 프로그램 사이에서는 배열의 첫 번째 요소인 '우선순위'가 실행 조건의 1순위가 되기 때문에 별도의 cmp 구조체를 이용하지 않고 greater<>만 이용해주었다. 이전에 풀었던 프로그래머스의 [디스크 컨트롤러](https://school.programmers.co.kr/learn/courses/30/lessons/42627)와 유사해서 많은 참고가 가능했다!  

## 코드

```cpp
#include <string>
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

bool cmp(vector<int> a, vector<int> b){
	if(a[1] == b[1]){
        if(a[0] == b[0])    return a[2] < b[2];
            return a[0] < b[0];
    }
    return a[1] < b[1];
}

vector<long long> solution(vector<vector<int>> program) {
    sort(program.begin(), program.end(), cmp);      // cmp 기준 : 호출시간 - 우선순위 - 실행시간
    
    int idx = 0,    time = 0;
    vector<long long> answer(11, 0);
    priority_queue<vector<int>, vector<vector<int>>, greater<vector<int>>> ready_pq;        // 대기 큐
    
    while(idx < program.size() || !ready_pq.empty()){
        while((idx < program.size()) && (program[idx][1] <= time)){
            ready_pq.push({program[idx][0], program[idx][1], program[idx][2]});
            idx++;
        }
        
        if(ready_pq.empty()){   // 전체 작업이 아직 안 끝났는데, 당장 작업할 수 있는 게 없을 때
            time = program[idx][1];
            continue;
        }
        
        answer[ready_pq.top()[0]] += (time - ready_pq.top()[1]);
        time += ready_pq.top()[2];
        ready_pq.pop();
    }
    answer[0] = time;
    return answer;
}
```
