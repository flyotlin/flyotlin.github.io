---
title: Uva 10039 - Railroads
date: 2020-12-1T17:01:22+08:00
# tags: 
# - OJ解題 
# - Uva
# categories: 解題
---
## 題目敘述
這題輸入滿複雜的，分為三個部分，會先輸入有幾組測資。
* PART 1:
    城市名稱
* PART 2:
    列車資訊
* PART 3:
    出發時間 出發城市 抵達城市
    <!-- more -->
## 解題想法
這題第一眼看到以為是dfs+圖論，但後來看了別人的解法後才發現是dp。

* dp式子: **dp[抵達時間][抵達城市] = 最晚出發時間**

* 初始設定:
    1. 將整個dp array(最晚出發時間)設為-1，不能設為0(因為0代表00:00)
    2. 出發城市所有能到達的城市(k)，給定dp[抵達時間][k]的初始值。
* 轉移條件:
    **dp[i][j] = max(dp[i][j], 到城市的最晚出發時間)**

## 解釋
1. **G[startNum][i].startTime >= journey_startTime**

    要能從出發城市(s)到下個城市(i)的話，就是上面的式子，
    
    條件為列車從s出發的時間比題目給的開始旅行時間晚。

2. **max(dp[G[startNum][i].endTime][G[startNum][i].cityNo], G[startNum][i].startTime)**

    * G[startNum][i]: 為出發城市能到達的城市的節點
    * startTime: 列車出發的時間    
    * endTime: 抵達該城市的時間
    * cityNo:  該抵達城市的編號
  
    前面為原本的，後面是新的出發時間，**兩者取較晚的**。

3. **G[j][k].startTime >= i**
   
   * G[j][k].startTime 是到城市k的出發時間
   * i 是抵達城市j的時間
   
   **接著看看第四點**

4. **max(dp[G[j][k].endTime][G[j][k].cityNo], dp[i][j])**
   
   前者是原本的，後者為什麼是dp[i][j]，以及為什麼第三點的條件式這樣呢?

   請看看下面這張圖

   ![](https://imgur.com/1aV5sjb.png)

   所以dp[i][j]就是在某城市出發的時間，接著先到J，最後再到k。

   而要確保乘客能搭到這班車，條件就是由J往k的出發時間比i還晚，也就是第三點的條件。


  
參考: [Morris](https://mypaper.pchome.com.tw/zerojudge/post/1324398988)
## 程式碼
```cpp
#include <iostream>
#include <map>
#include <vector>
using namespace std;

struct edge 
{
    int cityNo, startTime, endTime;
    edge(int a, int b, int c):
        cityNo{a}, startTime{b}, endTime{c} {}
};

vector<edge> G[105];    // 最多100個城市
int dp[2400][105];      // 抵達時間/城市編號
int main()
{
    int test_cases, cases = 1;
    cin >> test_cases;
    int city_num;
    map<string, int> city_map;
    string tmp;
    while (test_cases--) {
        cin >> city_num;
        for (int i = 0; i < city_num; i++) {
            cin >> tmp;
            city_map[tmp] = i;
            G[i].clear();
        }
        int train_num;
        cin >> train_num;
        for (int i = 0; i < train_num; i++) {
            int stop_num;
            cin >> stop_num;
            int time;           // 輸入時的 時間
            string station;     // 輸入時的 車站
            int prev_time;      // 上次的 時間
            string prev_station;// 上次的 車站
            for (int j = 0; j < stop_num; j++) {
                cin >> time >> station;
                if (j != 0)
                    G[city_map[prev_station]].push_back(edge(city_map[station], prev_time, time));
                prev_station = station;
                prev_time = time;
            }
        }

        int journey_startTime;
        string startCity, endCity;
        cin >> journey_startTime >> startCity >> endCity;
        int startNum = city_map[startCity];
        int endNum = city_map[endCity];
        // Solve
        cout << "Scenario " << cases++ << endl;
        // // 初始dp
        for (int i = 0; i < 2400; i++)
            for (int j = 0; j < 105; j++)
                dp[i][j] = -1;  // 初始最晚出發時間為-1，0代表00:00(所以不能用)
        
        // 初始出發城市的dp狀態
        for (int i = 0; i < G[startNum].size(); i++) {
            // G[startNum][i]
            if (G[startNum][i].startTime >= journey_startTime)
                dp[G[startNum][i].endTime][G[startNum][i].cityNo] = 
                    max(dp[G[startNum][i].endTime][G[startNum][i].cityNo], G[startNum][i].startTime);
        }

        bool has_solution = false;
        for (int i = 0; i < 2400; i++) {    // 從00:00~24:00(不含)
            for (int j = 0; j < city_num; j++) { // 最多共city_num座城市
                if (dp[i][j] == -1)
                    continue;
                for (int k = 0; k < G[j].size(); k++) {
                    if (G[j][k].startTime >= i) {
                        dp[G[j][k].endTime][G[j][k].cityNo] = 
                            max(dp[G[j][k].endTime][G[j][k].cityNo], dp[i][j]);
                    }
                }
            }
            if (dp[i][endNum] != -1) {
                printf("Departure %04d ", dp[i][endNum]);
                cout << startCity << endl;
                printf("Arrival   %04d ", i);
                cout << endCity << endl;
                has_solution = true;
                break;
            }
        }
        if (!has_solution)
            puts("No connection");
        puts("");

    }
    return 0;
}
```