---
title: harbor
date: 2018-04-03 17:34:00
tags:
---
## 什么是 Harbor？
Harbor是VMware公司開源的企業級DockerRegistry項目
Harbor是一個用於存儲和分發的Docker鏡像的企業級Registry服務器
Harbor提供更好的性能和安全性。使註冊表更接近構建和運行環境可提高圖像傳輸效率。
Harbor與現有的企業LDAP / AD集成，用於用戶驗證和管理。


## Harbor建置教學

![](.jpg " ")
### 環境準備
Ubuntu 16.04 LTS
Python 2.7 ↑
Docker 1.10 ↑
Docker-compose 1.6.0 ↑  
Docker Compose依靠Docker Engine進行任何有意義的工作。ex：Compose使用Docker標籤來追踪容器

### 環境安裝作業
確認VM版本 Check Version
![check version](version.jpg "cherck version")
執行apt-get update 和apt-get upgrade
![apt-get update](update.jpg "apt-get update")
![apt-get upgrade](upgrade.jpg "apt-get upgrade")
安裝python與驗證python版本
![安裝與驗證python](install_python_version.jpg "安裝與驗證python")
安裝docker
![安裝docker](install_docker.jpg "安裝docker")
驗證docker版本
![驗證docker版本](docker_version.jpg "驗證docker版本")
安裝Docker Compose
![安裝compose](compose.jpg "安裝compose")
編輯compose權限 與 驗證Docker Compose
![編輯與驗證compose](compose_version.jpg "編輯與驗證compose")
openssl.cnf內的[v3_ca]加入倉庫IP
![加入倉庫IP](add harbor ip.jpg "加入倉庫IP")

### 安裝Harbor
下載版本v1.1.1
wget https://github.com/vmware/harbor/releases/download/v1.1.1/harbor-online-installer-v1.1.1.tgz
![下載harbor](download harbor.jpg "下載harbor")
解壓縮檔案
![解壓縮](unzip.jpg "解壓縮")


