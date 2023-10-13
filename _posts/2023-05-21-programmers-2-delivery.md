---
title: "[Summer/Winter Coding(~2018)] 배달 (C++)"
author: gh13
date: 2023-05-21 15:31:00 +0800
categories: [프로그래머스, Lv.2]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/12978>
{: .prompt-info }

## 풀이

`다익스트라 알고리즘`을 이용해 1번 마을과 다른 마을 간의 최소거리를 구해서 풀었다.  

우선순위큐에서 매번 모든 road를 순회하며 값을 찾게 되면 시간이 오래 걸릴 것같아, 이중 벡터를 이용해 각 정점에서 갈 수 있는 마을과 거리 값을 별도로 저장해주었다.  

queue를 쓰다보니 `BFS와 다익스트라의 차이점`이 궁금해졌었는데 나중에 문제 푸는 데 도움될 것같아 메모해두기로! BFS는 목적지 한 곳이 뚜렷해서 그 이외의 정점까지의 거리 정보가 필요 없을 때, 다익스트라는 목적지가 뚜렷하지 않지만 대신 모든 정점까지의 최소 거리 정보가 필요할 때 사용하게 된다. 이 때, BFS는 모든 정점까지의 거리 정보가 담긴 배열(아래 코드에서는 dist)이 필요 없고, 대신 visit 처리를 하는 배열이 필요하게 된다. 다익스트라는 dist가 필요한 대신, visit 배열이 없어도 괜찮다.  

## 코드

```cpp
#include <vector>
#include <queue>

using namespace std;

int solution(int N, vector<vector<int> > road, int K) {
    vector<int> dist(N, 500001);
    dist[0] = 0;
    priority_queue<pair<int, int>> pq;      // pair<노드, 거리>
    pq.push(make_pair(0, 0)); 
    
    vector<vector<pair<int, int>>> path(N);         // 각 정점에서 갈 수 있는 마을 모음
    for(int i = 0; i < road.size(); i++){
        path[road[i][0] - 1].push_back(make_pair(road[i][1] - 1, road[i][2]));
        path[road[i][1] - 1].push_back(make_pair(road[i][0] - 1, road[i][2]));
    }
    
    while(!pq.empty()){
        int cur_v = pq.top().first;
        int cur_c = pq.top().second;
        pq.pop();
        for(auto p : path[cur_v]){
            int next_v = p.first;
            int next_c = cur_c + p.second;
            if(next_c < dist[next_v]){
                dist[next_v] = next_c;
                pq.push(make_pair(next_v, next_c));
            }
        }
    }
    
    int cnt = 0;
    for(int i = 0; i < dist.size(); i++)        // K보다 작거나 같은 거리 세기
        if(dist[i] <= K)    cnt++;
    return cnt;
}
```
