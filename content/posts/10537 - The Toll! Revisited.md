---
title: 10537 - The Toll! Revisited
date: 2020-12-18T17:01:22+08:00
tags: 
- OJ解題 
- Uva
categories: 解題
---
## 解題想法
這題用的演算法主要是dijkstra，求權重圖的最短路徑，但不能有負邊。
<!-- more -->

由於題目給的是到終點時的貨物數量，要我們求起點出發要有多少貨物。
所以在這邊我們選擇從終點反向計算到各點的single-source shortest path。最後輸出終點到起點的shortest path。

這題邊的權重可以看做是過路費，進到鄉村只收1單位貨物，進到城市則是每20個貨物收1單位貨物的過路費。

因此dijkstra在進行relaxation的時候，要看該點權重加上進入該點的過路費，有沒有小於下個點權重，小於的話就更新下個點的shortest path。
![](https://imgur.com/Cz4b9Qe.png)
> 上圖由A點更新其他點，更新後各點的狀況。

另外，題目還需要輸出經過的地方。這個只需要用一個陣列pre[]來記錄他的上個點是誰即可。

## 參考
我在學dijkstra的時候，看的是這位印度帥哥的教學影片。大推!
[Dijkstra - Abdul Bari](https://www.youtube.com/watch?v=XB4MIexjvY0&t=1s)
## 程式碼
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <cmath>
using namespace std;
// 將abc轉為0~51的編碼
int decode(char x)
{
    if (x >= 65 && x <= 90)
        return x-65;
    if (x >= 97 && x <= 122)
        return x-71;
    return 0;
}
// 將0~51的編碼轉回abc
int encode(int x)
{
    if (x >= 0 && x <= 25)
        return x + 65;
    if (x >= 26 && x <= 51)
        return x + 71;
    return 0;
}

long long INF = 0x7fffffffffff;     // 無限大
int cases = 1;  // 紀錄目前為第幾個case

int main()
{
    int path_num;
    char a, b, s, e;
    int aa, bb, start, end, item_num;
    while (cin >> path_num && path_num != -1) {
        vector<int> map[60];
        for (int i = 0; i < path_num; i++) {
            cin >> a >> b;
            aa = decode(a);
            bb = decode(b);
            // 建圖
            map[aa].push_back(bb);
            map[bb].push_back(aa);
        }
        cin >> item_num >> s >> e;    // 輸入題目問題
        start = decode(s);
        end = decode(e);
        // 由end開始進行反向dijkstra
        int pre[60];        // 該點的前一個點 
        bool visit[60];     // 是否造訪過
        long long d[60];    // 該點的最短路徑值
        // 初始化
        for (int i = 0; i < 60; i++) {
            pre[i] = -1;
            visit[i] = false;
            d[i] = INF;
        }

        queue<int> q;
        q.push(end);
        visit[end] = true;
        d[end] = item_num;

        int front, current, smaller_no;
        long long toll, smaller;
        while (!q.empty()) {
            front = q.front();
            q.pop();
            // 計算過路費
            if (front >= 0 && front <= 25)  // 城市
                toll = ceil(d[front] * 20.0 / 19.0);
            else    // 鄉村
                toll = d[front] + 1;
            // 進行dijkstra relaxation
            for (int i = 0; i < map[front].size(); i++) {
                current = map[front][i];
                if (toll < d[current]) {
                    d[current] = toll;
                    pre[current] = front;
                }
            }
            // 挑選目前圖中，未造訪且權重最小的點
            smaller = INF;
            for (int i = 0; i < 52; i++)
                if (visit[i] == false && d[i] < smaller) {
                    smaller = d[i];
                    smaller_no = i;
                }
            // 將點放到queue裡面
            if (smaller < INF) {
                q.push(smaller_no);
                visit[smaller_no] = true;
            }
        }
        // 輸出答案
        cout << "Case " << cases++ << ":" << endl;
        cout << d[start] << endl;
        current = start;
        while (current != end) {
            cout << (char)encode(current) << "-";
            current = pre[current];
        }
        cout << (char)encode(current) << endl;
    }
    return 0;
}
```