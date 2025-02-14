---
title: "Leetcode 6 — Zigzag Conversion"
date: 2025-02-14T16:25:38+08:00
tags:
- 刷題
- Leetcode
---

想維持解題的規律跟手感，但最近實驗室還是會有些雜事 block 住，只能寫多少算多少了。

目前還沒很進入找工作的狀態，可能是還有碩論跟雜事要忙，沒辦法全神貫注地投入。

https://leetcode.com/problems/zigzag-conversion/
## Solution

這題其實沒什麼演算法，解法有以下兩種：
### 1. 找出數學的規律

依序 concat 每個 zigzag row 的字元，要找出每個 row 的每個字元之間 index 的間隔。
以 PAYPALISHIRING，3 列為例，zigzag 字串會長成：

P.  A.  H.  N
A P L S I I G
Y.  I   R

第一列的 index 是 0, 4, 8, …，與最後一列的規律一樣，都是 stride = 4，其中stride=2*row-2 。

中間的列還要考慮會有其他的字元，所以 stride 不是單純的 2*row-2。規律會變成 (stride-2*i, 2 )，i 是當前的 row number。

時間複雜度: O(n)

空間複雜度: O(1)
### 2. 用 2D array 儲存 zigzag 字串

先建立一個 2D 的 array，接著把 string 依照 zigzag 順序放到 2D array 中。最後再 row-by-row 地把字串印出來。

時間複雜度: O(n)

空間複雜度: O(n²)

第二個解法的好處是想法單純，但當初第一次看到這題目並沒想到這想法，而是想用第一種解法，找出數學規律。
## Note
### `std::unordered_set`

底層是用 hash table 實作的，跟 std::set 的實作不同，std::set 底層是紅黑樹 （self balance binary tree）。

set 是個 associative container (關聯式容器)，裡面只能儲存不重複的元素。

如名字所述，一個有排序，一個沒有排序。排序指的是用 iterator 遍歷該容器時，是否會有序的遍歷容器內的元素。

    unordered_set
    插入、查詢、刪除：O(1)
    空間：O(n)
    碰撞發生時，插入的 worst case time complexity 為 O(n)
    set
    插入、刪除：avg O(1), worst O(logn)
    查詢：O(logn)
    空間：O(n)

#include <iostream>
// 需要引入標頭檔 (header file)
#include <unordered_set>
#include <set>

using namespace std;

int main() {
    set<int> s;
    // unordered_set<int> s;
    s.insert(1);
    s.insert(5);
    s.insert(2);

    for (auto it = s.begin(); it != s.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

### hash table

能快速地將 key 對應到 value，屬於一種關聯式陣列 associative array。

內部透過 hash function，將 key 轉換成 hash table 的 index，再從 table 的 index 把 value 取出。

給定不同的 value，hash function 有可能計算出相同的 index，這種情況被稱作是 hash collision (碰撞)。

關聯式陣列指的是有鍵值對應 (key-value) 的資料結構，又被稱為 map (對映) 或 dictionary (字典)。
## Ref

    https://shengyu7697.github.io/std-unordered_set/
    https://github.com/bradtraversy/traversy-js-challenges/blob/main/06-hash-tables-maps-sets/02-hash-table-intro/readme.md