---
title: "Leetcode 3 — Longest Substring Without Repeating Characters"
date: 2025-02-14T16:25:27+08:00
tags:
- 刷題
- Leetcode
---
[https://leetcode.com/problems/longest-substring-without-repeating-characters](https://leetcode.com/problems/longest-substring-without-repeating-characters)

Solution
========

使用 sliding window + set 就能解決這個問題，只是一開始被前幾天解的 dp 題 (找「最長回文子字串 longest palandromic substring」) 擾亂思緒，往列舉出所有長度的子字串，再一個個的檢查是否 valid 的方向思考。

不過，這題 input string 長度是 10⁴，用列舉法的 TC 就至少是 O(n³)，肯定會 TLE。

其實這題並沒有那麼複雜，可以用 sliding window 維護兩個 pointer (left & right)，由左往右慢慢更新。並且把當前的 window 範圍內出現過的 character 紀錄在 set 裡面。

遇到沒看過的字元，就 right += 1。

遇到看過的字元，就持續 left +=1，直到當前的 s(left, right) 不包含重複的字元。

[leetcode/3-longest-substring-without-repeating-characters/1.cpp at main · flyotlin/leetcode
-------------------------------------------------------------------------------------------

### Contribute to flyotlin/leetcode development by creating an account on GitHub.

github.com

](https://github.com/flyotlin/leetcode/blob/main/3-longest-substring-without-repeating-characters/1.cpp?source=post_page-----14ac3154b289---------------------------------------)

Note
====

std::set
--------

```
// 初始化  
std::set<int> s{1, 3, 4};  
  
int arr\[\] = \[1, 3, 5, 5\];  
std::set<int> s(arr, arr+4);  // 從 c-style 的 array 初始化  
  
// 插入  
s.insert(1);  
  
// 查找  
s.count(1);  // 回傳純量，0 or 1  
s.find(1);  // 回傳 iterator，沒找到會回傳 s.end()  
  
// 刪除  
s.erase(1);  
  
// 清空  
s.clear();
```

std::unordered\_set
-------------------

Refs
====

*   [https://shengyu7697.github.io/std-set/](https://shengyu7697.github.io/std-set/)

> medium-to-markdown@0.0.3 convert
> node index.js https://medium.com/@flyotlin/leetcode-6-zigzag-conversion-70a90b71fb05

