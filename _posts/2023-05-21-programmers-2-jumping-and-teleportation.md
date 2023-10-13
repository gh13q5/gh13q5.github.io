---
title: "[Summer/Winter Coding(~2018)] 점프와 순간 이동 (C++)"
author: gh13
date: 2023-05-21 14:18:00 +0800
categories: [프로그래머스, Lv.2]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/12980>
{: .prompt-info }

## 풀이

n을 0까지 줄여나가는 방식으로 접근했다. n이 짝수면 순간이동한 것으로 여겨 2로 나눠주고, 홀수면 점프한 것으로 여겨 1을 빼주었다.

다른 사람의 풀이를 보면 의외로 bit를 이용해 푸는 사람들이 많았다! 한 번 참고해봐야겠다.

## 코드

```cpp
using namespace std;

int solution(int n){
    int cnt = 0;
    while(n > 0){
        if(n % 2 == 0)  n /= 2;
        else    n--,    cnt++;
    }
    return cnt;
}
```
