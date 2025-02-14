---
title: "Go Hugo"
date: 2025-02-14T17:09:16+08:00
tags:
- tech
---
Go Hugo 是一個基於 Golang 產生靜態網站的 SSG (static site generation) 框架
## 基本概念

- site

一個 go hugo 產生的靜態網站
- content

網站中的靜態頁面、靜態內容
- configuration

設定 go hugo 產生的靜態網站 (site configuration)

## 重要操作
- create a site

`hugo new site`
- add content

`hugo new content <path-to-content>`，網站的主題 (theme) 通常會被放在 `/themes` 底下。

content 通常會用 [font matter](https://gohugo.io/content-management/front-matter/) 格式，將 metadata 加在 content 上
- start site

`hugo server` 可以啟動一個開發用的網站
- configure site

透過 site configuration 設定 hugo 產生的靜態網站
- publish

`hugo` 指令會將靜態網站 build 出來，將最終的靜態資源 (html, css, images, files) 放在 `/public` 資料夾中
