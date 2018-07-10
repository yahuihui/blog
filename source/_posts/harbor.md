---
title: harbor
date: 2018-04-03 17:34:00
tags:
---
![harbor](harbor.jpg " harbor")
## 什麼是 Harbor？？

Harbor是VMware公司開源的企業級DockerRegistry項目，Docker Registry 是被用來儲存 Docker 所建立的映像檔的地方，我們可以把自己建立的映像檔透過上傳到 Registries 來分享給其他人
Harbor是一個用於存儲和分發的Docker鏡像的企業級Registry服務器
Harbor提供更好的性能、安全性、UI介面，使用者可以輕鬆的瀏覽、搜索鏡像倉庫以及對項目進行管理。
Harbor與現有的企業LDAP / AD集成，用於用戶驗證和管理。



## Harbor建置教學

### 環境準備
Ubuntu 16.04 LTS
Python 2.7 ↑
Docker 1.10 ↑
Docker-compose 1.6.0 ↑  
Docker Compose依靠Docker Engine進行任何有意義的工作。ex：Compose使用Docker標籤來追踪容器。

### 環境安裝作業
確認VM版本 Check Version
![check VM version](version.jpg "cherck VM version")
執行apt-get update
![apt-get update](update.jpg "apt-get update")
執行apt-get upgrade
![apt-get upgrade](upgrade.jpg "apt-get upgrade")
安裝python
![安裝python](install_python.jpg "安裝python")
驗證python版本
![驗證python](python_version.jpg "驗證python")
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
一、下載版本v1.1.1
wget https://github.com/vmware/harbor/releases/download/v1.1.1/harbor-online-installer-v1.1.1.tgz
![下載harbor](download harbor.jpg "下載harbor")
解壓縮檔案
![解壓縮](unzip.jpg "解壓縮")

二、修改Harbor config
vim haobor.cfg
![進入harbor.cfg](harbor_cfg.jpg "進入harbor.cfg")
![修改harbor.cfg](fix_harbor_cfg.jpg "修改harbor.cfg")

-------
hostname = reg.mydomain.com
→ hostname = harbor._______.xsg

 ui_url_protocol = http
→ ui_url_protocol = https

 ssl_cert = /data/cert/server.crt
→ ssl_cert = /home/server/harbor/cert/_____.xsg.crt

 ssl_cert_key = /data/cert/server.key
→ ssl_cert_key = /home/server/harbor/cert/_____.xsg.key

-------


三、harbor安裝
sudo ./install.sh
![install_harbor](install_harbor.jpg "install_harbor")

四、在harbor倉庫下檢查容器
sudo docker-compose top

五、新增DNS
![新增dns設定](dns1.jpg "新增dns設定")
![進入dns設定](dns2.jpg "進入dns設定")
![新增dns設定](dns3.jpg "新增dns設定")

六、harbor web登入
https://harbor.atcity.xsg/
帳號：admin
密碼：Harbor12345(預設)

### 安裝 Certificate
在harbor下產生cert資料夾，可利用windows產生憑證後，改製作成.crt(CERTIFICATE) 和.key  (PRIVATE KEY) 檔案 
使用xftp上傳，或是產空白檔貼上憑證

因config預設的憑證路徑是在/data/cert/，以免後續改用LDAP需重啟服務清掉data，憑證會被刪掉
所以把憑證改設定在 
/home/server/harbor/cert/atcity.xsg.crt
/home/server/harbor/cert/atcity.xsg.key

mkdir cert
cp /date/cert/atcity.xsg.crt /home/server/harbor/cert/
cp /date/cert/atcity.xsg.key /home/server/harbor/cert/


### LADP設定
修改Harbor LDAP config
vim haobor.cfg
![進入harbor.cfg](harbor_cfg.jpg "進入harbor.cfg")
![修改LDAP設定](fix_LDAB_harbor_cfg.jpg "修改LDAP設定")

-------
auth_mode = db_auth
→ auth_mode = ldap_auth

ldap_url = ldaps://ldap.mydomain.com
→ ldap_url = ldap://t0ldap.gosmio.biz:3268

ldap_searchdn = uid=searchuser,ou=people,dc=mydomain,dc=com
→ ldap_searchdn = CN=ittestuser,OU=PublicID,OU=Account,DC=gosmio,DC=biz

ldap_search_pwd = password
→ ldap_search_pwd = a123456A

ldap_basedn = ou=people,dc=mydomain,dc=com
→ ldap_basedn = dc=gosmio,dc=biz

ldap_uid = uid
→ ldap_uid = sAMAccountName

-----

重啟服務並強制清除data目錄下資料
    docker-compose down -v
    rm -rf /data
    ./prepare
    docker-compose up -d
![docker-compose down -v](docker-compose down -v.jpg "docker-compose down -v")


測試網頁使用LDAP登入
https://harbor.atcity.xsg/

admin / Harbor12345
![登入](login.jpg "登入")
![登入成功](login_admin.jpg "登入成功")

調整Configuration設定
![調整LDAP](LDAP_Configuration.jpg "調整LDAP")

測試網頁登入
![登入成功](login_LDAP.jpg "登入成功")