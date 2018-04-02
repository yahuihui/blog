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
![K8S](pod.jpg "pod VS container")

Kubernetes中所有的配置都是通过API对象的spec去设置的s
kubernates 是使用etcd做為系統的設定儲存中心、重要資料都是持久化在etcd中的。


kubectl是和Kubernetes API交互的命令行程序

k8s使用etcd作為儲存中介軟體


### K8S建置教學 (以三台環境建置為例)
環境需3台。一台當master，二台當node
登入後切換至最高權限 sudo su 
![K8S](0.jpg "sudo su")

安裝kubeadm 管理k8s套件，依照文件照操作
![K8S](kubeadm.jpg "安裝kubeadm")
![K8S](001.jpg "安裝kubeadm")
![K8S](002.jpg "安裝kubeadm")
![K8S](003.jpg "安裝kubeadm")
![K8S](004.jpg "安裝kubeadm")
![K8S](005.jpg "安裝kubeadm")
![K8S](006.jpg "安裝kubeadm")



![K8S](screenshot_001.jpg "控制台")
![K8S](screenshot_002.jpg "控制台")
![K8S](screenshot_003.jpg "控制台")
![K8S](screenshot_004.jpg "控制台")
![K8S](screenshot_005.jpg "控制台")
![K8S](screenshot_006.jpg "控制台")
![K8S](screenshot_007.jpg "控制台")
![K8S](screenshot_008.jpg "控制台")
![K8S](screenshot_009.jpg "控制台")
![K8S](screenshot_010.jpg "控制台")
![K8S](screenshot_011.jpg "控制台")
![K8S](screenshot_012.jpg "控制台")
![K8S](screenshot_013.jpg "控制台")
![K8S](screenshot_014.jpg "控制台")
![K8S](screenshot_015.jpg "控制台")
![K8S](screenshot_016.jpg "控制台")
![K8S](screenshot_017.jpg "控制台")




参考文獻
<a href="https://kubernetes.io/docs/setup/independent/install-kubeadm/">1.https://kubernetes.io/docs/setup/independent/install-kubeadm/</a>
<a href="https://jimmysong.io/kubernetes-handbook/concepts/concepts.html">2.https://jimmysong.io/kubernetes-handbook/concepts/concepts.html</a>
<a herf="http://cizixs.com/2016/10/25/kubernetes-intro-kubelet">3.http://cizixs.com/2016/10/25/kubernetes-intro-kubelet </a>