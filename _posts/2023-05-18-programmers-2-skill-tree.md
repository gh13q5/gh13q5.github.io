---
title: "[Summer/Winter Coding(~2018)] 스킬트리 (C++)"
author: gh13
date: 2023-05-18 15:18:00 +0800
categories: [프로그래머스, Lv.2]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/49993>
{: .prompt-info }

## 풀이

`스킬트리의 스킬 중 skill에 포함되는 스킬로만 구성된 문자열`을 만들어준 후, 해당 문자열이 skill에 포함되면 answer를 증가시켜주었다. 이 때, skill에 포함되어도 가장 앞에 오는 첫 번째 스킬이 빠져 있으면 불가능한 스킬트리이기 때문에 `skill.find()의 반환값이 0일 때만 증가`시키도록 했다.

## 코드

```cpp
#include <string>
#include <vector>

using namespace std;

int solution(string skill, vector<string> skill_trees) {
    int answer = 0;
    for(auto t : skill_trees){
        string include_skill = "";
        for(auto c : t)
            if(skill.find(c) != string::npos)   include_skill += c;
        if(skill.find(include_skill) == 0)  answer++;
    }
    return answer;
}
```
