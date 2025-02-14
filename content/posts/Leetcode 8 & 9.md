---
title: "Leetcode 8 & 9"
date: 2025-02-14T16:26:28+08:00
tags:
- 刷題
- Leetcode
---

這兩題都算滿簡單的，所以放在一起講。

https://leetcode.com/problems/string-to-integer-atoi

https://leetcode.com/problems/palindrome-number/
## Solution
### Leetcode 8 — string to integer atoi

這題只有一些邊界問題要討論，其實挺簡單的。

邊界問題是 integer (int) 是否會 overflow，跟昨天的 Leetcode 7 有點像。

把數字字元加到 int 變數 n 時，正負數會分開處理。也就是正數用 + 的，負數用 - 的。

判斷 overflow 則會分別判 INT_MAX 和 INT_MIN，但因為 res 已經是正負數，所以如果 overflow 發生，就直接把最大最小的邊界值給 res 就好。

如果最後才考慮正負，res 恆為正，會發生 INT_MIN 沒辦法被表示在 res 裡面。
### Leetcode 9 — Palindrome number

naive solution 是把數字轉字串，再檢查該字串是否為回文字串。

follow up 則是不轉成字串，嘗試直接檢查該數字是否為回文。