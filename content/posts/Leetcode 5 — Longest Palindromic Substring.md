---
title: "Leetcode 5 — Longest Palindromic Substring"
date: 2025-02-14T16:25:03+08:00
tags:
- 刷題
- Leetcode
---

前陣子在忙些雜事，刷題就荒廢了一陣子。途中還跑去學 Rust，之後可能會把 Rust book 重要的內容也整理一下，打成一篇筆記。

https://leetcode.com/problems/longest-palindromic-substring/

這題算標準的 Longest Palindromic Substring (最長回文子字串)，但很久沒寫這類題目，因此花了滿久卡在思考流程與邊界條件。
## Solution
### Brute Force

暴力 brute force 的想法是：

從長度為 1 的子字串枚舉到長度為 n 的子字串，遇到是回文且較長，就更新 global 最大值。

時間複雜度是 O(n³)，outer loop 是長度 1-n，inner loop 是枚舉該長度的所有字串 (n)，再加上檢查字串是否為回文。

[https://github.com/flyotlin/leetcode/blob/main/5-longest-palindromic-substring/3.cpp?source=post_page-----63aa3b4943e3---------------------------------------](https://github.com/flyotlin/leetcode/blob/main/5-longest-palindromic-substring/3.cpp?source=post_page-----63aa3b4943e3---------------------------------------)

brute force 雖然是 n³，但因為 input 限制字串長度為 1000，10⁹ 勉強能通過，不會 TLE。

但要注意 memory 使用，字串直接放在 stack 上傳，可能會造成 memory limit exceed。

把字串直接存在 heap 上就行了。
### DP

但仔細想想，這題目有點 dp (dynamic programming) 的味道。枚舉的過程中，短的字串會被包含在長的字串中。其實沒必要在看長的字串時，從頭開始檢查是否為回文。

可以用一個 array dp(i, j)紀錄 s(i, j) 是否為回文。根據題目要求，也可以變化為紀錄 s(i, j) 的最長子回文長度。

// 初始條件
dp(i, j) = false

// 更新
dp(i, j) = true, if dp(i+1, j-1) = true
         = false, else

但更新時要注意邊界，例如：

    i == j, => 0
    i > j, 這邊不會出現
    j-1 < 0, 特判

dp(i,j) 更新後，就可以更新 global 最大值。
### Manacher’s Algorithm

TBD
## Note
### 如何判斷回文？

solve 1:

利用兩個變數作為頭尾往中間靠攏，自己覺得比較直覺，不用思考字串奇偶長度的邊界問題。

bool isPalindrome(string s) {
 int l = 0;
 int r = s.size() - 1;

 while (l <= r) {
  if (s[l] != s[r]) {
   return false;
  }
  l++, r--;
 }
 return true;
}

solve 2:

出發點是「比較 size/2 次」，頭尾要自己計算，我覺得比較不直覺。

奇數的部分，for 的 end condition 有等於，會比較中間的 char，沒有等於就不會比較到。可以直接省略等於。

bool isPalindrome(string s) {
 for (int i = 0; i < s.size() / 2; i++) {
  if (s[i] != s[s.size() - i - 1]) {
   return false;
  }
 }
 return true;
}

### `memset()`

    declaration:

void *memset(void *ptr, int value, size_t n);

bool dp[100][100];
memset(dp, 5, sizeof(dp));

使用 memset() 需要 include string.h (C) 或 cstring (C++)，可以將 ptr 指向的記憶體區塊 (n 個 byte) 設為 value 。

memset() 的單位是 byte，因此只適用於 bool、char 之類大小為 1 byte 的型別。

像是 int 之類的型別 (4 bytes)，就不能用 memset() 來初始化。
## Refs

- https://web.ntnu.edu.tw/~algo/Palindrome.html
- https://leetcode.com/problems/longest-palindromic-substring/solutions/3202985/best-c-3-solution-dp-string-brute-force-optimize-one-stop-solution
- https://shengyu7697.github.io/cpp-memset/
