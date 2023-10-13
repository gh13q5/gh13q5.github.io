---
title: "[월간 코드 챌린지 시즌1] 내적 (C++)"
author: gh13
date: 2023-05-18 15:19:00 +0800
categories: [프로그래머스, Lv.1]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/70128>
{: .prompt-info }

## 새로 푼 코드 (2023.04.25)

```cpp
#include <string>
#include <vector>

using namespace std;

int solution(vector<int> a, vector<int> b) {
    int answer = 0;
    for(int i = 0; i < a.size(); i++)
        answer += (a[i] * b[i]);
    return answer;
}
```

## 이전 코드 (2022.10)
```cpp
#include <string>
#include <vector>

using namespace std;

int solution(vector<int> a, vector<int> b) {
    int answer = 0;
    int length = a.size();
    
    for(int i = 0; i < (length / 2); i++){
        answer += (a[i] * b[i]);
        answer += (a[length - i - 1] * b[length - i - 1]);
    }
    
    if(length % 2 != 0){
        int mid = (length / 2);
        answer += (a[mid] * b[mid]);
    }
    
    return answer;
}
```
