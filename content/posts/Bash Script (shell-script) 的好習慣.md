---
title: "Bash Script 的好習慣"
date: 2023-07-16T21:32:03+08:00
---

bash script 前面沒有特別設定的話，執行時遇到錯誤，通常會繼續往下執行，而不會像平常熟悉的程式語言遇到 exception 時（沒有 error handling）直接跳出結束執行。

另外，在開發、撰寫 bash script 時，也時常有需要 debug 的時候。

因此，以下簡單介紹一些常用且有幫助的 bash script 前置設定。
> 以下設定只在 bash 上測試過，其他 shell 不一定能使用。

## set -x
在 bash script 開發過程中，如果需要 debug 可以在 script 前面加上 `set -x`。行為是 script 在執行時，執行每一行指令前，會先把指令印出來。

雖然不像一些高階程式語言有 debugger 能設中斷點、直接觀察變數值以及 stack 狀況，但對於有許多變數、迴圈、邏輯判斷的複雜 script 來說，透過 print-debug like 的方式來找碴也十分有幫助。

## set -eou pipefail
bash script 執行時遇到錯誤，以及遇到 unset variable 時，預設不會中斷而是會繼續執行。面對這種狀況，能在 script 前加上 `set -eou pipefail` 解決以上問題。

### Arguments
-e: 任何指令執行失敗會讓整個script跳出，return value非零。

-u: unset variables would exit the script.

-o pipefail: pipeline會把最後一個指令當作return value。

使用此option後只要pipeline中任一指令失敗就會回傳非零。

不過，要搭配-e才能避免失敗後繼續往下執行。

### Test
-e:
```
#!/bin/bash

set -e      # 請自行移除觀察行為有何差別

ls not-exist
echo "HI"
```
加上 `set -e` 後就不會印出 HI，另外，`echo $?` 觀察 return value 也從 0 變成 1

-u:
```
#!/bin/bash

set -u      # 請自行移除觀察行為有何差別

echo "$notset"
```
加上 `set -u` 後第 5 行會報錯，另外，`echo $?` 觀察 return value 也從 0 變成 1

-o pipefail
```
#!/bin/bash

set -o pipefail     # 請自行移除觀察行為有何差別

ls not-exist | echo "HI"
```
加上 `set -o pipefail` 後 **依然會繼續執行 pipeline 剩餘指令**，因此能看到 stdout 印出 HI。

原本 script 會把 pipeline 最後一個指令的 return 當作 return value，但加上此 option 後，只要 pipeline 中任何一個指令失敗，return 

value 就是 1，因此觀察到 return value 從 0 變成 1。

```
#!/bin/bash

set -o pipefail     # 請自行移除觀察行為有何差別

echo "HI" | ls not-exist
```
這個範例無論有沒有加 `set -o pipefail`，return value 都是 1，原因與上面提到 pipeline 的 return value 機制有關。

## 範例 Example sciprt
```
#!/bin/bash

set -eou pipefail
set -x

# your script...
```

## 結語

之後有遇到其他好用的設定會再慢慢更新上來...
