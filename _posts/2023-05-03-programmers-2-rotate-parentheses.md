---
title: "[월간 코드 챌린지 시즌2] 괄호 회전하기 (C++)"
author: gh13
date: 2023-05-03 16:07:00 +0800
categories: [프로그래머스, Lv.2]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/76502>
{: .prompt-info }

## 풀이

`Stack`을 이용해 올바른 괄호를 찾아낸 후, string 함수를 이용해 문자열을 이동시켜줬다.

## 코드

```cpp
#include <string>
#include <vector>
#include <stack>

using namespace std;

int solution(string s) {
    int cnt = 0;
    for(int i = 0; i < s.length(); i++){
        bool isRight = true;
        stack<char> st;
        for(char c : s){
            if(c == ')'){
                if(!st.empty() && st.top() == '(')  st.pop();
                else{   isRight = false;    break;  }
            }
            else if(c == ']'){
                if(!st.empty() && st.top() == '[')  st.pop();
                else{   isRight = false;    break;  }
            }
            else if(c == '}'){
                if(!st.empty() && st.top() == '{')  st.pop();
                else{   isRight = false;    break;  }
            }
            else    st.push(c);
        }
        if(isRight && st.empty())  cnt++;
        s += s[0];      s.erase(0, 1);
    }
    
    return cnt;
}
```
