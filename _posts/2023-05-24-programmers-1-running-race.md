---
title: "달리기 경주 (C++)"
author: gh13
date: 2023-05-24 18:10:00 +0800
categories: [프로그래머스, Lv.1]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/178871>
{: .prompt-info }

## 풀이

각 플레이어의 이름을 key로, 현재 순위를 value로 하는 `unordered_map`을 이용해 풀어주었다. `map을 이용해 이름이 불린 선수의 현재 순위를 가져오고,` 이를 이용해 순위 순으로 정렬된 선수 목록 vector에도 바로 접근하는 방식이다.


## 코드

```cpp
#include <string>
#include <vector>
#include <unordered_map>

using namespace std;

vector<string> solution(vector<string> players, vector<string> callings) {
    unordered_map<string, int> rank;     // map<이름, 순위>
    for(int i = 0; i < players.size(); i++)
        rank.insert({players[i], i + 1});
    
    vector<string> answer = players;
    for(string c : callings){
        int cur = rank[c] - 1;
        swap(answer[cur], answer[cur - 1]);
        rank[c] -= 1;
        rank[answer[cur]] += 1;
    }
    return answer;
}
```
