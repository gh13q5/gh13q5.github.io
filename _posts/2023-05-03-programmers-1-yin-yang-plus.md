---
title: "[월간 코드 챌린지 시즌2] 음양 더하기 (C++)"
author: gh13
date: 2023-05-03 16:02:00 +0800
categories: [프로그래머스, Lv.1]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/76501>
{: .prompt-info }

## 풀이

`삼항 연산자`를 이용해, true면 더하고 false면 빼주었다.

## 코드

```cpp
#include <string>
#include <vector>

using namespace std;

int solution(vector<int> absolutes, vector<bool> signs) {
    int sum = 0;
    for(int i = 0; i < absolutes.size(); i++)
        signs[i] ? (sum += absolutes[i]) : (sum -= absolutes[i]);
    return sum;
}
```
