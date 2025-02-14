---
title: "Leetcode 7 — Reverse Integer"
date: 2025-02-14T16:26:11+08:00
tags:
- 刷題
- Leetcode
---

很簡單的一個題目，唯一要注意的點是怎麼判斷 int overflow。

https://leetcode.com/problems/reverse-integer
Solution

定義 n 為原本的數字。做法是用 % 取餘數，取得 n 的最小位數。接著再 n/10，持續這件事直到 n 為 0。

```
n := original number
rev := reverse of n

while n != 0
  remainder = n % 10
  rev = 10 * rev + remainder
```
但要判斷反轉後的 n 是不是會 overflow。

int 的範圍是 2³¹ -1 ~ -2³¹，也就是 2147483647 ~ -2147483648。

根據上面的 pseudocode，可以知道 10*rev+remainder 可能造成 overflow，因此要特別檢查。

10 * rev + remainder >= INT_MAX
rev >= (INT_MAX - remainder) / 10

如果 rev > INT_MAX，肯定會造成 overflow。

如果 rev = INT_MAX，就要看 remainder。INT_MAX / 10 會把個位數的 7 去掉，所以當 remainder 大於 7，就會造成 overflow。

INT_MIN 也是同理，當 remainder 小於 -8，就會 overflow。
## Note
### C/C++ 負數除法與取餘

    cout << 12 % 10 << endl;    // 2
    cout << 12 / 10 << endl;    // 1

    cout << -12 % 10 << endl;   // -2
    cout << -12 / 10 << endl;   // -1

    cout << -23 % 10 << endl;   // -3
    cout << -23 / 10 << endl;   // -2

跟正整數的除法、取餘一樣，只不過加上負號。

跟 Python 負數取餘 (餘數不會是負的) 的行為不同，但跟 Javascript 的行為相同。
### C++ template 模板

template 其實就是泛型，可以將具體的資料型別抽離程式碼，在 compile time 透過 compiler 自動帶入型別 + 檢查。

template 分為兩種，function template 與 class template。

example:

// funciton template
template <class T>
T add(T a, T b) {
    return a + b;
}

// class template
template <class T>
class MyClass
{
    T& add(const T& a) {
        return *this;
    }
};

MyClass<int> c1;

通常動態型別的語言不需要 template/generic，因為本身就支援動態型別了，如 Python、Javascript。

但像是 Typescript 因為引入型別，因此又支援 generics。
## Refs

    https://www.rocksaying.tw/archives/3641717.html