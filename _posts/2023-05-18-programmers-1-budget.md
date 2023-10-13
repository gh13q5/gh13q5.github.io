---
title: "[Summer/Winter Coding(~2018)] 예산 (C++)"
author: gh13
date: 2023-05-18 15:23:00 +0800
categories: [프로그래머스, Lv.1]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/12982>
{: .prompt-info }

## 풀이

주어진 d를 `sort`해준 후, `작은 수부터 더해가며` 예산을 초과하면 반복문을 빠져나가는 방식으로 풀 수 있었다.

## 코드

```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int solution(vector<int> d, int budget) {
    sort(d.begin(), d.end());
    
    int sum = 0, cnt = 0;
    for(auto cur : d){
        sum += cur;
        if(sum > budget)   break;
        cnt++;
    }
    return cnt;
}
```
