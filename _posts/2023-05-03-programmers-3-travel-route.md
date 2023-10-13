---
title: "여행경로 (C++)"
author: gh13
date: 2023-05-03 17:37:00 +0800
categories: [프로그래머스, Lv.3]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/43164>
{: .prompt-info }

## 풀이

무사히 끝까지 탐색할 수 있는 경로 하나를 찾아야 했기 때문에 `DFS`를 이용했다. 내가 작성한 코드의 경우에는 전역 변수 is_finish를 이용하는 것이 중요했다. `경로 탐색을 완전히 끝마치지 못 했을 때, 그 동안 탐색한 경로를 모두 없애`주는 식으로 초기화시키고 다른 경로를 탐색하는 방식으로 진행해서 그 중요성이 유독 두드러진 것같다.

## 코드

```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

vector<string> answer;
bool visited[100001];
bool is_finish = false;     // 경로 탐색이 끝났는지 확인

void dfs(string start, vector<vector<string>> &tickets){
    if(answer.size() == tickets.size()) is_finish = true;
    
    answer.push_back(start);
    for(int i = 0; i < tickets.size(); i++){
        if(visited[i])  continue;
        if(tickets[i][0] == start){
            visited[i] = true;
            dfs(tickets[i][1], tickets);
            if(!is_finish){
                answer.pop_back();
                visited[i] = false;
            }
        }
    }
}

vector<string> solution(vector<vector<string>> tickets) { 
    sort(tickets.begin(), tickets.end());       // 오름차순 정렬
    dfs("ICN", tickets);
    return answer;
}
```
