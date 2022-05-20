---
title: Uva 10131 - Is Bigger Smarter?
date: 2020-11-25T17:01:22+08:00
# tags: 
# - OJ解題 
# - Uva
# categories: 解題
---
## 題目
題目給大象的體重及智商，求體重**嚴格**遞增，智商**嚴格**遞減的最長子集合(subset)。
<!-- more -->
## 解題想法
我們可以先將體重由小到大排序(也可以智商由大到小排序，後面就反著做)，再對智商做LDS。但要注意體重跟智商的值有可能相等，所以在對智商做LDS的時候，也要注意體重沒有取到相等的值。

參考: [Longest Increasing Subsequence](http://web.ntnu.edu.tw/~algo/LongestIncreasingSubsequence.html)
## 程式碼
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

typedef struct {
    int num, weight, iq;
} triple;

bool cmp(triple &a, triple &b)
{
    return a.weight < b.weight;
}

int length[1000];
int prev_iq[1000];
int main()
{
    int a, b, no = 1;
    vector<triple> elephants;
    while (cin >> a >> b) {
        triple tmp;
        tmp.num = no++;
        tmp.weight = a;
        tmp.iq = b;
        elephants.push_back(tmp);
    }

    sort(elephants.begin(), elephants.end(), cmp);

    for (int i = 0; i < elephants.size(); i++) {
        length[i] = 1;
        prev_iq[i] = -1;
    }
    for (int i = 0; i < elephants.size(); i++) {
        for (int j = 0; j < i; j++) {
            if (elephants[i].iq < elephants[j].iq && elephants[i].weight > elephants[j].weight) {   // 前面條件為智商，後面條件為確保體重沒取到相等的
                if (length[j] + 1 > length[i]) {
                    length[i] = length[j] + 1;
                    prev_iq[i] = j;
                }
            }
        }
    }

    int lis_length = -1234, maxIdx = 0;
    for (int i = 0; i < elephants.size(); i++) {
        if (length[i] > lis_length) {
            lis_length = length[i];
            maxIdx = i;
        }
    }
    cout << lis_length << endl;
    vector<int> lis;
    while (maxIdx != -1) {
        lis.push_back(maxIdx);
        maxIdx = prev_iq[maxIdx];
    }
    for (int i = lis.size() - 1; i >= 0; i--)
        cout << elephants[lis[i]].num << endl;

    return 0;
}
```