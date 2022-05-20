---
title: Uva 455 - Periodic Strings
date: 2020-02-06T17:01:22+08:00
# tags: 
# - OJ解題 
# - Uva
# categories: 解題
---

題目連結: [Uva455](https://onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=396)

## 解題想法
### 輸入
第一行是1個整數n，代表有幾組測資。接下來每組測資都包含兩行，分別是**空白行**以及**一個字串**(最多80個字元)。

### Solve
題目要我們找出字串的**最小週期**

一個字串最小週期的最大可能值是字串本身的長度，所以用for迴圈，假設i為可能最小週期，從i=1開始檢查到i=字串長度。

由於最小週期一定要是字串長度的因數，也就是最小週期一定要能整除字串的長度。所以進到第一個for迴圈內後，用一個if檢查前面的陳述(假設的最小週期是否整除字串長度)是否為真。
<!-- more -->

接下來是我在寫的時候想比較久的地方，用第二個for迴圈從字串的第一個字元檢查到最後一個字元，搭配if檢查該字串的最小週期是否如同我們假設的i。這個迴圈從頭檢查到尾，裡面的if檢查字元是否等於**字元Mod最小週期**。

## 字元是否等於字元Mod最小週期

字串word: abcdabcd

a b c d a b c d

0 1 2 3 4 5 6 7

假設最小週期=2

word[0] == word[0%2]

word[1] == word[1%2]

word[2] != word[2%2]

word[3] != word[3%2]

word[4] == word[4%2]

word[5] == word[5%2]

word[6] != word[6%2]

word[7] != word[7%2]

假設最小週期=4

word[0] == word[0%2]

word[1] == word[1%2]

word[2] == word[2%2]

word[3] == word[3%2]

word[4] == word[4%2]

word[5] == word[5%2]

word[6] == word[6%2]

word[7] == word[7%2]

如果if檢查發現不同就直接跳出，假設大一點的最小週期。如果相同就繼續檢查，直到最後一個字元。

在輸出的時候要注意格式，題目要求:

> 對於每組測資輸出一個整數，表示輸入字串的最小period。
兩個連續的輸出由空白行分隔。

空白行指的是每組測資的結果輸出後都要換行，最後一組就不用再換行，光是格式我就吃了好幾次的NA。

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
有興趣的可以參考一下~

