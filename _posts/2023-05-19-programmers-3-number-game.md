---
title: "[Summer/Winter Coding(~2018)] 숫자 게임 (C++)"
author: gh13
date: 2023-05-19 16:40:00 +0800
categories: [프로그래머스, Lv.3]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/12987#>
{: .prompt-info }

## 풀이

A와 B를 `오름차순으로 정렬`해준 후, 각각의 인덱스를 이동시켜가며 풀어주었다. 현재 B의 값이 A의 값보다 크다면 cnt를 증가시킴과 동시에 A와 B의 인덱스를 모두 증가시켜준다. 이외의 경우에는 `현재 A의 값보다 큰 B의 값을 찾아야 하기 때문에 B의 cnt만 증가`시켜주었다.

처음에는 내림차순으로 내려오는 방식으로 접근했었는데, 같은 값을 만났을 경우 해결하는 법이 잘 보이지 않았다. 벡터를 정렬했기 때문에 현재 A와 B의 값이 같다면, 그보다 작은 인덱스에서의 B의 값들은 반드시 A의 값보다 작아지기 때문이었던 것같다.

## 코드

```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int solution(vector<int> A, vector<int> B) {
    sort(A.begin(), A.end());
    sort(B.begin(), B.end());
    
    int cnt = 0;
    int a_idx = 0, b_idx = 0;
    while(a_idx < A.size() && b_idx < B.size()){
        if(A[a_idx] < B[b_idx])
            cnt++,  a_idx++,    b_idx++;
        else        b_idx++;
    }
    return cnt;
}
```
