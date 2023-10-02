---
title: "[PCCP 모의고사 1회] 외톨이 알파벳 (C++)"
author: gh13
date: 2023-10-02 15:29:00 +0800
categories: [프로그래머스, PCCP 모의고사]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/15008/lessons/121683>
{: .prompt-info }

## 풀이

우선 문자열에서 `연속으로 등장하는 알파벳들을 제거`해준 후 알파벳 순으로 정렬했다. `정렬된 문자열에서 2번 이상 나타나는 알파벳`을 2회 이상 나타나면서도 2개의 부분으로 나누어져 있는 외톨이 알파벳이라 판단해 풀었다.  

> edeaaabbccd -> edeabcd -> abcddee -> de
{: .prompt-tip }

## 코드

```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

string solution(string input_string) {
    string tmp = "";
    tmp += input_string[0];
    for(int i = 0; i < input_string.length() - 1; i++){
        if(input_string[i] == input_string[i + 1])  continue;
        tmp += input_string[i + 1];
    }
    
    sort(tmp.begin(), tmp.end());
    
    string answer = "";
    for(int i = 0; i < tmp.length(); i++)
        if(tmp[i] == tmp[i + 1] && answer.back() != tmp[i])    answer += tmp[i];
    if(answer == "")    return "N";
    return answer;
}
```
