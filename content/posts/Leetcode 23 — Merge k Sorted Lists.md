---
title: "Leetcode 23 — Merge K Sorted Lists"
date: 2025-02-14T16:17:45+08:00
tags:
- 刷題
- Leetcode
---

接續前一天的策略，隨手從 Leetcode Linked List 題組中挑了一個 Hard 的題目。

之前做過 Merge two Sorted Lists，這題算是那一題的變形、Follow-up。

[https://leetcode.com/problems/merge-k-sorted-lists](https://leetcode.com/problems/merge-k-sorted-lists)

## Solution

看完題目後，偷瞄了一下 Topics 發現跟 heap、priority queue 有關。當下還沒仔細思考複雜度，但腦中瞬間有了一個解法。

建立一個 priority\_queue，遍歷所有 Linked Lists 中的 elements，並且加到 priority queue 裡面。

接著再依序取 priority queue 的 top（最小值）並且 pop，直到 queue 變成空的。

題目的限制是：

*   Linked List 的數量: k (0 ≤ k ≤ 10⁴)
*   Linked List 中元素個數: n (0 ≤ n ≤ 500)

最多有 10⁶ 個元素，時間複雜度會是 O(nk\*ln(nk))，大約需要 10⁶ \* ln10⁶ 次計算。

空間複雜度是 O(nk)，因為需要一個儲存所有元素的 priority queue。

不過，使用 priority queue 就沒辦法用到題目給的 sorted 的好處。AC 後看其他人的 solution 是 divide and conquer，就能利用到這個好處了。

## Note

### Priority Queue

是一個 heap 的結構，C++ STL (standard template library) 也有這個資料結構。

在 C++ STL 實作中，[priority queue](https://en.cppreference.com/w/cpp/container/priority_queue) 的 declaration 是：

```
template<  
    class T,  
    class Container = std::vector<T>,  
    class Compare = std::less<typename Container::value\_type>  
\> class priority\_queue;
```

預設會由大排到小，如果要由小到大，需要指定 Compare 為 `std::greater<int>`。  
**Compare return true，會越晚從 priority queue 被 pop 出來。**

值得注意的是，[STL container](https://en.cppreference.com/w/cpp/container) 有幾種類別：

*   sequence containers  
    如 array、vector、list、deque
*   associative containers  
    如 set、map
*   unordered associative containers  
    如 unordered\_set、unordered\_map

但，priority queue 其實並不屬於以上的任何一個類別。priority queue 是一個 container adaptor，用來**提供不同的 interface 給 sequence container**。

這也是為什麼在 template 的 declaration 中，第二個 template parameter 會是 container。

### Leetcode address sanitizer

第一次寫完 submit 的時候發現會一直出現以下的錯誤：

```
AddressSanitizer: attempting free on address which was not malloc()-ed
```

後來檢查了一下，在程式碼中有一個 `ListNode *head;`，沒有被初始化為一個 `ListNode` 的 object 或者是 `nullptr`。

## Conclusion

submit 出現的各種 error（如 `AddressSanitizer`）有空可以再更仔細的追究發生的原因。
