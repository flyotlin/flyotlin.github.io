---
title: "Leetcode 4 — Median of Two Sorted Arrays"
date: 2025-02-14T16:25:17+08:00
tags:
- 刷題
- Leetcode
---

https://leetcode.com/problems/median-of-two-sorted-arrays
## Solution

這題給你兩個 sorted array，請你找出這兩個 array 合併後的中位數。
### Brute force

測資的限制其實挺寬鬆的，雖然要求 O(log(m+n)) 的時間複雜度，但就算用 O(m+n) 的解法也是能 AC。

brute force 就單純的直接 merge 兩個 array，再直接計算 median 回傳。
### Two pointers

在兩個 vector 上分別維護 pointer i, j，初始值為 0。迭代更新 i, j，如果 v1[i] ≤ v2[j]，就把 i 加一。

更新過程中，也把最新的兩個 value 存到 m1, m2 中。

持續以上的過程，直到累積 size/2 個 value，最後就能把 m1, m2 的 value 當作 median 回傳。

https://github.com/flyotlin/leetcode/blob/main/4-median-of-two-sorted-array/2.cpp
## Note
### Vector initialization

    vector<int> vec = {1, 2, 3}; : Using initializer list
    vector<int> vec;
    vector<int> vec(num, value); : Initialize vector with num values
    vector<int> vec = {arr, arr+size};
    vector<type> vec(v.begin(), v.end());

### `const` for variables in C++

    一般的 scalar
    在 stack 上的 scalar value 不能被修改
    指標
    依照 const 的位置，會導致 stack 上的 pointer 或 heap 上的實際內容不能被修改

// scalar
const int num = 5;
num = 6;  // error

// pointer (指標指向的 heap 內容不可修改)
const char* name = "abc";

// pointer (在 stack 上的指標本身不可修改)
char* const name = "abc";

### `const` for function in C++

    const 加在 member function 前
    不能修改 member function 的回傳值，通常用在 reference 上。
    const 加在 member function 後
    該 member function 不能修改 data member。

class Person {
public:
  void toggle() const {
    int n = 2;
    n = 3;  // ok

    name = "abc";  // compile-error
  }

  const string& getName() {
    return this->name;
  }
private:
  string name;
}

int main() {
  Person p;
  const string& name = p.getName();
  
  name = "abc";  // compile-error
}

## Refs

    https://www.geeksforgeeks.org/initialize-a-vector-in-cpp-different-ways/
    https://shengyu7697.github.io/cpp-const/