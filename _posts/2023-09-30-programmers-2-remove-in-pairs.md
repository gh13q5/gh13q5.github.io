---
title: "[2017 팁스타운] 짝지어 제거하기 (C++)"
author: gh13
date: 2023-09-30 16:15:00 +0800
categories: [프로그래머스, Lv.2]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/12973#>
{: .prompt-info }

## 풀이

`stack`의 top이 현재 문자와 같다면 pop해주는 방식으로 풀어주었다. stack이 비어있으면 전부 제거가 가능하다고 보고 1을 반환했다. stack 문제로 유명한 괄호 문제와 비슷한 문제인 듯 싶다.  


## 코드

```cpp
#include <string>
#include <stack>

using namespace std;

int solution(string s){
    stack<int> st;
    st.push(s[0]);
    for(int i = 1; i < s.length(); i++){
        if(!st.empty() && st.top() == s[i])   st.pop();
        else    st.push(s[i]);
    }
    
    if(st.empty())
        return 1;
    return 0;
}
```
