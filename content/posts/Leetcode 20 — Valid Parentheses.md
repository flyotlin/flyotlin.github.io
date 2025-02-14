---
title: "Leetcode 20 — Valid Parentheses"
date: 2025-02-14T16:24:13+08:00
tags:
- 刷題
- Leetcode
---

這幾天會找一些 stack、queue 的題目練習，主要圍繞在 C++ 的語法複習，以及基礎資料結構的熟悉。

https://leetcode.com/problems/valid-parentheses
## Solution

標準的可以利用 stack FILO 特性的題目。

解法就是單純的依序讀取字串，遇到的左括號放進 stack 中，遇到右括號就開始比對 stack top。

如果跟 stack top 成對，就 pop stack；反之，則代表這是個不合法的 parentheses string。
## Note
### Stack

    特性：FILO
    可以用 array 或 linked list 實作
    時間複雜度
    插入：O(1)、搜尋：O(n)
    C++ STL container 以 sequence adapter 的方式實作
    對 stack 操作前，要檢查 stack 是否是 empty。
    否則會因為 stack 預設的 container 是以 linked list 實作的 deque，而踩到 SEGV (segmentation violation)

### Iterator

“Iterate” is a technical term for looping.

An iterator is a pointer to the element inside a data structure.

Though underlying data access mechanism is different, iterator provides a uniform interface to access data from either sequential or associative container.
How to use Iterator?

Take vector for example:

vector<int> vec = {1, 2, 3};
vector<int>::iterator it;

// forward
for (it = vec.begin(); it != vec.end(); it++) {
  cout << *it << endl;
}

// backward (reverse)
for (it = vec.rbegin(); it != vec.rend(); it++) {
  cout << *it << endl;
}

Note that vec.end() points to one position after the last element.
Conclusion

有空可以再手刻以 array 和 linked list 實作的 stack，而非直接使用 STL library 的 stack。
## Reference

    https://web.ntnu.edu.tw/~algo/Data.html