---
title: "Git Submodules"
date: 2025-02-14T16:53:14+08:00
tags:
- tech
---

## git submodules
當你的專案需要依賴於外部的 git repo、library 時，可以透過 git submodule 將該外部依賴引入至你的專案中。

git submodule 會將外部依賴的某個 commit 引入到你的 repo 中。

## How to use?
### 查看 submodule
`git submodule status`
### 初始化 submodule
當你 clone 一個有 submodule 的專案時，需要先透過 `git submodule init` 初始化被定義在 `.gitsubmodules` 中的外部依賴。

接著，在透過 `git submodule update` 更新 submodule。

以上兩個步驟可以透過 `git submodule update --init --recursive` 合併在一起。
### 新增 submodule
透過指令 `git submodule add <repository_url> <submodule directory path>`，將某個外部 repo 從指定的 url clone 到指定的 path。
### 更新 submodule
可以直接在 submodule 的資料夾變更檔案、commit 改動、push 到遠端 repo，你的 repo 會記錄目前 repo 正在使用的 submodule 的 commit。
### 刪除 submodule
執行 `git rm <path-to-submodule>`，再 commit 改動到遠端 repo。

## Ref
- [Perplexity thread](https://www.perplexity.ai/search/tell-and-teach-me-what-is-and-VjYKAj52QHqCyLDuCKfiuA)
- https://stackoverflow.com/questions/1260748/how-do-i-remove-a-submodule
