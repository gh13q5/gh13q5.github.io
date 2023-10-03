---
title: "[2021 Dev-Matching] 로또의 최고 순위와 최저 순위 (C++)"
author: gh13
date: 2023-07-10 19:27:00 +0800
categories: [프로그래머스, Lv.1]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/77484>
{: .prompt-info }

## 풀이

`일치하는 번호의 수`와 `알아볼 수 없는 번호(0)의 수`를 세어 당첨 순위를 계산해주었다.  


## 코드

```cpp
#include <string>
#include <vector>

using namespace std;

vector<int> solution(vector<int> lottos, vector<int> win_nums) {
    int cnt = 0,    zero_cnt = 0;
    for(int i = 0; i < lottos.size(); i++){
        if(lottos[i] == 0){
            zero_cnt++;
            continue;
        }
        for(int j = 0; j < win_nums.size(); j++)
            if(lottos[i] == win_nums[j])    cnt++;
    }
    
    int best = (cnt + zero_cnt > 1) ? 7 - (cnt + zero_cnt) : 6;
    int worst = (cnt > 1) ? 7 - cnt : 6;
    return {best, worst};
}
```
