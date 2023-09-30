---
title: "[2021 Dev-Matching] 다단계 칫솔 판매 (C++)"
author: gh13
date: 2023-09-30 11:32:00 +0800
categories: [프로그래머스, Lv.3]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/77486>
{: .prompt-info }

## 풀이

각 판매자의 수익과 추천인을 담은 2가지의 `unordered_map`을 만들어 key값(직원 이름)으로 쉽게 접근할 수 있게 하고, 판매원의 이익을 분배하는 반복문을 돌려주었다. 반복문에서는 현재 직원의 수익을 더해주고 추천인을 다음 반복문의 대상으로 설정했다. 더이상 위로 분배할 수 있는 돈이 없을 때 반복을 끝낼 수 있도록 (money > 0) 조건도 추가했다!

<br/>

## 코드

```cpp
#include <string>
#include <vector>
#include <unordered_map>

using namespace std;

vector<int> solution(vector<string> enroll, vector<string> referral, vector<string> seller, vector<int> amount) {
    unordered_map<string, int> revenue;
    for(int i = 0; i < enroll.size(); i++)
        revenue.insert({enroll[i], 0});
    
    unordered_map<string, string> refer_m;
    for(int i = 0; i < enroll.size(); i++)
        refer_m.insert({enroll[i], referral[i]});
    
    for(int i = 0; i < seller.size(); i++){
        int money = amount[i] * 100;
        string cur = seller[i];
        while(money > 0){
            revenue[cur] += (money - (money / 10));
            money /= 10;
            
            string next = refer_m[cur];
            if(next == "-") break;
            cur = next;
        }
    }
    
    vector<int> answer(enroll.size(), 0);
    for(int i = 0; i < enroll.size(); i++)
        answer[i] = revenue[enroll[i]];
    return answer;
}
```
