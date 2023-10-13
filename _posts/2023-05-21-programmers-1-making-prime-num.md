---
title: "[Summer/Winter Coding(~2018)] 소수 만들기 (C++)"
author: gh13
date: 2023-05-21 13:57:00 +0800
categories: [프로그래머스, Lv.1]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/12977>
{: .prompt-info }

## 풀이

`next_permutation()`을 이용해 `조합`을 구해준 후,해당 조합으로 구해진 숫자의 소수 여부를 확인해주었다.  

사소하지만 실수하기 좋았던 부분 하나~ next_permutation()의 인자에 nums와 관련된 반복자를 넣어줬더니 중복 없는 조합을 구하는 코드인데도 불구하고, 자꾸 중복이 발생해서 순열이 구해졌었다. bool 벡터인 tmp의 반복자를 넣어줘야만, {false, false, true, true, true}-{true, false, false, true, true} 식으로 이동해가며 조합이 구해지는 것같다. 간과하지 말아야겠다 ㅜ

## 코드

```cpp
#include <vector>
#include <algorithm>

using namespace std;

bool is_prime(int n){
    for(int i = 2; i < n; i++)
        if(n % i == 0)  return false;
    return true;
}

int solution(vector<int> nums) {
    sort(nums.begin(), nums.end());
    
    vector<bool> tmp(nums.size(), true);
    for(int i = 0; i < nums.size() - 3; i++)
        tmp[i] = false;

    int cnt = 0;
    do{
        int sum = 0;
        for(int i = 0; i < nums.size(); i++)
            if(tmp[i])  sum += nums[i];
        if(is_prime(sum))   cnt++;
    }while(next_permutation(tmp.begin(), tmp.end()));
    return cnt;
}
```
