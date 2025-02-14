---
title: "Leetcode 11 — Container With Most Water"
date: 2025-02-14T16:07:01+08:00
tags:
- 刷題
- Leetcode
---

最近開始刷題，先從基本的 data structure 開始複習。策略是主題式的複習，這幾天的目標是 array、linked list 之類的資料結構，因此從 leetcode array 類別中選到了這一題。

[https://leetcode.com/problems/container-with-most-water](https://leetcode.com/problems/container-with-most-water)

## Naive Solution

這題乍看之下的 naive solution 是 O(n²)，把所有可能的長寬組合都暴力嘗試過一遍，Pseudocode 如下：

```=C++
max_area = 0
for i = 1 ... n
  for j = i+1 ... n
    max_area = max(max_area, (j-i) * min(height[i], height[j]))
```

不過仔細觀察題目的限制 (constraints)，2 ≤ n ≤ 10⁵，以這種 O(n²) 的解法，複雜度會變成 10¹⁰，超過一般 CPU 可以在時間範圍內處理完的指令數。

印象中一般在解這種題目的最大可接受複雜度是 10⁸ ~ 10⁹，超過的話通常會 TLE。（有錯請糾正我，知道 10⁸ 怎麼導出來的也可以告訴我，太久沒寫這類題目）

## Real Solution

這題其實可以利用 two pointer 的方法解決。

two pointer 通常會用在 sorted array 或 sorted linked list 上，將 left、right pointer 分別設在左右兩個端點，利用 sorted 的特性，逐漸往中心收攏，探索最大的可能性，個人覺得有種 greedy 的味道。

這題雖然沒有 sorted array，但有個特性是最大的底 * 最大的高，會得到最大的面積。

一開始我們無法得知最大的高，因此可以先從最大的底探索，也就是把 two pointer 設在左右端點。

接著，逐漸往中心靠攏，在過程中把較短的邊捨棄掉，探索最大的高。

如此一來，就能將所有可能的最大面積都探索過一次。

## Bonus

### Min & Max

在過程中用到 C++ 的 max 跟 min，就順手看了一下 cppreference 的介紹。

如果 a, b 是一樣的 (equivalent)，會直接回傳 a。

除了最基本的 max(a, b) 這樣的用法， max() 還有多個 overloaded 的實作。

其中一種實作可以傳入 comp，改掉預設的 compare 行為。max 預設是 <，min 預設是 >，也可以自己傳入一個 binary function。
我想這個設計，是為了支援 object 之間的比較。因為預設的 >、< 只能比較 scalar，除非 object 的 class 有做 operator overloading。

另一種實作可以傳入 il (initializer list)，讓 max、min 可以從一個 list 中找出最大、最小值。

另外，max 跟 min 為了支援多種 data type，將資料型態從實作中抽離，背後是透過 C++ 的 template 實作的。

### Initializer List

根據 cppreference，initializer list 的 declaration 是 a list of elements of type const T 。

可以透過 std::initializer_list 來定義

[std::initializer_list](https://cplusplus.com/reference/initializer_list/initializer_list/?source=post_page-----7ac4d9203d44---------------------------------------)

## Conclusion

寫題目前要把題目的 constraints 看好，把時間、空間複雜度估好，不然很容易一 submit 就 TLE。