---
title: "Field Manage System (FMS) of FRC"
description: "FMS 系統架構簡介"
slug: "frc-fms-introduction"
date: 2024-05-14
lastmod: 2024-05-14
categories:
    - FMS
tags:
    - FRC
    - FMS
---

## FMS
FMS 全名 Field Manage System(場地管理系統)<br>
FRC 競賽中，場地上的一切都是透過FMS進行操作，包括網路、計分、裁判、燈號等<br>
FMS 提供實時計分、機器人管理、資料管理與畫面呈現等功能<br>
為了保證穩定性與公平性，網路之間會透過VLan進行切分，避免互相干擾<br>

## 系統組成
### Smart Router
提供FMS對外網路，通常由場館提供

### Field Router
負責場地內的網路，連結SCC、AP、FMS Server、PLC、燈條等設備

### FMS Server
一台伺服器，負責執行FMS軟體，處理競賽流程與資料<br>
FMS Server 並不直接與Robot連線，而是透過DS進行中繼，若DS與FMS斷線，Robot也會因為看門狗(watchdog)停下<br>
FMS Server 也會收集來自DS與Robot的資訊<br>

### Programmable Logic Controller(PLC)
PLC被用於場地上的設備操作，包括馬達或自動計分器，透過網路與FMS Server連線

### Station Control Cabinet(SCC)
負責操作站上的設備與FMS Server連線，包括燈條、指示燈板、緊急停機按鈕、隊伍電腦等<br>

### Field Wireless Access Point(AP)
負責場地上的無線網路，robot使用5GHz(-2024)/6GHz(2025-)，其他設備使用2.4GHz<br>
設備為 Linksys WRT1900AC(-2024)/VIVID-Hosting VH-109(2025-)<br>
加密方法為 WPA2/AES，每場競賽都會重新釋放隊伍專屬網路與密碼

### Referee Panels
裁判記分板，可以使用平板替代

### Robots
機器人上包含 網路機、roboRIO，網路機在競賽中應該要被設定為橋接模式並連結指定的SSID與密碼

## Field Network
### 場地網路結構圖
![Field Network](fms-whitepaper-0.png)

### VLan
Field Router會切分Vlan將隊伍與場地的網路分離
* Blue:  10, 20, 30
* Red:   40, 50, 60
* Field: 100
Robot 所連結的Wifi 也會做出相同的切分，避免機器互相干擾

### Addressing
總共有7個DHCP Server，包括6個隊伍網路與1個場地網路

### Network Bandwidth
Robot radio將會被限制4Mb/s的頻寬<br>
網路優先順序為DS, Network Tables, others

## 總結
以上部份我們可以進行仿照以及簡化，減少設備需求，並達到所需要的功能<br>
下一篇將會介紹第三方的FMS

## 附錄
### 參考資料
* [FMS Whitepaper](https://fms-manual.readthedocs.io/en/latest/fms-whitepaper/fms-whitepaper.html)
* [FRCture - Reverse Engineering FRC](https://frcture.readthedocs.io/en/latest/)