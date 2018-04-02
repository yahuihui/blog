---
title: Kubernetes
date: 2018-03-27 17:02:07
tags: Kubernetes、K8S
---
### Kubernetes 是什麼
Kubernetes (通常稱為K8s) 是自動化容器操作的開源平台，它建置在docker技術之上，用於自動部署、隨時擴展或收縮容器和管理容器化（containerized）應用程式的開源系統。

K8S屬分布式系統，主要元件有：
一、Master – 主要控制節點。包含以下元件：
    1.API Server：提供給外部使用者和內部元件呼叫。
    2.scheduler：負責各種排程作業。
    3.controller：負責執行各種控制。
二、. Node – 執行k8s的實體或虛擬的主機，並運行了許多容器。包含以下元件：
    1.kubelet：負責Node節點上pod的創建、修改、監控、刪除等全生命週期的管理
    2.kubernetes proxy：Proxy是為了解決外部網絡能夠訪問跨機器集群中容器提供的應用服務而設計的。
    3.docker：node是容器執行節點，需要執行docker服務
管理pod、檢查容器是否正常運行、監控所有節點的資源使用狀況
在 kubernetes 的設定中，最基本的管理单位是pod，而不是 container。pod是許多容器的組合。
![pod](pod.jpg "pod VS container")

Kubernetes中所有的配置都是通过API对象的spec去设置的s
kubernates 是使用etcd做為系統的設定儲存中心、重要資料都是持久化在etcd中的。


kubectl是和Kubernetes API交互的命令行程序

k8s使用etcd作為儲存中介軟體


### K8S建置教學 (以三台環境建置為例)
環境需3台。一台當master，二台當node

「master、node」登入後切換至最高權限 sudo su 
![K8S](0.jpg "sudo su")
執行apt-get update 
執行apt-get upgrade -y
執行apt-get dist-upgrade -y
安裝docker：apt-get install –y docker.io 
安裝kubelet kubeadm kubectl ：apt-get install -y kubelet kubeadm kubectl 
kubeadm 管理k8s套件，依照文件照操作，如下圖
![kubeadm官方文件](kubeadm.jpg "kubeadm官方文件")
![安裝kubeadm](001.jpg "安裝kubeadm")
![安裝kubeadm](002.jpg "安裝kubeadm")
![安裝kubeadm](003.jpg "安裝kubeadm")
![安裝kubeadm](004.jpg "安裝kubeadm")
![安裝kubeadm](005.jpg "安裝kubeadm")
![安裝kubeadm](006.jpg "安裝kubeadm")

「master、node」關閉SWAP，因為SWAP開著會無法安裝K8S
執行swapoff –a 
![關閉SWAP](swapoff.jpg "關閉SWAPS")

安裝完kubeadm之後進行初始化和架設網路，網路是使用fiannl來建置。
「master」 kubeadm初始化文件
![kubeadm初始化文件](screenshot_002.jpg "kubeadm初始化文件")
執行kubeadm init --pod-network-cidr=10.244.0.0/16 
![kubeadm初始化](screenshot_003.jpg "kubeadm初始化")
![kubeadm初始化](screenshot_004.jpg "kubeadm初始化")

「master」kubeadm架設網路
執行sysctl net.bridge.bridge-nf-call-iptables=1
「master」建置資料夾、COYP檔案、修改權限
執行mkdir -p $HOME/.kube 
執行sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config 
執行sudo chown $(id -u):$(id -g) $HOME/.kube/config
![架設網路、建置資料夾、COYP檔案、修改權限](screenshot_006.jpg "架設網路、建置資料夾、COYP檔案、修改權限")
執行完後會出現join node，綠框的金鑰複製下來。

「node」 join node，貼上金鑰讓node加入到master，即完成cluster
![node1 join](screenshot_007.jpg "node1 join")
![node2 join](screenshot_008.jpg "node2 join")

「master」真正分配網路
![分配網路文件](screenshot_009.jpg "分配網路文件")
![分配網路](screenshot_010.jpg "分配網路")

「master」檢查 DNS 狀態狀態
執行kubectl get pods –namespace kube-system 
剛開始 dns 還是 pending 狀態，等過一段時間後就會是 running 狀態
![檢查 DNS 狀態狀態](screenshot_011.jpg "檢查 DNS 狀態狀態")
以上k8s建置完畢

### 測試cluster，以nginx為例

建置nginx pods狀態，nginx的狀態要在running
執行kubectl run nginx --image=nginx --replicas=2 --port=80 
![建置nginx](screenshot_012.jpg "建置ngin台")
查詢pods
![查詢pods](screenshot_013.jpg "查詢pods")
開始部署
![開始部署](screenshot_014.jpg "開始部署")
開服務使用的port
![開服務使用的port](screenshot_015.jpg "開服務使用的port")
查詢服務狀態
![查詢服務狀態](screenshot_016.jpg "查詢服務狀態")
 網頁測試
![ 網頁測試](screenshot_017.jpg " 網頁測試")


----------------------------------------
如需重建，整組刪掉後再進行kubectl init
![kubeamd reset 刪除](screenshot_001.jpg "kubeamd reset 刪除")


参考文獻
<a href="https://kubernetes.io/docs/setup/independent/install-kubeadm/">1.https://kubernetes.io/docs/setup/independent/install-kubeadm/</a>
<a href="https://jimmysong.io/kubernetes-handbook/concepts/concepts.html">2.https://jimmysong.io/kubernetes-handbook/concepts/concepts.html</a>
<a herf="http://cizixs.com/2016/10/25/kubernetes-intro-kubelet">3.http://cizixs.com/2016/10/25/kubernetes-intro-kubelet </a>