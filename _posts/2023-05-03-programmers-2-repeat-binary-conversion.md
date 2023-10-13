---
title: "[월간 코드 챌린지 시즌1] 이진 변환 반복하기 (C++)"
author: gh13
date: 2023-05-03 17:12:00 +0800
categories: [프로그래머스, Lv.2]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/70129>
{: .prompt-info }

## 풀이

문제에서 이야기한 1) 0을 제거하는 일과 2) 변환한 2진수의 길이를 2진수로 바꾸는 일 2가지를 반복적으로 해줌으로써 풀 수 있었다. `temp`를 이용해 기존의 문자열과 적절하게 바꿔주면서 푸는 게 중요했던 것같다.

## 코드

```cpp
#include <string>
#include <vector>

using namespace std;

vector<int> solution(string s) {
    int cnt = 0,    zero_cnt = 0;
    string tmp = "";
    
    while(s != "1"){
        for(char c : s){
            if(c == '1')    tmp += '1';                 // 1) 0 제거
            else    zero_cnt++;
        }
        s = tmp,    tmp = "";
        
        int size = s.length();                          // 2) 길이 변환
        while(size > 0)
            tmp += to_string(size % 2),  size /= 2;
                
        s = "";
        for(int i = tmp.length() - 1; i >= 0; i--)
            s += tmp[i];
        tmp = "";
        
        cnt++;
    }
    
    return {cnt, zero_cnt};
}
```
