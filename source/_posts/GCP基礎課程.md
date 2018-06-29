---
title: GCP基礎課程
date: 2018-06-29 11:02:39
tags:
---
![GCP服務分類](google-cloud-platform.png "GCP服務分類")

# GCP 介紹
google所提供的雲端服務  (ex：computer engine、app engine、cloud storeage…etc)

![GCP服務分類](gcp01.png "GCP服務分類")
![GCP服務分類](gcp02.png "GCP服務分類")
-Compute：以計算為主的服務，包含Compute Engine, App Engine等服務；我們多使用Compute Engine建執行個體; 近期Google更推出Container Service，稱作Google Container Engine(GKE)，來服務使用Docker的用戶。

-Big Data：大資料儲存方案，包含BigQuery, Dataflow等等處理大資料的相關方案。

-Storage：儲存相關服務，包含SQL - CloudSQL, NoSQL - Datastore, File - Cloud Storage等儲存相關服務。

-APIs：包含所有Google服務的可操作程式介面，讓需要的使用者以PaaS的方式與Google介接，使用Google之服務。

-Networking：網路相關的服務，包含VPN與DNS服務，以Google管理的方式，提供網路層需要使用到的一些應用。

## IAM (Identity and Access Management)
IAM 主要概念可以用這句話描述  ”誰，想要怎麼使用，哪些資源”
![IAM](iam.png "IAM")

