---
title: Uva 10157 - Expressions
date: 2020-10-04T17:01:22+08:00
# categories: 解題
# tags: 
#     - OJ解題 
#     - Uva
---

# Catalan Number
https://zh.wikipedia.org/wiki/%E5%8D%A1%E5%A1%94%E5%85%B0%E6%95%B0
https://blog.csdn.net/u011489043/article/details/77884434
1. (0, 0) ---> (n, n)
2. 矩陣相乘，結合律
3. 括號總數

<!-- more -->
# 神秘的解題公式
https://www.twblogs.net/a/5baaee952b7177781a0eb263/

dp(k, d): k對括號，深度**不超過**d的方法數

dp(k, d)算法:

> dp(k, d) += summation(dp(i, d-1) * dp(k-1 - i, d)), i from 0 to k-1

上面summation內的dp只有k-1個括號，所以表達形式如下:

> **(A)B**

- 疑問一: 為甚麼summation內是用相乘的?
- 疑問二: 為甚麼特地讓summation內的括號總數是k-1，而非k。是特意為了方便dp而設計的嗎?
- 疑問三: 為甚麼dp(k, d)要是k對括號，深度不超過d的方法數，而非k對括號，深度為d的方法數。

欲求k對括號，深度為d的方法數
> dp(k, d) - dp(k, d-1)

