---
title: Kubernetes
date: 2018-03-27 17:02:07
tags: Kubernetes、K8S
---
![k8s](k8slogo.jpg "k8s")
### 什麼是 Kubernetes

Kubernetes(通常稱為K8s)是 Google 團隊開發的開源項目，它的目標是管理跨多個主机的容器平台，它建置在docker技術之上，用於自動部署、隨時擴展或收縮容器和管理容器化（containerized）應用程式的開源系統。
簡言之，以更高效的方式自動化跨群集的應用程序容器的分發和調度。

![k8s_cluster](k8s_cluster.jpg "k8s_cluster")
![K8S_nodes](k8s_nodes.jpg "k8s_nodes.jpg")
主服務器(master)管理集群，節點(nodes)用於管理正在運行的應用程序。

在 kubernetes 的設定中，最基本的管理单位是pod。pod是一個或多個容器的組合，容器才是真正的執行個體。
Pod中的每個容器都共享網路，包括IP地址和網路端口
![pod](pod.jpg "pod VS container")


K8S屬分布式系統，主要元件有：
一、Master – 主服務器(master)管理集群。包含以下元件：
    1.API Server：是整個系统的對外接口，提供給外部使用者和內部元件呼叫。
    2.scheduler：負責各種排程作業、對資源進行調度，分配某個pod 到某個節點node上。
    3.controller：負責執行各種控制。
二、Node – 執行k8s的實體或虛擬的主機，並運行了許多容器。包含以下元件：
    1.kubelet：負責Kubernetes Master和Node之間的通信的過程; 它管理機器上運行的Pod和容器。
    2.kubernetes proxy：Proxy是為了解決外部網絡能夠訪問跨機器集群中容器提供的應用服務。
    3.docker：管理pod、檢查容器是否正常運行、監控所有節點的資源使用狀況，是Pod中使用的最常見的容器運行
![k8s基本概念](k8s1.jpg "k8s基本概念")

### K8S 架構
![k8s_architecture](k8s_architecture.jpg "k8s_architecture")

這些節點使用主機公開的Kubernetes API與主機進行通信。

Kubernetes中所有的配置都是通过API对象的spec去设置的
kubernates是使用etcd做為系統的設定儲存中心、重要資料都是持久化在etcd中的。
kubectl是和Kubernetes API交互的命令行程序
kubectl是用於針對Kubernetes群集運行命令的命令行界面, 類似docker run的功能

*
RPC(Remote Procedure Call)遠端程序呼叫是一個電腦通訊協定。該協定允許執行於一台電腦的程式呼叫另一台電腦的子程式
GRPC 是Google提供的一套RPC函式庫，需要Go version 1.5 以上的版本才支援。

protobuf (Protocol Buffers) 是由 Google 所推出的一格式，可以把它想像成是 XML 或 JSON 格式，但是更小、更快，而且更簡潔。這能夠幫你節省網路與硬體資源，且只需要定義一次資料結構，接著就會自動生成符合你程式語言的檔案，讓你能夠直接在你的程式上使用。

Kubernetes集群内部存在三個IP，分别是：
Node IP：主機的IP地址
Pod IP：使用网络插件创建的IP（如flannel），使跨主机的Pod可以互通
Cluster IP：虚拟IP，通过iptables规则访问服务



### K8S建置教學 (以三台環境建置為例)
環境需3台。一台當master，二台當node

「master、node」登入後切換至最高權限 sudo su 
![K8S](0.jpg "sudo su")
```
apt-get update 
apt-get upgrade -y
apt-get dist-upgrade -y
docker：apt-get install –y docker.io 
```
kubeadm 管理k8s套件，依照文件照操作，如下圖
![kubeadm官方文件](kubeadm.jpg "kubeadm官方文件")
```
apt-get update && apt-get install -y apt-transport-https 

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add - 
 
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list 
deb http://apt.kubernetes.io/ kubernetes-xenial main 
EOF 
 
apt-get update 
 
apt-get install -y kubelet kubeadm kubectl 
```
![安裝kubeadm](001.jpg "安裝kubeadm")
![安裝kubeadm](002.jpg "安裝kubeadm")
![安裝kubeadm](003.jpg "安裝kubeadm")
![安裝kubeadm](004.jpg "安裝kubeadm")
![安裝kubeadm](005.jpg "安裝kubeadm")


「master、node」關閉SWAP，因為SWAP開著會無法安裝K8S
```
swapoff –a 
```
![關閉SWAP](swapoff.jpg "關閉SWAPS")

安裝完kubeadm之後進行初始化和架設網路，網路是使用fiannl來建置。
「master」 kubeadm初始化文件


![kubeadm初始化文件](screenshot_002.jpg "kubeadm初始化文件")
```
kubeadm init - -pod-network-cidr=10.244.0.0/16 
```
![kubeadm初始化](screenshot_003.jpg "kubeadm初始化")
![kubeadm初始化](screenshot_004.jpg "kubeadm初始化")

「master」kubeadm架設網路
```
sysctl net.bridge.bridge-nf-call-iptables=1
```
「master」建置資料夾、COYP檔案、修改權限
```
mkdir -p $HOME/.kube 
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config 
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
![架設網路、建置資料夾、COYP檔案、修改權限](screenshot_006.jpg "架設網路、建置資料夾、COYP檔案、修改權限")
執行完後會出現join node，綠框的金鑰複製下來。

「node」 join node，貼上金鑰讓node加入到master，即完成cluster
![node1 join](screenshot_007.jpg "node1 join")
![node2 join](screenshot_008.jpg "node2 join")

「master」真正分配網路
```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/v0.9.1/Documentation/kube-flannel.yml
```
![分配網路文件](screenshot_009.jpg "分配網路文件")
![分配網路](screenshot_010.jpg "分配網路")

「master」檢查 DNS 狀態狀態
```
kubectl get pods – -namespace kube-system 
```
剛開始 dns 還是 pending 狀態，等過一段時間後就會是 running 狀態
![檢查 DNS 狀態狀態](screenshot_011.jpg "檢查 DNS 狀態狀態")
以上k8s建置完畢

### 測試cluster，以nginx為例

建置nginx pods狀態，nginx的狀態要在running
```
kubectl run nginx - -image=nginx - -replicas=2 - -port=80 
```
![建置nginx](screenshot_012.jpg "建置ngin台")
查詢pods
```
kubectl get pods
```
![查詢pods](screenshot_013.jpg "查詢pods")
開始部署
```
kubectl get deploy
```
![開始部署](screenshot_014.jpg "開始部署")
開服務使用的port
```
kubectl expose deploy nginx - -type=NodePort
```
![開服務使用的port](screenshot_015.jpg "開服務使用的port")
查詢服務狀態
```
kubectl get service
```
![查詢服務狀態](screenshot_016.jpg "查詢服務狀態")
 網頁測試
 VM的IP+NodePort
![ 網頁測試](screenshot_017.jpg " 網頁測試")


----------------------------------------
如需重建，整組刪掉後再進行kubectl init
![kubeamd reset 刪除](screenshot_001.jpg "kubeamd reset 刪除")


参考文獻
<a href="https://kubernetes.io/docs/setup/independent/install-kubeadm/">https://kubernetes.io/docs/setup/independent/install-kubeadm/</a>
<a href="https://jimmysong.io/kubernetes-handbook/concepts/concepts.html">https://jimmysong.io/kubernetes-handbook/concepts/concepts.html</a>
<a herf="http://cizixs.com/2016/10/25/kubernetes-intro-kubelet">http://cizixs.com/2016/10/25/kubernetes-intro-kubelet </a>
<a herf="https://jimmysong.io/kubernetes-handbook/">https://jimmysong.io/kubernetes-handbook </a>
<a herf="https://kubernetes.io/docs/tutorials/kubernetes-basics/explore-intro/">https://kubernetes.io/docs/tutorials/kubernetes-basics/explore-intro/ </a>
<a herf="https://yeasy.gitbooks.io/docker_practice/content/kubernetes/concepts.html">https://yeasy.gitbooks.io/docker_practice/content/kubernetes/concepts.html </a>