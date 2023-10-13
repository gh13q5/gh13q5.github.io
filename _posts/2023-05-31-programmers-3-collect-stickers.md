---
title: "[Summer/Winter Coding(~2018)] 스티커 모으기(2) (C++)"
author: gh13
date: 2023-05-31 17:48:00 +0800
categories: [프로그래머스, Lv.3]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/12971#>
{: .prompt-info }

## 풀이

앞에서부터 차례대로 스티커를 떼어나갈 때, `해당 스티커를 뗐을 때와 떼지 않았을 때 둘 중 어느 쪽의 합이 더 큰지`를 기록하는 `DP`를 이용해 풀어주었다. 예를 들면 3번째 스티커 차례에서 1번째 스티커와 3번째 스티커를 같이 뗀 값이 더 큰지, 3번째 스티커를 떼지 않고 바로 앞에 있는 2번째 스티커만 뗀 값이 더 큰지 비교하는 식이다.  

> [점화식] dp[i] = max(dp[i - 1], dp[i - 2] + sticker[i])
{: .prompt-tip }

원형으로 이어져 있기 때문에, `첫번째 스티커를 떼는 경우와 그렇지 않은 경우` 2가지로 나눠서 반복문을 실행시켰다.   

처음엔 단순히 홀수 번호의 원소들을 더한 값과 짝수 번호의 원소들을 더한 값을 비교해서 더 큰 값을 반환하는 식으로 풀었다. 주어진 예제 2개는 당연하게도 돌아갔지만, 테케가 문제였다... DP 문제임을 눈치채는 게 조금 느린 것같다. 점화식 세우는 법이랑 같이 공부를 더 해봐야 할 것같다.  

## 코드

```cpp
#include <vector>
#include <algorithm>

using namespace std;

int solution(vector<int> sticker){
    if(sticker.size() == 1)     return sticker[0];
    
    vector<int> dp(sticker.size());
    
    dp[0] = sticker[0],    dp[1] = dp[0];   // 첫번째 원소 포함
    for(int i = 2; i < sticker.size() - 1; i++)
        dp[i] = max(dp[i - 1], dp[i - 2] + sticker[i]);
    int sum1 = *max_element(dp.begin(), dp.end());
    
    dp[0] = 0;      // 첫번째 원소 미포함
    for(int i = 1; i < sticker.size(); i++)
        dp[i] = max(dp[i - 1], dp[i - 2] + sticker[i]);
    int sum2 = *max_element(dp.begin(), dp.end());
    
    return max(sum1, sum2);
}
```
