---
title: Uva 674 - Coin Change
date: 2020/11/16
tags: 
- OJ解題 
- Uva
categories: 解題
---
## 解題想法
題目要問的是給定一個金額，請問用{1, 5, 10, 25, 50}來湊的話，共有幾種不同的湊法。
這題其實就是無限背包問題，所以會用到動態規劃的概念。只是把背包的限制重量變成題目欲湊出的金額。物品變成面額。
然後面額的數量沒有限制，隨你喜歡用多少就用多少。

可以分別考慮加上每個面額後，能湊出該金額的方法數會如何變化。
也就是以下的這個轉移式:
> **dp(金額) += dp(金額 - 當下面額)**

## 程式碼
```c++
#include <iostream>
using namespace std;

int main()
{
    int dp[8000] = {1};
    int coins[5] = {1, 5, 10, 25, 50};

    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 8000; j++) {
            if (j - coins[i] >= 0)
                dp[j] += dp[j - coins[i]];
        }
    }

    int money;
    while (cin >> money) 
        cout << dp[money] << endl;

    return 0;
}

```