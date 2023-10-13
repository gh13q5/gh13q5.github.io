---
title: "[월간 코드 챌린지 시즌1] 두 개 뽑아서 더하기 (C++)"
author: gh13
date: 2023-05-18 15:17:00 +0800
categories: [프로그래머스, Lv.1]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/68644>
{: .prompt-info }

## 풀이

중복값을 제거하고 자동정렬해주는 `Set`을 이용해 풀 수 있었다.

## 새로 푼 코드 (2023.04.25)

```cpp
#include <string>
#include <vector>
#include <set>

using namespace std;

vector<int> solution(vector<int> numbers) {
    set<int> sum_list;
    for(int i = 0; i < numbers.size() - 1; i++)
        for(int j = i + 1; j < numbers.size(); j++)
            sum_list.insert(numbers[i] + numbers[j]);
    
    vector<int> answer(sum_list.begin(), sum_list.end());
    return answer;
}
```

## 이전 코드 (2022.10)

```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

vector<int> solution(vector<int> numbers) {
    vector<int> answer;
    
    int sum = 0;
    for(int i = 0; i < numbers.size(); i++){
        for(int j = i + 1; j < numbers.size(); j++){
            sum = numbers[i] + numbers[j];
            for(int k = 0; k < answer.size(); k++){
                if(sum == answer[k]){
                    sum = -1;
                    break;
                }
            }
            if(sum != -1){
                answer.push_back(sum);
            }
        }
    }
    
    sort(answer.begin(), answer.end());
    return answer;
}
```
