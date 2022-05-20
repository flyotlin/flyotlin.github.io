---
title: Uva 10154 - Weights and Measures
date: 2020-11-18T17:01:22+08:00
# tags: 
# - OJ解題 
# - Uva
# categories: 解題
---
## 解題想法
先按照烏龜的力量由小到大排序。
<!-- more -->
再依序檢查每隻烏龜，從塔的下方試試看，如果他的力量夠大，且重量比原本的輕就放進去。

> **dp[i]: 儲存第i層烏龜塔所承受的總重量**


    if dp[i-1] + turtle[i].weight < turtle[i].strength

        dp[i] = min{dp[i], dp[i-1] + turtle[i].weight}

有點LIS的味道，同樣是檢查此元素(數字/烏龜)是否能接到這個地方，但烏龜塔這題會從塔的下方檢查上去。

如果從反方向，可能會因為這層的承受重量被改過了，下一層就不是原本的那個承受重量。
## 程式碼
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

typedef struct {
    int weight, strength;
} turtle;

bool cmp(turtle &a, turtle &b)
{
    if (a.strength == b.strength)
        return a.weight < b.weight;
    return a.strength < b.strength;
}

int dp[5610];

int main()
{
    vector<turtle> vec;
    int a, b;
    while (cin >> a >> b) {
        turtle tmp;
        tmp.weight = a;
        tmp.strength = b;
        vec.push_back(tmp);
    }
    sort(vec.begin(), vec.end(), cmp);

    for (int i = 0; i < 5610; i++)
        dp[i] = 1e9;
    dp[0] = 0;

    for (int i = 1; i <= vec.size(); i++) 
        for (int j = i; j > 0; j--) 
            if (dp[j-1] + vec[i].weight < vec[i].strength) 
                dp[j] = min(dp[j], dp[j-1] + vec[i].weight);

    int level = vec.size();
    while (dp[level] == 1e9)
        level--;
    cout << level << endl;

    return 0;
}
```