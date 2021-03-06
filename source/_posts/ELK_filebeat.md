---
title: filebeat
date: 2018-03-28 17:14:34
tags:
---
![filebeat](filebeat.jpg "filebeat")
### ELK 簡介 
ELK 是 Elasticsearch、Logstash 和 Kibana 三種軟體產品的首字母縮寫，這三者都是開源軟體。
 
Elasticsearch︰分散式搜索和分析引擎。具有高可伸縮、高可靠和易管理等特點。能對大容量的資料進行接近實時的存儲、搜索和分析操作。

Logstash︰資料收集引擎。從各種資料源搜集資料，並對資料進行過濾、分析、豐富、統一格式等操作，然後存儲到用戶指定的位置。 

Kibana︰資料分析和可視化平台。通常與 Elasticsearch 配合使用，對其中資料進行搜索、分析和以統計圖表的方式展示。 

Filebeat︰ELK 協議棧的新成員，一個輕量級開源日志檔案資料搜集器，基于 Logstash-Forwarder 源代碼開發。在需要采集日志資料的 server 上安裝 Filebeat，並指定日志目錄或日志檔案後，Filebeat 就能讀取資料，迅速傳送到 Logstash 進行解析，亦或直接傳送到 
Elasticsearch 進行集中式存儲和分析。 




最簡單架構只有一個 Logstash、Elasticsearch 和 Kibana 。Logstash 通過輸入外掛程式從多種資料源（比如日志檔案、標準輸入 Stdin 等）獲取資料，再經過濾外掛程式加工資料，然後經 Elasticsearch 輸出外掛程式輸出到 Elasticsearch，通過 Kibana 展示。

把一個 Logstash 資料搜集節點擴展到多個，分布于多台機器，將解析好的資料傳送到 Elasticsearch server 進行存儲，最後在 Kibana 查詢、生成日志報表等。 原文網址：https://ifun01.com/BHBLFYZ.html


這種結構因為需要在各個伺服器上部署 Logstash，而它比較消耗 CPU 和內存資源，所以比較適合計算資源豐富的伺服器，否則容易造成伺服器性能下降，甚至可能導致無法正常工作。 原文網址：https://ifun01.com/BHBLFYZ.html


### filebeat 架構
![filebeat](filebeat01.jpg "filebeat架構")
filebeat.yml中需設定日誌檔案的路徑，每個日誌檔案(LOG)，Filebeat啟動harvester(收割機)。每個harvester讀取新的內容一個日誌檔案，新的日誌資料傳送到spooler（後台處理程式），它匯集的事件和聚合資料傳送到你已經配置了Filebeat輸出。 


原文網址

<a href="https://ifun01.com/BHBLFYZ.html">1.https://ifun01.com/BHBLFYZ.html</a>

<a href="https://ifun01.com/8JODF4K.html">2.https://ifun01.com/8JODF4K.html</a>
