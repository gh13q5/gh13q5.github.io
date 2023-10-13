---
title: "[월간 코드 챌린지 시즌2] 약수의 개수와 덧셈 (C++)"
author: gh13
date: 2023-05-03 16:05:00 +0800
categories: [프로그래머스, Lv.1]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/77884>
{: .prompt-info }

## 풀이

left와 right를 범위로 하는 반복문에서 약수의 개수를 구해준다.

## 코드

```cpp
#include <string>
#include <vector>

using namespace std;

int solution(int left, int right) {
    int sum = 0;
    for(int i = left; i <= right; i++){
        int cnt = 0;
        for(int j = 1; j <= i / 2; j++)
            if(i % j == 0)  cnt++;
        (cnt + 1) % 2 == 0 ? (sum += i) : (sum -= i);       // 자기 자신도 약수에 포함하기 때문에 cnt + 1
    }
    return sum;
}
```
