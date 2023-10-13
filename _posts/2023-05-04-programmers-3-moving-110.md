---
title: "[월간 코드 챌린지 시즌2] 110 옮기기 (C++)"
author: gh13
date: 2023-05-04 15:58:00 +0800
categories: [프로그래머스, Lv.3]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/77886#>
{: .prompt-info }

## 풀이

`문자열에 포함된 "110"을 모두 찾아 없애준 후,` 가장 마지막으로 등장하는 '0'의 다음에 "110"을 없앤 개수만큼 붙여주는 식으로 풀어줬다.

처음에는 while()문과 string::find()를 이용해서 "110"을 제거해줬는데, 채점하니 시간초과가 꽤 나더라... 그리고 for each 문으로 접근해서 풀면 원래의 배열에 영향을 안 미치는 걸까? 메모리를 아끼는 게 좋을 것같아서 바꿔준 s를 곧바로 return 했더니 그동안의 코드가 적용이 안 된 s가 반환됐다.

## 코드

```cpp
#include <string>
#include <vector>
#include <iostream>

using namespace std;

vector<string> solution(vector<string> s) {
    vector<string> answer;
    for(string cur : s){
        int cnt = 0;
        for(int i = 0; (cur.length() > 3) && (i < cur.length() - 2); i++){
            if(cur[i] == '1' && cur[i + 1] == '1' && cur[i + 2] == '0'){
                cur.erase(i, 3);
                i > 2 ? i -= 4 : i = -1;
                cnt++;
            }
        }
        for(int i = cur.length() - 1; i >= 0; i--){
            if(i == 0 || cur[i] == '0'){
                int idx = (cur[i] == '0') ? i + 1 : i;
                for(int j = 0; j < cnt; j++)
                    cur.insert(idx, "110");
                break;
            }
        }
        answer.push_back(cur);
    }
    return answer;
}
```
