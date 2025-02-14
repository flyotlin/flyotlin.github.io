---
title: "Leetcode 225 — Implement Stack Using Queues"
date: 2025-02-14T16:24:35+08:00
tags:
- 刷題
- Leetcode
---

接續昨天的選題，今天挑了一個 queue 相關的題目。

https://leetcode.com/problems/implement-stack-using-queues
## Solution

題目的要求是使用兩個 queue (q1、q2) 來模擬 stack FILO 的行為。用紙筆簡單畫了一下，有了一點想法。

主要的想法是，在 stack pop 前，push() 就只塞到其中一個 queue 裡面。

在過程中，如果遇到：

    empty()：檢查兩個 queue 的 size
    top()：回傳當前 queue 的 back()

最後，當 stack pop() 的時候，持續 pop q1，並且將數值塞到 q2，直到 q1 為空。pop 的過程中，最後一個元素是 stack 的 top，要特別保留下來，並且不塞到 q2。
## Note
### STL Queue

member functions:

    push()
    pop()
    front(): queue head, popped first
    back(): queue tail, popped last
    size()
    empty()

### C++ Class

Class 是 OOP 為了達成資料抽象 (abstraction) 與資料封裝 (encapsulation) 的手段，用來區分介面 (interface) 與實作 (implementation)。

在 C 的世界中，其實老早就有抽象的概念，將 declaration (.h) 與 definition (.c) 拆分開來。

在 class 的實作中，可以不用加上 this，直接訪問被定義的 member data 或 function。

C++ 的 class constructor 可以利用 initialization list 快速初始化 member data。

C++ class 中的 this 是一個指向 object 本身的 pointer。

C++ 的 class 還有 copy constructor、move constructor，還可以做 operator overloading，詳細的介紹之後有讀到再補充。（Rules of three/five）

class T {
public:
  T() {}  // constructor
  ~T() {}  // destructor
  // member data
  // member function
private:
  // members
protected:
  // members
};

## Reference

    https://cplusplus.com/reference/queue/queue/
    https://hackmd.io/@Mes/MinerT_Class