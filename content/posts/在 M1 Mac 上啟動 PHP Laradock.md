---
title: "在 M1 Mac 上啟動 PHP Laradock"
date: 2022-05-15T17:01:22+08:00
# draft: true
---
校內工讀的伺服器上部署了 3-4 個不同的網站，好在當初有使用 PHP 的容器化解決方案 - Laradock，能夠以容器化的方式使用多個服務。

這次遇到的問題是 MySQL 對 arm64 (M1 Mac) 的支援度並不好，再加上沒有將前一次 Laradock 啟動後產生的 MySQL 檔案清乾淨，導致 local 啟動時踩了不少坑。

接下來會直接簡述如何確保 M1 Mac 能順利啟動包含 MySQL 服務的 Laradock，最後才是 tedious 的 debug 過程。

## How to Run Laradock with MySQL on M1 Mac
1. Laradock `.env` 中的 `MYSQL_VERSION` 要指定為 arm64 版本的 image，如 `8.0.29-oracle`。
2. 確保 `~/.laradock/data/` 中沒有 mysql 資料夾，否則會有存取問題。

## MySQL support for ARM
一開始 Laradock build nginx, MySQL 時就會出現錯誤，原因是 MySQL latest image 並不支援 arm64 平台。

稍微 google 後，許多 2021 年初的文章推薦直接在 pull image 時指定 platform 為 `linux/x86_64`。

也發現其實 [MySQL 開始支援 arm 平台](https://hub.docker.com/r/arm64v8/mysql/)，但需要另外指定對應的 tag。

目前以 MySQL 8.0 以上帶有 `oracle` 後綴的 tag 為主。

## Weird MySQL Errors
Google 了一下並經過思考後，我決定使用 arm64 版本的 MySQL image。但這也是一連串奇怪錯誤的開始...

### Failed to Run MySQL
第一個是比較容易理解的錯誤，由於 Laradock 中的 MySQL 沒有順利啟動，其他依賴資料庫的 web 服務 (laradock) 當然也沒好受。

最直接的錯誤是：

`php_network_getaddresses: getaddrinfo failed: Temporary failure in name resolution`

很明顯問題是 MySQL 啟動失敗，web 服務沒辦法存取 MySQL。

會出現 name resolution 錯誤是因為 Laradock 各個服務透過 `docker-compose` 啟動，彼此透過 service 名稱連線。

MySQL 沒啟動，當然沒辦法將 MySQL service name (**mysql**) 解析成該容器對應的 private ip。

### Laradock 的大坑
發現是 MySQL 的錯誤後，緊接著檢查了 container logs 的錯誤訊息，分別發現了 2 個奇怪的錯誤。

1. `Linux Native AIO interface is not supported on this platform`

2. `chown: cannot read directory '/var/lib/mysql/': Permission denied`

找了半天，在 `docker-compose.yml` 中看到了蛛絲馬跡。

```
// from .env
DATA_PATH_HOST=~/.laradock/data

// from docker-compose.yml
mysql:
    build:
        context: ./mysql
        args:
          - MYSQL_VERSION=${MYSQL_VERSION}
    environment:
        - MYSQL_DATABASE=${MYSQL_DATABASE}
        - MYSQL_USER=${MYSQL_USER}
        - MYSQL_PASSWORD=${MYSQL_PASSWORD}
        - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
        - TZ=${WORKSPACE_TIMEZONE}
    volumes:
        - ${DATA_PATH_HOST}/mysql:/var/lib/mysql            // 元兇！
        - ${MYSQL_ENTRYPOINT_INITDB}:/docker-entrypoint-initdb.d
    ports:
        - "${MYSQL_PORT}:3306"
    networks:
        - backend
```

原來 Laradock build MySQL 時會把 MySQL 相關設定 mount 到 `~/.laradock/data/mysql`，進而導致後續一連串的奇怪問題。
