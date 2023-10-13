---
title: "[월간 코드 챌린지 시즌3] n^2 배열 자르기 (C++)"
author: gh13
date: 2023-05-18 15:56:00 +0800
categories: [프로그래머스, Lv.2]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/87390>
{: .prompt-info }

## 풀이

`(행, 열)에서 더 큰 값이 해당 칸의 값`이 된다. (1, 1)에는 1이, (1, 2)에는 2가, (1, 3)에는 3이 들어가는 형식이다.

다시 풀어볼 때는 left와 right는 long long으로 주어지지만 `right-left가 10의 5승보다 작다`는 점에서 힌트를 얻어, for문의 구간을 int를 이용해 작성할 수 있었다. 그리고 Effective C++을 공부하는 도중 #define과 같은 선행 처리자는 멀리하는 것이 좋다는 것을 알았기 때문에, 대신 algorithm 라이브러리의 max 함수를 사용해주었다.

## 코드

```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

vector<int> solution(int n, long long left, long long right) {
    vector<int> answer;    
    for(int i = 0; i <= right - left; i++){
        int max_n = max((left + i) / n + 1, (left + i) % n + 1);
        answer.push_back(max_n);
    }
    return answer;
}
```
