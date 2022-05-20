---
title: Uva 10633 - Rare Easy Problem
date: 2020-02-04T17:01:22+08:00
# tags: 
# - OJ解題 
# - Uva
# categories: 解題
---

題目連結: [Uva10633](https://onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=1574)

## 解題想法
N, M

其中M為N除去最小位數

用數學式表達即 M = ( N - (N%10) ) / 10

題目Input: N-M

希望Output:所有可能的N
<!-- more -->
首先從數學式著手 M = ( N - (N%10) ) / 10

題目給的Input: N-M = N - ( N - (N%10) ) / 10

所以 10(N-M) = 9N + (N%10)

又N%10的可能解為0, 1, 2, 3, 4, 5, 6, 7, 8, 9

(10k, 10k+1, 10k+2, ……, 10k+9)

到此題目就變得十分簡單明瞭了 跟名稱Easy一樣(重點誤)

開個for迴圈 宣告i 從0 ~ 10

首先檢查 10*(N-M) - (N%10) 是否被 9 整除

if整除 則計算N的值 並將他輸出

題目測資可能會有10^16次方大 記得用long long儲存

## C++程式碼
```cpp
#include <iostream>
using namespace std;

int main()
{
    long long int k, n;
    while(cin >> k && k != 0){
        for(int i = 9; i >= 0; i--) {
            if((10*k-i)%9 == 0){    //先判斷是否整除 而非先直接除再判斷是否為整數
                n = (10*k-i)/9.0;
                cout << n << " ";
            }
        }
        cout << endl;
    }
    return 0;
}
```