---
title: GCP基礎課程
date: 2018-06-29 11:02:39
tags:
---
![GCP服務分類](google-cloud-platform.png "GCP服務分類")

# Google Cloud OnBoard 
## GCP 介紹
google所提供的雲端服務  (ex：computer engine、app engine、cloud storeage…etc)

![GCP服務分類](gcp01.png "GCP服務分類")
![GCP服務分類](gcp02.png "GCP服務分類")
- Compute：以計算為主的服務，包含Compute Engine, App Engine等服務；我們多使用Compute Engine建執行個體; 近期Google更推出Container Service，稱作Google Container Engine(GKE)，來服務使用Docker的用戶。
- Big Data：大資料儲存方案，包含BigQuery, Dataflow等等處理大資料的相關方案。
- Storage：儲存相關服務，包含SQL - CloudSQL, NoSQL - Datastore, File - Cloud Storage等儲存相關服務。
- APIs：包含所有Google服務的可操作程式介面，讓需要的使用者以PaaS的方式與Google介接，使用Google之服務。
- Networking：網路相關的服務，包含VPN與DNS服務，以Google管理的方式，提供網路層需要使用到的一些應用。

## IAM (Identity and Access Management)
IAM 主要概念可以用這句話描述  ”誰，想要怎麼使用，哪些資源”
![IAM](iam.png "IAM")

GCP內部預設的角色有：
- Owner：資源的擁有者，可以再將資源分享給其他人員
- Editor：資源的可編輯者，可以針對資源進行新增、修改、刪除等動作
- Viewer：資源的檢視者，可以針對資源進行使用，但無法修改資源狀態。
服務代理人：
- default預設的管理員(Compute Engine default service account)請不要砍掉，否則會有無法預期的問題

![IAM範例](iam01.png "IAM範例")

## 定價
Google雲端平台將即用即付的定價推向極致，並採用行業領先的創新技術
- 持續使用折扣：當您使用它們運行持續工作負載時，會自動降低虛擬機（VM）的價格。對於整個月運行的實例，賺取高達30％的折扣。計算引擎會自動折扣一個月內運行時間超過25％的實例。客戶平均享有24％的折扣。

- 每秒計費：提供真正的只付費即用型定價模式。例如，如果您正在構建微服務，或者只是運行一些資源測試，那麼您只需支付您使用GCP的微秒精確計費時間。

- 自定義機器類型：自定義機器類型可讓您輕鬆創建根據您的需求定制的機器類型，優惠折扣高達50％。
- 最適規模推薦：根據實例的使用情況推薦最佳數量的內核和內存。平均而言，調整每個實例可為客戶節省15％。

計價方式：https://cloud.google.com/pricing/
計算機：https://cloud.google.com/products/calculator/

## GCP指令列工具
Cloud SDK 是一套 Cloud Platform 工具，其中包含 gcloud、gsutil、bq和kubectl，可讓您透過指令列存取 Google Compute Engine、Google Cloud Storage、Google BigQuery，以及其他產品和服務。您可以利用這些工具進行互動操作，也可以運用在您的自動化指令碼中。

- gcloud：是Google Cloud Platform提供主要命令行界面的工具。您可以使用此工具從命令行或腳本和其他自動化中執行許多常見的平台任務。

- gsutil 工具提供指令列存取權，可用來管理 Cloud Storage 的 Bucket 和物件。Powershell cmdlet (Windows) 支援 PowerShell 的 Google 雲端工具 這是一套 Windows PowerShell cmdlet，可用於在 Windows PowerShell 環境中管理 Google Cloud Platform 資源。

- bq 工具
bq 可讓您透過指令列在 BigQuery 中執行查詢，以及操作資料集、資料表和實體。

- kubectl 工具
kubectl 可協調 Kubernetes 容器叢集在 gcloud 上的部署和管理作業。





## Stackdriver
Stackdriver可以通過訪問世界各地的地點來驗證服務的可用性。

Monitoring有一個概覽頁面，顯示您的所有正常運行時間檢查，以及詳細的個別檢查的儀表板。

如果您正在檢查的資源不公開，則必須配置資源的防火牆以允許來自正常運行時間檢查服務器的傳入流量。