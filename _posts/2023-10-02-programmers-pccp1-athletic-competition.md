---
title: "[PCCP 모의고사 1회] 체육대회 (C++)"
author: gh13
date: 2023-10-02 15:25:00 +0800
categories: [프로그래머스, PCCP 모의고사]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/15008/lessons/121684>
{: .prompt-info }

## 풀이

주어진 배열의 길이가 10 이하로 작길래 `DFS`를 이용해 모든 조합, 즉 모든 경우의 수를 살펴주었다.  

## 코드

```cpp
#include <string>
#include <vector>

using namespace std;

bool visited[10] = {false, };
int max_sum = -1;

void dfs(int sum, int idx, const vector<vector<int>> ability){
    if(idx >= ability[0].size()){
        if(sum > max_sum)   max_sum = sum;
        return;
    }
    
    for(int i = 0; i < ability.size(); i++){
        if(visited[i])  continue;
        visited[i] = true;
        dfs(sum + ability[i][idx], idx + 1, ability);
        visited[i] = false;
    }
}

int solution(vector<vector<int>> ability) {
    dfs(0, 0, ability);
    return max_sum;
}
```
