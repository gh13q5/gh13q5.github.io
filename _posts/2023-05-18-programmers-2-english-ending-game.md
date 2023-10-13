---
title: "[Summer/Winter Coding(~2018)] 영어 끝말잇기 (C++)"
author: gh13
date: 2023-05-18 15:48:00 +0800
categories: [프로그래머스, Lv.2]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/12981>
{: .prompt-info }

## 풀이

이전 단어의 끝과 지금 단어의 앞이 다르면 곧바로 return해주고, 이전에 등장한 단어들을 저장한 배열 said에서 중복된 단어가 발견되면 return해주는 방식으로 풀 수 있었다.

return할 때는 `현재 인덱스 값의 나머지 값과 나눈 값`을 이용했다. 끝말잇기 참가 인원이 3명이고 현재 인덱스 값이 2라면, (2 % 3 + 1 = 3)번째 참가자가 (2 / 3 + 1 = 1)번째 차례에서 탈락했다는 뜻이 된다.

## 코드

```cpp
#include <string>
#include <vector>

using namespace std;

vector<int> solution(int n, vector<string> words) {
    string pre = words[0];
    vector<string> said;
    said.push_back(pre);
    
    for(int i = 1; i < words.size(); i++){
        if(pre.back() != words[i].front())  return {i % n + 1, i / n + 1};
        for(int j = 0; j < said.size(); j++)
            if(words[i] == said[j])     return {i % n + 1, i / n + 1};
        pre = words[i];
        said.push_back(pre);
    }
    return {0, 0};
}
```
