---
title: "[월간 코드 챌린지 시즌1] 삼각 달팽이 (C++)"
author: gh13
date: 2023-05-03 17:31:00 +0800
categories: [프로그래머스, Lv.2]
tags: [C++, 프로그래머스]
render_with_liquid: false
---

> <https://school.programmers.co.kr/learn/courses/30/lessons/68645>
{: .prompt-info }

## 풀이

주어진 삼각형을 `이차원 배열`처럼 생각해주는 것이 이 문제의 포인트였다.

![matrix image](/assets/img/post_img/2023-05-03-01.png){: width="972" }

주대각선을 포함한 하삼각행렬과 같은 모양
switch문을 이용해 현재 삼각형의 왼쪽 변이나 아랫변, 오른변 중 어느 변을 이동하고 있는지 구분한 후, `현재 위치를 나타내는 x와 y 변수를 이동`시켜가며 값을 넣어주는 방식이다.

처음에는 단순히 숫자 사이의 규칙만을 찾아서 적용시키려 했지만 잘 되지 않았다... 그 다음으로 찾은 해결법은 재귀였는데 이 방법은 종료조건을 찾지 못 했다. 문제에서 주어진 상황을 적절히 변형하는 훈련이 필요한 것같다.

## 코드

```cpp
#include <string>
#include <vector>

using namespace std;

vector<int> solution(int n) {
    int size = n * (n + 1) / 2;
    vector<vector<int>> triangle(n + 1, vector<int>(n + 1, 0));
    
    char pos = 'l';
    int i = 1, x = 0, y = 0;
    while(i <= size){
        int max_num = i + n - 1;
        switch(pos){
            case 'l':
                while(i <= max_num)
                    triangle[y++][x] = i++;
                pos = 'b', y--;
                break;
            case 'b':
                while(i <= max_num - 1)
                    triangle[y][++x] = i++;
                pos = 'r';
                break;
            case 'r':
                while(i <= max_num - 2)
                    triangle[--y][--x] = i++;
                pos = 'l', n -= 3, y++;
                break;
        }
    }
    
    vector<int> answer;
    for(auto r : triangle)
        for(auto c : r)
            if(c != 0)  answer.push_back(c);
    return answer;
}
```
