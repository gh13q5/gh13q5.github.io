---
title: "[2021 Dev-Matching] 행렬 테두리 회전하기 (C++)"
author: gh13
date: 2023-09-30 11:35:00 +0800
categories: [프로그래머스, Lv.2]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/77485>
{: .prompt-info }

## 풀이

먼저 행렬을 만들어주고, `인덱스를 옮겨가며` 행렬의 테두리를 회전시켜주었다. 인덱스가 처음 회전을 시작한 위치(x1, x2)로 되돌아오면 반복을 끝내고 다음 query를 살펴보는 방식이다.  

처음에 현재 인덱스의 x와 y 모두 시작한 위치와 같을 때를 종료 조건으로 했었는데 대부분의 테스트 케이스에서 실패가 떴다. 출력해서 살펴보니 시작한 위치의 숫자가 바뀌어 있지 않고 있더라... 그래서 x의 값이 시작한 위치보다 작아지면 종료하도록 바꿔주었더니 해결됐다! 나는 왼쪽 테두리를 이동할 때 x의 값만 증가시켜주도록 코드를 작성해서 x의 값만 비교하는 게 시간적으로도 훨씬 이득이었다 휴;

<br/>

## 코드

```cpp
#include <string>
#include <vector>
#include <cmath>

using namespace std;

vector<int> solution(int rows, int columns, vector<vector<int>> queries) {
    vector<vector<int>> map(rows, vector<int>(columns, 0));
    for(int i = 0; i < rows; i++)
        for(int j = 0; j < columns; j++)
            map[i][j] = i * columns + j + 1;
    
    vector<int> answer;
    for(auto q : queries){
        pair<int, int> start = {q[0] - 1, q[1] - 1};
        pair<int, int> end = {q[2] - 1, q[3] - 1};
        
        int n = map[start.first][start.second];
        int min_n = n;
        int cur_x = start.first,   cur_y = start.second + 1;
        while(cur_x >= start.first){
            int tmp = map[cur_x][cur_y];
            map[cur_x][cur_y] = n;
            n = tmp;
            min_n = min(min_n, n);
            
            if(cur_y == start.second)   cur_x--;        // 회전
            else if(cur_x == end.first) cur_y--;
            else if(cur_y == end.second)    cur_x++;
            else    cur_y++;
        }
        answer.push_back(min_n);
    }
     
    return answer;
}
```
