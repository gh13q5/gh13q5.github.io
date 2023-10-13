---
title: "[월간 코드 챌린지 시즌2] 2개 이하로 다른 비트 (C++)"
author: gh13
date: 2023-05-03 16:50:00 +0800
categories: [프로그래머스, Lv.2]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/77885>
{: .prompt-info }

## 풀이

`짝수인 경우와 홀수인 경우`로 나눠 생각해볼 수 있는 문제였다.

짝수라면 bit의 2^0 자리가 무조건 0으로 끝나기 때문에 1 큰 수가 답이다. 홀수라면 `bit에서 처음으로 등장하는 0`의 자릿수에 해당하는 2의 제곱수를 더해준 후, 그의 오른쪽 자리의 1을 0으로 바꿔준 수가 답이었다. 이 때, pow 함수를 이용해 자릿수에 맞는 2의 제곱수를 구할 수 있었다. (2, 4, 8, 16 등...)

처음에 XOR 및 시프트 연산을 이용해 접근해봤는데, 2문제 정도가 시간초과가 발생했다. 하나하나 비교하는 방식을 사용한 게 문제였던 것같다.

## 코드

```cpp
#include <string>
#include <vector>
#include <cmath>

using namespace std;

vector<long long> solution(vector<long long> numbers) {
    vector<long long> answer;
    for(int i = 0; i < numbers.size(); i++){
        if(numbers[i] % 2 == 0)     answer.push_back(numbers[i] + 1);
        else{
            int idx = 0;
            long long tmp = numbers[i];
            while(tmp != 0){
                if(tmp % 2 == 0)    break;
                idx++,  tmp /= 2;
            }
            answer.push_back(numbers[i] + pow(2, idx) - pow(2, idx - 1));
        }
    }
    return answer;
}
```
