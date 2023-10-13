---
title: "[월간 코드 챌린지 시즌1] 3진법 뒤집기 (C++)"
author: gh13
date: 2023-05-03 17:07:00 +0800
categories: [프로그래머스, Lv.1]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/68935>
{: .prompt-info }

## 풀이

2진법을 구하는 식에 3을 대입해 풀어줬다.

## 코드

```cpp
#include <string>
#include <vector>
#include <cmath>

using namespace std;

int solution(int n) {
    string trit = "";
    while(n > 0)
        trit += to_string(n % 3),    n /= 3;
    
    int answer = 0;
    for(int i = trit.length() - 1; i >= 0; i--)
        answer += ((trit[i] - '0') * pow(3, trit.length() - i - 1));
    return answer;
}
```
