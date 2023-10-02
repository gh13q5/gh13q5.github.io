---
title: "[PCCP 모의고사 1회] 유전법칙 (C++)"
author: gh13
date: 2023-10-02 15:17:00 +0800
categories: [프로그래머스, PCCP 모의고사]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/15008/lessons/121685>
{: .prompt-info }

## 풀이

`현재 세대의 완두콩들을 4개로 나눈 묶음` 중, p가 몇 번째 묶음에 속하는지에 대한 `stack`을 활용해 풀 수 있었다. `p를 4로 나누어갈 때, 그 나머지들이 p가 속한 묶음의 번호`를 나타낸다. 첫 번째 묶음(RR)은 0, 두세번째 묶음(Rr)은 1과 2, 네번째 묶음(rr)은 3, 이런 방식이다. 이 때! 처음 나온 나머지값은 p가 속한 가장 작은 묶음에서의 위치가 되므로, 가장 큰 묶음에서부터 p의 위치를 알아가기 위해 FIFO 방식의 stack을 활용했다. 또한, 원활한 나머지 계산을 위해 (p-1)을 기준으로 진행했다.  

> query가 [3, 7]일 때의 stack (p = 7 - 1 = 6) : `[ 2, 1 |`  
> => 3세대의 7번째 개체는 3세대의 완두콩 16개를 4개로 나눈 묶음 중 두 번째 묶음에 위치하며(1), 해당 두 번째 묶음 안에서는 세 번째 위치에 존재한다.(2)
{: .prompt-tip } 

개체의 위치에 대한 stack을 완성한 후, `가장 큰 묶음의 부모부터 시작해서 해당 개체의 부모를 찾을 때까지` stack을 반복해주었다. 첫 번째 묶음의 부모는 무조건 RR, 네 번째 묶음의 부모는 무조건 rr이 되기 때문에 이렇게 부모가 바뀌게 된 경우는 확률을 고려할 필요가 없어 반복문을 빠져나왔다. 다르게 말하면, 부모가 Rr인 경우에만 1:2:1의 확률을 고려해야 하기 때문에 stack의 내용을 계속 진행해주었다.  

처음에는 4로 나누는 알고리즘은 알았어도 부모의 경우를 고려하지 않아서 몇 가지 테스트 케이스에서 오류가 발생했다. 특히, [3, 2]와 같은 맨 앞에 위치한 query가 등장할 때 오류가 발생했었는데, 아마 부모를 고려하지 않았던 점 그리고 p를 4로 나누는 계산을 반복할 때 계산이 한 번만 되어버리는 경우를 고려하지 못 한 이유에서였던 것같다.  

## 코드
```cpp
#include <string>
#include <vector>
#include <stack>

using namespace std;

vector<string> solution(vector<vector<int>> queries) {
    vector<string> answer;
    for(int i = 0; i < queries.size(); i++){
        int n = queries[i][0];
        int p = queries[i][1] - 1;
        stack<int> pos_st;
        
        for(int j = 0; j < n - 1; j++){
            pos_st.push(p % 4);
            p /= 4;
        }
        
        string parent = "Rr";
        while(!pos_st.empty()){
            int pos = pos_st.top();
            pos_st.pop();
            
            if(parent == "Rr"){
                if(pos == 0)    parent = "RR";
                else if(pos == 3)   parent = "rr";
            }
            else    break;
        }
        answer.push_back(parent);
    }  
    return answer;
}
```
