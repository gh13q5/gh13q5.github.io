---
title: "[Summer/Winter Coding(~2018)] 방문 길이 (C++)"
author: gh13
date: 2023-05-18 15:32:00 +0800
categories: [프로그래머스, Lv.2]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/49994>
{: .prompt-info }

## 풀이

`현재 간선을 방문했는지에 대한 set`을 만들어준 후, 해당 `(set의 크기 / 2)`를 반환하는 방식을 이용했다. set에 현재 정점->다음 정점, 다음 정점->현재 정점, 양방향 모두 고려해서 넣었기 때문에 나누기 2를 해주었다. 간선의 방문 여부를 담는 visited 변수는 이전에 풀었던 [방의 개수](https://school.programmers.co.kr/learn/courses/30/lessons/49190) 문제를 참고했다.

푸는 도중에 if문을 쓸까 switch문을 쓸까 고민을 조금 하긴 했는데...... char를 비교하는 구문이라 if문보다는 switch가 좀 더 가독성이 좋을 것같아서 switch문을 이용했다. 

## 코드

```cpp
#include <string>
#include <set>

using namespace std;

int solution(string dirs) {
    pair<int, int> cur(0, 0);
    set<pair<pair<int, int>, pair<int, int>>> visited;
    for(auto d : dirs){
        pair<int, int> next = cur;
        switch(d){
            case 'U':
                next.second++;
                break;
            case 'D':
                next.second--;
                break;
            case 'L':
                next.first--;
                break;
            case 'R':
                next.first++;
                break;
        }
        if(next.first > 5 || next.first < -5 || next.second > 5 || next.second < -5)    continue;
        visited.insert(make_pair(cur, next));
        visited.insert(make_pair(next, cur));
        cur = next;
    }
    return visited.size() / 2;
}
```
