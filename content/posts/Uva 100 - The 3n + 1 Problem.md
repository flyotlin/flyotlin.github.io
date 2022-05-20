---
title: Uva 100 - The 3n + 1 Problem
date: 2020-02-05T17:01:22+08:00
# tags: 
# - OJ解題 
# - Uva
# categories: 解題
---

## 題目
[Uva 100- The 3n+1 problem](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=36)

```
f(n) = 3n+1 , n為奇數 – (1)

     = n/2 , n為偶數  – (2)
```

簡而言之，題目會input兩個數字(a, b)，藉由f(n)的運算，請你輸出依照input順序輸出a b，

以及在a b之間最長的cycle length的長度。
<!-- more -->
*cycle length:

假設n=22，會依序跑出 22 11 34 17 52 26 13 40 20 10 5 16 8 4 2 1，

則f(22)的cycle length為16

## 解題想法
題目給的定義為:

```
f(n) = 3n+1 , n為奇數 – (1)

     = n/2 , n為偶數  – (2)
```

f(n)結束的條件為n=1的時候

其實這個定義在數學上叫做考拉茲猜想，有興趣可以去維基百科上了解更多。

所以只要用while迴圈，條件為n != 1時，重複做(1) (2)的兩件事，直到n=1即可。

要特別注意的是輸入的兩個數第二個不一定會大於第一個，所以程式中要注意

兩個數的順序。

此外，題目輸出時，前兩個要按照輸入時的順序輸出，然後第三個再輸出最長

的cycle length。

## C++程式碼

```cpp
#include <iostream>

using namespace std;
int three_n(int);
int main()
{
    int a, b;
    while(cin >> a >> b)
    {
        int temp, aa, bb;
        if(a > b)
        {
            bb = a;
            aa = b;
        }
        else
        {
            aa = a;
            bb = b;
        }
        int max = 0;
        for(int i = aa; i <= bb; i++)
        {
            max = (three_n(i) > max) ? three_n(i) : max;
        }
        cout << a << " " << b << " " << max << endl;
    }

    return 0;
}
int three_n(int n)
{
    int count=0;
    while(n != 1)
    {
        if(n%2 == 1)
        {
            n = 3*n+1;
            count++;
        }
        else
        {
            n /= 2;
            count++;
        }
    }
    return count+1;
}
```