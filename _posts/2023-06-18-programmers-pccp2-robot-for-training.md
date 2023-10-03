---
title: "[PCCP 모의고사 2회] 실습용 로봇 (C++)"
author: gh13
date: 2023-06-18 16:05:00 +0800
categories: [프로그래머스, PCCP 모의고사]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/15009/lessons/121687>
{: .prompt-info }

## 풀이

`현재 어느 방향으로 가고 있는지를 표현하는 state 변수`를 이용해 풀어주었다. state를 char로 선언할까 고민했지만, 시계 방향 및 반시계 방향으로 돌리는 걸 표현하기 위해 int로 선언하고 dx와 dy를 이용해 값을 증감하는 방식으로 진행했다. 좌표값을 반환해야 해서 pair를 쓸까 싶었지만, 반환값이 vector<int>라서 그냥 크기가 2인 vector를 만들어줬다.  

## 코드

```cpp
#include <string>
#include <vector>

using namespace std;

int dx[4] = {0, 1, 0, -1};  // U, R, D, L
int dy[4] = {1, 0, -1, 0};

vector<int> solution(string command) {
    vector<int> cur(2, 0);
    int state = 0;      // 0:U, 1:R, 2:D, 3:L
    for(char c : command){
        switch(c){
            case 'R':
                state == 3 ? state = 0 : state++;
                break;
            case 'L':
                state == 0 ? state = 3 : state--;
                break;
            case 'G':
                cur[0] += dx[state],    cur[1] += dy[state];
                break;
            case 'B':
                cur[0] -= dx[state],    cur[1] -= dy[state];
                break;
        }
    }
    return cur;
}
```
