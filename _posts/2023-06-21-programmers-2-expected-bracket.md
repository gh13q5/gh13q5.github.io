---
title: "[2017 팁스타운] 예상 대진표 (C++)"
author: gh13
date: 2023-06-21 16:52:00 +0800
categories: [프로그래머스, Lv.2]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/12985>
{: .prompt-info }

## 풀이

`a와 b가 홀수와 짝수로 연속된 수가 될 때까지` a와 b를 2로 계속 나눠주었다. 이 때 a가 b보다 더 큰 수일수도 있으므로, a와 b의 차이를 구할 때는 절대값 함수를 이용했다.  


## 코드

```cpp
#include <cmath>

using namespace std;

int solution(int n, int a, int b){
    int cnt = 1;
    while(max(a, b) % 2 != 0 || abs(b - a) != 1){
        a = ceil(static_cast<double>(a) / 2);
        b = ceil(static_cast<double>(b) / 2);
        cnt++;
    }
    return cnt;
}
```
