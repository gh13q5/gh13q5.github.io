---
title: "[PCCP 모의고사 2회] 보물지도 (C++)"
author: gh13
date: 2023-06-19 19:37:00 +0800
categories: [프로그래머스, PCCP 모의고사]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/15009/lessons/121690#>
{: .prompt-info }

## 풀이

`BFS` 방식으로 풀어주되, 칸 방문 여부와 함정 유무를 `3중 배열`을 이용해 표시했다. 같은 칸이어도 점프를 사용한 채 방문했을 때와 사용하지 않고 방문했을 때 차이가 발생해서, `map[x][y][jump여부]`로 정의해주고 방문한 곳이나 함정은 true로 체크해주었다. BFS로 길을 찾을 때 좌표와 점프여부를 담은 queue를 이용해, 역시 걸어가는 방식과 점프해서 넘어가는 방식을 모두 고려했다.  

평소 BFS를 사용할 때처럼 map의 크기를 넘어서면 continue로 넘어가는 방식을 적용했더니 안 돌아가는 케이스가 꽤 많았다. 이렇게 되면 걸어가는 방식만 확인하고 점프해서 가는 방식은 패스해버리는 경우가 생겨버려 예외가 발생하는 거였다. 아래는 이 예외를 처리하기 위해 이용한 테스트 케이스다.  

> n = 1, m = 3, hole [[1, 2]]   ->   answer = 1
{: .prompt-tip }

걸어서 방문했을 때와 점프해서 방문했을 때의 코드가 거의 비슷해서 함수로 뺄까 싶었지만, BFS로 큐를 사용하는 와중에 스택까지 사용해 여러번 왔다갔다 하는게 과연 효율적일까 싶어 그만뒀다. 가독성이나 결합도 관련으론 훨씬 좋아보여서 여전히 고민하곤 있지만ㅋㅋㅠㅠ  더 잘 쓸 수 있는 방법이 없나 생각해봐야지.  

## 코드

```cpp
#include <string>
#include <vector>
#include <queue>

using namespace std;

int dx[4] = {0, 1, 0, -1};     // up - right - down - left
int dy[4] = {1, 0, -1, 0};

int solution(int n, int m, vector<vector<int>> hole) {
    bool map[1001][1001][2] = {false, };       // [x][y][is jump?]  -> true: visited and hole
    for(int i = 0; i < hole.size(); i++){
        map[hole[i][0]][hole[i][1]][0] = true;
        map[hole[i][0]][hole[i][1]][1] = true;
    }
    
    int distance = 1;
    queue<pair<pair<int,int>, bool>> route_q;   // pair<좌표, 점프여부>
    route_q.push({{1, 1}, false});
    
    while(!route_q.empty()){
        int q_size = route_q.size();
        for(int i = 0; i < q_size; i++){
            pair<int, int> cur = route_q.front().first;
            bool is_jumped = route_q.front().second;
            route_q.pop();

            for(int j = 0; j < 4; j++){
                pair<int, int> next = {cur.first + dx[j], cur.second + dy[j]};
                if(next.first >= 1 && next.first <= n && next.second >= 1 && next.second <= m && !map[next.first][next.second][is_jumped]){
                    if(next.first == n && next.second == m)   return distance;
                    route_q.push({next, is_jumped});
                    map[next.first][next.second][is_jumped] = true;
                }
                
                if(!is_jumped){
                    next = {next.first + dx[j], next.second + dy[j]};
                    if(next.first >= 1 && next.first <= n && next.second >= 1 && next.second <= m && !map[next.first][next.second][1]){
                        if(next.first == n && next.second == m)   return distance;
                        route_q.push({next, true});
                        map[next.first][next.second][1] = true;
                    }
                }
            }
        }
        distance++;
    }
    
    return -1;
}
```
