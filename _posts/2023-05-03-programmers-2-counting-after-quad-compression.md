---
title: "[월간 코드 챌린지 시즌1] 쿼드압축 후 개수 세기 (C++)"
author: gh13
date: 2023-05-03 17:19:00 +0800
categories: [프로그래머스, Lv.2]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/68936>
{: .prompt-info }

## 풀이

주어진 칸을 `왼쪽위-오른쪽위-왼쪽아래-오른쪽아래 총 4칸으로 쪼개고,` 쪼갠 칸도 다시 4칸으로 쪼개는 `재귀`를 이용했다. 종료조건은 배열의 최소 크기인 1칸에 다다랐을 때와 모든 칸이 같은 값을 가졌을 때, 2가지 경우로 정했다.

Effective C++를 공부하는 도중, 변수는 최대한 늦게 정의하고 초기화하는 것이 좋다고 한 점을 이용해 풀어보려 했는데... 빠른 코드를 만드려면 신경 써야할 부분이 생각보다 많구나 싶다ㅜ

## 코드

```cpp
#include <string>
#include <vector>
#include <iostream>

using namespace std;

int zero_cnt = 0,   one_cnt = 0;

void quad_tree(int size, int start_x, int start_y, const vector<vector<int>> &arr){
    int start_data = arr[start_x][start_y];
    if(size == 1){
        (start_data == 0) ? zero_cnt++ : one_cnt++;
        return;
    }
    
    bool isComb = true;         // 하나로 합쳐지는가?
    for(int i = start_x; i < start_x + size; i++){
        for(int j = start_y; j < start_y + size; j++)
            if(arr[i][j] != start_data){
                isComb = false;
                break;
            }
        if(!isComb)     break;
    }
    
    if(isComb){
        (start_data == 0) ? zero_cnt++ : one_cnt++;
        return;
    }
    
    quad_tree(size / 2, start_x, start_y, arr);
    quad_tree(size / 2, start_x, start_y + (size / 2), arr);
    quad_tree(size / 2, start_x + (size / 2), start_y, arr);
    quad_tree(size / 2, start_x + (size / 2), start_y + (size / 2), arr);       // 왼위 - 오위 - 왼아 - 오아
}

vector<int> solution(vector<vector<int>> arr) {
    quad_tree(arr.size(), 0, 0, arr);
    return {zero_cnt, one_cnt};
}
```
