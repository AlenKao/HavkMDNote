---
###### tags: `Program` `PHP`
---
PHP Program 概念(1)
===

## PHP 的用途概要
:::warning
![](https://i.imgur.com/6oQYXUc.png)
> 程式語言與程式
:::

* PHP 主要用於製作網頁應用程式(Web Application)

:::warning
![](https://i.imgur.com/AJobCi3.png)
> 網頁應用程式示意圖
:::

:::warning
![](https://i.imgur.com/iiNh7EV.png)
> 透過瀏覽器與網站伺服器使用網頁應用程式
:::

:::warning
![](https://i.imgur.com/9fPskmv.png)
> Request 與 Response
:::

:::warning
![](https://i.imgur.com/C5131LU.png)
> 網頁應用程式需進行的處理
:::

* HTML 無法做到的依 Request 內容變化處理，但是 PHP 程式可以製作。
* JavaScript 程式更擅長在瀏覽器上就能解決的處理。

## 建立開發環境
* 安裝之後你的電腦就是伺服器，具備了開發網頁應用程式時必備的網站伺服器和資料庫管理系統等軟體。
:::info
![](https://i.imgur.com/D3AO5gZ.png)
> 裡用 XAMPP 可一次建構好網站伺服器與資料庫
:::

:::info
**XAMPP 所包含的軟體**
* Apache
* MariaDB (MySQL)
* PHP 套件
* Perl 套件

可以支援 Windows、Linux、Mac OS X 等作業系統。
:::

:::info
**LAMP**
* Linux
* Apache
* MySQL
* PHP / Perl / Python

除 XAMPP 之外，LAMP 也是常指開發環境，LAMP 原本常用來建置網站伺服器的軟體合稱。
:::

### 下載 XAMPP
> XAMPP 官方網站
> https://www.apachefriends.org/zh_tw/index.html

> XAMPP 下載網頁
> https://www.apachefriends.org/zh_tw/download.html

### 安裝 XAMPP
1. 執行安裝檔
2. 選擇要安裝的軟體
3. 選擇安裝路徑
4. 開始安裝

### 啟動 XAMPP 控制面板
* XAMPP Control Panel

### 啟動 Apache
![](https://i.imgur.com/qe5v5lW.png)

### 確認 Apache 是否啟動
> 利用瀏覽器開啟下列 URL，確認 Apache 是否正常啟動。
> http://localhost/
> http://localhost/dashboard/

![](https://i.imgur.com/Pq2Y7Lu.png)

### 執行 PHP 程式
> 在檔案總管中開啟 C 磁碟機中 xampp 資料夾下的 htdocs 資料夾，即路徑 ==C:\xampp\htdocs==。
> 這個 htdocs 資料夾內就是用來放置 PHP 程式和 HTML 等檔案。

:::warning
請務必記住 **http://localhost** 與 **C:\xampp\htdocs\\** 的對應關係。

==要執行 PHP 程式時，必須輸入程式的對應 URL。==
:::

### 顯示執行結果
![](https://i.imgur.com/dnigPRb.png)

### 沒有正常顯示?
:::danger
> Apache 沒有啟動

![](https://i.imgur.com/j3QqRBa.png)

> 檔案存放的位置不正確

![](https://i.imgur.com/zJet3t6.png)
:::

[下一篇: PHP Program 概念(2)](https://hackmd.io/@leetcodeguy/php_2)