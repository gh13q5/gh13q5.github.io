---
title: "[Summer/Winter Coding(~2018)] 기지국 설치 (C++)"
author: gh13
date: 2023-05-23 16:02:00 +0800
categories: [프로그래머스, Lv.3]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/12979>
{: .prompt-info }

## 풀이

`현재 기지국의 왼쪽에 있는 아파트 중, 전파가 닿지 않으면서도 가장 멀리 떨어져 있는 아파트`와의 거리를 이용해 풀어주었다.(변수 prev로 표현) prev에서 현재 기지국까지의 거리를, 새로 기지국을 세웠을 때 가질 수 있는 전파의 범위`(w * 2 + 1)`로 나눠주었다. ceil()을 이용해 올림해줘야 전파가 닿지 않는 구역 없이 기지국을 설치할 수 있다. 예로 prev가 4이고 현재 기지국의 위치가 10, w가 1라면 prev와 현재 기지국 사이에 (10 - 4 - 1) / (1 * 2 + 1) = 2.xxx, 즉 3개의 기지국을 설치해야 한다.  

처음에는 범위를 표현한 n의 값이 너무 크고, stations가 정렬되어 있다는 점에 착안해서 이분탐색으로 접근했었다. 하지만, 코드를 짤수록 탐색을 반복할 필요도 없어 보였고, 무엇보다도 min과 max를 줄이고 늘리는 기준이 마련되지 않았다. 또 테케 1번이랑 15번에서 꽤 애먹었었는데, 마지막 station에서 오른쪽 끝 아파트까지 필요한 기지국의 수를 계산할 때 칸 수가 1칸 부족했어서 계속 실패가 떴던 것같다. `구간이 칸 수로 계산`되다 보니 이런 예외가 발생하는구나......  

그리고! ceil()을 사용할 때 (double)을 사용하고 가독성을 택할까 고민했었지만, `C++ 스타일의 캐스팅이 가능할 때는 C 스타일의 캐스팅은 피해보는 게 좋다`해서 static_cast<double>()을 사용해주었다. 처음의 코드를 효율과 가독성을 생각해 여러 번 수정한 문제라 그런지, 짧은 문제에서도 배운 게 많은 것처럼 느껴진다.  

## 코드

```cpp
#include <vector>
#include <cmath>

using namespace std;

int solution(int n, vector<int> stations, int w){
    int cnt = 0, prev = 1;
    for(auto st : stations){
        if(st - w > prev)
            cnt += ceil(static_cast<double>(st - prev - w) / static_cast<double>(w * 2 + 1));
        prev = st + w + 1;
    }
    if(prev <= n)           // 마지막 station이 마지막 아파트에 없을 경우
        cnt += ceil(static_cast<double>(n - prev + 1) / static_cast<double>(w * 2 + 1));
    return cnt;
}
```
