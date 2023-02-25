---
###### tags: `Program` `SQL`
---
SQL Program 概念
===

:::warning
**有關 SQL 的標記方式**

SQL 將大寫與小寫的英文字母視為相同的字母，但為了方便學習，都將 SQL 的關鍵字標記成大寫，表格名稱與視圖名稱則只有開頭是大寫，欄位名稱則全部以小寫英文字母標記

```sql!
SELECT name FROM Employee;
```

SQL 陳述句必須有「;」(分號)才能執行，所以 SQL 語句無法寫在同一行沒關係，但為了方便閱讀程式，都會刻意斷開過長的陳述句。

```sql!
SELECT name, birthday FROM Employee
WHERE birthday >= '1990-01-01'
AND birthday <= '1999-12-31';

+------------+------------+
| name       | birthday   |
+------------+------------+
| 山田太郎    | 1995-01-01 |
| 佐藤次郎    | 1990-05-03 |
| 鈴木花子    | 1990-02-11 |
+------------+------------+
3 rows in set
```
:::

## Chapter 1 學習SQL的事前準備

:::warning
資料庫就是「資料的基地」，要在電腦建立資料庫，就要使用 SQL 這種語言。
讓我們先安裝「MySQL」這套免費的 DBMS，接著建立練習用的資料庫，再新增練習用的資料，完成練習 SQL 的事前準備。

* **資料庫** - 紀錄資料的檔案，這個檔案可以多人共用。
* **DBMS** - 是個程式能讓多名使用者使用這個檔案。

使用者可以利用 SQL 語言撰寫 SQL 陳述式，再將這個陳述是傳遞給 DBMS，藉此使用資料庫。
:::

### 了解資料庫的基礎
:::info
![](https://i.imgur.com/V3dRfeA.png)

> 儲存檔案資料的電腦已被當成資料庫使用。
> 這台電腦會向多位使用者提供資料，所以又被稱為「伺服器(server = 提供者)」，也因為是提供資料庫功能的伺服器，所以又稱為「資料伺服器」，使用伺服器的使用者電腦則稱為「用戶端(client = 使用者)」。

\# 目前的資料庫都是向多位使用者提供資料的伺服器。

> 資料庫的實體就是儲存於伺服器的檔案，而這個檔案是由「DBMS(Data Base Management System = 資料庫管理系統)」的程式管理，使用者可以透過 DBMS 間接存取檔案的資料。

> 而「SQL(Structured Query Language) = 結構化查詢語言)」就是用來撰寫使用者向 DBMS 傳遞命令的語言。

\# 不管是哪種 DBMS 產品，SQL 的語法基本上都相同。
:::

:::info
![](https://i.imgur.com/D1jDaqB.png)

![](https://i.imgur.com/QlxcT87.png)

\# 活用資料庫就是指「搜尋」必要的資料。
:::

:::info
**目前的主流為「關聯式資料庫(relational data base)」**

\# 關聯式資料庫是以表格格式儲存資料。

* table 表格
* row 列
* column 欄

![](https://i.imgur.com/UDQl51p.png)

\# 表格是以列為單位，新增與刪除資料。
:::

:::info
**[關聯式資料庫的規範](https://hackmd.io/@HsingyuChen/NormalForm)**

==建立關聯式資料庫的兩項基本規則==
* 表格必須具有主鍵
* 表格只能有從屬於主鍵的項目

\# 表格一定要有主鍵(primary key)

\# 表格只能有從屬於主鍵的項目

* 要不要設定為特殊欄需視情況而定

\# 資料是否特殊，端看當下的情況而定

* 最好將長度固定的欄設定為主鍵
* 表格與表格連接的關聯性 -> 外部鍵(foreign key)

\# 外部鍵會從表格移動至另一個表格，讓資料產生關聯性的時候使用。
:::

:::success
建立表格時，應先決定欄數以及每一欄要存放什麼樣的資料。

在要建立資料表時，必須要在一開始就明確定出資料表的用途，避免之後要修改資料欄架構的情況發生。
:::

### 安裝 XAMPP 來建構 MySQL / MariaDB 資料庫
:::warning
在網路上可以找到許多特別打包的軟體套件，只要安裝妥當，便可以讓電腦具備 WWW 伺服器 (通常都是 Apache 伺服器) 及 PHP 軟體，以及學習上所需的 MySQL (現稱 MariaDB) 資料庫。

本節要介紹的是適用於 Windows 的 XAMPP 軟體套件，此套件結合 Apache 伺服器的 Windows 版本、PHP、MySQL / MariaDB 資料庫管理系統，此外還包括 PHPmyadmin 這個實用的 MySQL Web 管理介面，讓初學者能立即上手，接觸資料庫的世界。
:::

#### 安裝 XAMPP
你可以連到 https://www.apachefriends.org/zh_tw/index.html

![](https://i.imgur.com/5xhNlR3.png)

#### 設定 MySQL / MariaDB 管理員密碼
完成下載 XAMPP，就可以啟動 XAMPP 控制台來啟動 MySQL / MariaDB 資料庫了，MySQL / MariaDB 伺服器內建有一個管理員帳號 root，但預設沒有密碼，代表在剛安裝完成的狀態下，任何人都能透過 root 帳號取得 MySQL 的管理員權限，進行任何動作。因此為提高資料庫的安全性，最好先為此 root 帳號設定密碼，此設定工作可透過 phpMyAdmin 提供的 Web 管理介面來設定。

![](https://i.imgur.com/HV0xhKR.png)

![](https://i.imgur.com/KSglfD7.png)

![](https://i.imgur.com/6IJg9rv.png)
> 寫入剛剛設定的密碼

:::info
MaraiaDB 是做為 MySQL 延伸產品開發出來的 RDBMS，在使用方式上與 MySQL 完全相同。
:::

### 建立練習專用的資料庫 / 透過 SQL 語法來操作資料庫
:::warning
在 phpMyAdmin 資料庫管理介面中，透過拖拉滑鼠、點擊按鈕的方式，就可以建立資料庫或是建立資料表，全然不使用 SQL 語法也行。然而對於初學者來說，我們非常建議扎實地從 SQL 與法學起，這是往後學習更進階技術的基本功。
:::

:::info
資料庫使用者將 SQL 指令送交給 RDBMS，RDBMS 會依 SQL 指令對資料庫進行處理，並將結果傳回給資料庫使用者。
:::

:::success
==建立資料庫 -> 建立資料表 -> 新增資料==

1. 建立資料庫
```sql!
CREATE DATABASE prative;
```

2. 建立資料表
```sql!
CREATE TABLE Person
(person_id        CHAR(10) NOT NULL,
 person_name  VARCHAR(100) NOT NULL,
 person_birth   DATE NOT NULL,
 PRIMARY KEY (person_id));
```
![](https://i.imgur.com/9M8DIdC.png)

3. 新增資料
```sql!
INSERT INTO Person VALUES ('N226056789', '陳幸妤', '2002-02-12');
INSERT INTO Person VALUES ('N226123456', '王曉明', '1998-01-01');
```
:::

## SQL 基本語法
:::warning
* 要確定在 MySQL 命令列用戶端輸入的命令時，可按下 Enter 鍵。
* SQL 陳述式的結尾必須式分號(:)
* 要取消輸入的 SQL 陳述式就輸入「\c」
* 代表註解的兩個連字號(--)必須是半形字元。
* 不同的 DBMS 產品在 SQL 的命令上有些差異。
:::

### SQL 命令的分類
![](https://i.imgur.com/cQIlY1S.png)

:::success
### MySQL 的主要資料類型

| 資料類型 | MySQL 的關鍵字 | 資料種類 |
| -------- | -------- | -------- |
| 整數型 | INT | 沒有小數點的數值 |
| 實數型 | DOUBLE | 包含小數點的數值 |
| 固定長度字串類型 | CHAR | 字數固定的字串 |
| 可變長度字串類型 | VARCHAR | 字數不固定的字串 |
| 日期型 | DATE | YYYY-MM-DD 格式的日期 |
:::

:::info
\-\- 以指定的資料庫名稱建立資料庫
```sql!
CREATE DATABASES [資料庫名稱];
```

\-\- 列出所有資料庫
```sql!
SHOW DATABASES;
```

\-\- 利用資料庫名稱使用特定的資料庫
```sql!
USE [資料庫名稱];
```
> 只要執行一次，就能一直使用該資料庫，直到命令使用其他的資料庫。
> // 如果沒有先選取資料庫就執行操作資料庫 SQL 陳述句，就會顯示「++ERROR 1046(3D000):No database selected++(未選取資料庫)」的錯誤訊息。

\-\- 在欄位指定資料類型與條件，並且建立表格
```sql!
CREATE TABLE [表格名稱](
[欄位名稱 1] 資料類型 條件,
[欄位名稱 2] 資料類型 條件,
    .
    .
[欄位名稱 n] 資料類型 條件,
PRIMARY KEY(主鍵的欄位名稱)
);
```
> // 若顯示「Query OK」的訊息，代表已根據指定的名稱在資料庫新增具有指定表格。

\-\- 列出所有表格
```sql!
SHOW TABLES;
```

\-\- 於表格新增資料
```sql!
INSERT INTO [表格名稱] VALUES (資料1, 資料2,..., 資料n);
```

\-\- 從表格取得所有欄位的資料
```sql!
SELECT * FROM [表格名稱];
```

\-\- 以表格名稱指定要刪除的表格
```sql!
DROP TABLE [表格名稱];
```

\-\- 以資料庫名稱指定要刪除的資料庫
```sql!
DROP DATABASE [資料庫名稱];
```
:::

:::success
### 算數運算子的種類

| 運算子 | 意義 |
| -------- | -------- |
| + | 加法 |
| - | 減法 |
| * | 乘法 |
| / | 除法 |
:::

:::info
\-\- 從指定的表格取得特定欄位的資料
```sql!
SELECT [列名] FROM [表名];
```

\-\- 從表格取得多格欄位
```sql!
SELECT [欄位名稱 1, 欄位名稱 2, ...,欄位名稱 n] FROM [表格名稱];
```

\-\- 從表格取得排除重複之後的資料
```sql!
SELECT DISTINCT [欄位名稱] FROM [表格名稱];
```

\-\- 設定條件再取得資料的基本 SQL 陳述式
```sql!
SELECT [欄位名稱 1, 欄位名稱 2, ...,欄位名稱 n] FROM [表格名稱] WHERE [條件];
```

\-\- 取得等於某值的資料
```sql!
SELECT [欄位名稱 1, 欄位名稱 2, ...,欄位名稱 n] FROM [表格名稱] WHERE [條件名稱 = 值];
```

\-\- 取得不等於某值的資料
```sql!
SELECT [欄位名稱 1, 欄位名稱 2, ...,欄位名稱 n] FROM [表格名稱] WHERE [條件名稱 <> 值];
```

\-\-從表格取得欄位名稱為 NULL 的資料
```sql!
SELECT...FROM [表格名稱] WHERE [欄位名稱] IS NULL;
```

\-\- 取得不等於某值的資料
```sql!
SELECT...FROM [表格名稱] WHERE [欄位名稱] IS NOT NULL;
```

\-\- 欄位資料大於某值(只列出 WHERE 陳述句)
```sql!
WHERE [欄位 > 值];
```

\-\- 欄位資料大於等於某值(只列出 WHERE 陳述句)
```sql!
WHERE [欄位 >= 值];
```

\-\- 欄位資料小於某值(只列出 WHERE 陳述句)
```sql!
WHERE [欄位 < 值];
```

\-\- 欄位資料小於等於某值(只列出 WHERE 陳述句)
```sql!
WHERE [欄位 <= 值];
```
:::

:::success
### 邏輯運算子的種類
| 運算子 | 意義 |
| -------- | -------- |
| AND | 以及 |
| OR | 或是 |
| NOT | 不是 |

> 進行邏輯運算時，AND 比 OR 優先執行
:::

:::info
\-\- 條件 1 以及 條件 2(只列出 WHERE 陳述句)
```sql!
WHERE [條件 1] AND [條件 1];
```

\-\- 條件 1 或是 條件 2(只列出 WHERE 陳述句)
```sql!
WHERE [條件 1] OR [條件 1];
```

\-\- 不是該條件(只列出 WHERE 陳述句)
```sql!
WHERE NOT [條件];
```

\-\- 欄位落在 值 1 到 值 2 之間的範圍(只列出 WHERE 陳述句)
```sql!
WHERE [欄位名稱] BETWEEN [值 1] AND [值 2];
```

\-\- 欄位在 值 1、值 2、...、值 n 之中(只列出 WHERE 陳述句)
```sql!
WHERE [欄位名稱] IN ([值 1、值 2、...、值 n]);
```

\-\- 欄位不在 值 1、值 2、...、值 n 之中(只列出 WHERE 陳述句)
```sql!
WHERE [欄位名稱] NOT IN ([值 1、值 2、...、值 n]);
```

\-\- 欄位資料的結尾與指定的字串一致(只列出 WHERE 陳述句)
```sql!
WHERE [欄位名稱] LIKE ['%字串'];
```

\-\- 欄位資料的開頭與指定的字串一致(只列出 WHERE 陳述句)
```sql!
WHERE [欄位名稱] LIKE ['字串%'];
```

\-\- 欄位資料之中，有與指定字串一致的字元(只列出 WHERE 陳述句)
```sql!
WHERE [欄位名稱] LIKE ['%字串%'];
```
:::

:::info
\-\- 依照降冪(由大至小)的順序排列資料(只列出 ORDER BY 陳述句)
```sql!
ORDER BY [排序鍵] DESC;
```

\-\- 依照升冪(由小至大)的順序排列資料(只列出 ORDER BY 陳述句)
```sql!
ORDER BY [排序鍵] ASC;
```

\-\- 利用第1排序鍵與第2排序鍵排序資料(只列出 ORDER BY 陳述句)
```sql!
ORDER BY [第1排序鍵] ASC 或 DESC, [第2排序鍵] ASC 或 DESC;
```

\-\- 從起始位置開始，取得指定筆數的資料(只列出 LIMIT 陳述句)
```sql!
LIMIT [起始位置, 列數];
```
> ORDER BY 之後
:::

:::success
### 主要的彙總函數

| 彙總函數 | 功能 |
| -------- | -------- |
| SUM(欄位名稱) | 計算欄的總和 |
| AVG(欄位名稱) | 計算欄的平均值 |
| MAX(欄位名稱) | 計算欄的最大值 |
| MIN(欄位名稱) | 計算欄的最小值 |
| COUNT(欄位名稱) | 計算排除NULL欄位後的列數 |
| COUNT(\*) | 計算包含NULL欄位的列數 |
:::

:::info
\-\- 利用彙總函數從欄位資料取得需要的結果
```sql!
SELECT [彙總函數(欄位名稱)] FROM [表格名稱];
```

\-\- 計算欄位不為NULL的紀錄筆數
```sql!
SELECT COUNT(欄位名稱) FROM [表格名稱];
```

\-\- 計算欄位所有的紀錄筆數(包含NULL)
```sql!
SELECT COUNT(*) FROM [表格名稱];
```

\-\- 以指定位數四捨五入計算結果(只寫出函數的語法)
```sql!
ROUND(要四捨五入的數值, 進行四捨五入的位數);
```

\-\- 無條件捨去指定位數以下的數值(只寫出函數的語法)
```sql!
TRUNCATE(要進行無條件捨去的數值，要進行無條件捨去的位置);
```
:::

:::danger
```sql!
SELECT name, salary FROM Employee
WHERE salary > AVG(salary);
```
> 這個會出現錯誤訊息
> 無法在 WHERE 陳述句使用彙總函數
> 因為程式先讀 WHERE 在找 SELECT，所以在讀 WHERE 時，還不知道該欄位的數值，也就無法計算。
:::

:::success
```sql!
SELECT name, salary FROM Employee
WHERE salary > (SELECT AVG(salary) FROM Employee);
```
> 這個就不會出現錯誤訊息
:::

:::info
\-\- 利用 欄位名稱 1 與 欄位名稱 2 合併 表格 1 與 表格 2(只寫出 FROM 與 WHERE 陳述句)
```sql!
FROM [表格名稱 1, 表格名稱 2] WHERE [表格名稱 1.欄位名稱 1]=[表格名稱 2.欄位名稱 2];
```
> 如果 表格名稱 1.欄位名稱 1 為 NULL 就無法合併，也就不會顯示。

\-\- 以 欄位名稱 1 與 欄位名稱 2 讓 表格名稱 1 與 表格名稱 2 進行內部合併(只寫出接在 FROM 後面的內容)
```sql!
FROM [表格名稱 1] INNER JOIN [表格名稱 2] ON [表格名稱 1.欄位名稱 1]=[表格名稱 2.欄位名稱 2];
```
> 內部合併(INNER JOIN)無法取得無法合併的資料。

\-\- 以左外部合併的方式合併 表格名稱 1 與 表格名稱 2(只寫出接在 FROM 後面的內容)
```sql!
FROM [表格名稱 1] LEFT OUTER JOIN [表格名稱 2] ON [表格名稱 1.欄位名稱 1]=[表格名稱 2.欄位名稱 2];
```
> 左外部合併(LEFT OUTER JOIN)可在左側的表格(作為基準的表格)的資料無法與右側表格的資料合併時取得左側表格的資料。

\-\- 以 欄位名稱 1 與 欄位名稱 2 讓 表格名稱 1 與 表格名稱 2 進行內部合併(只寫出接在 FROM 後面的內容)
```sql!
FROM [表格名稱 1] RIGHT OUTER JOIN [表格名稱 2] ON [表格名稱 1.欄位名稱 1]=[表格名稱 2.欄位名稱 2];
```
> 右外部合併(RIGHT OUTER JOIN)可在右側的表格(作為基準的表格)的資料無法與左側表格的資料合併時取得右側表格的資料。
:::

:::info
\-\- 以欄位名稱為彙總鍵，群組化資料(只寫出 GROUP BY 陳述句)
```sql!
GROUP BY 欄位名稱;
```
> 群組化之後，能接在 SELECT 命令之後的只有彙總函數或是接在 GROUP BY 的欄位名稱。

\-\- 以欄位名稱群組化資料，再取得符合條件的資料(只寫出 GROUP BY 陳述句與 HAVING 陳述句)
```sql!
GROUP BY 欄位名稱 HAVING 條件;
```
> HAVING 陳述句可使用彙總函數。
:::

:::success
### SELECT 陳述式的陳述句剖析順序

1. FROM 陳述句 \-\-指定資料來源的表格
2. WHERE 陳述句 \-\-在群組化之前，先篩選出列
3. GROUP BY 陳述句 \-\- 群組化篩選出來的列
4. HAVING 陳述句 \-\- 在取得群組之前，先篩選群組
5. SELECT 陳述句 \-\- 取得群組化之後的欄位名稱以及彙總函數的處理結果
6. ORDER BY 陳述句 \-\- 排序取得的資料
:::

:::info
\-\- 依照條件，設定要取得的值(只寫出 CASE 的語法)
```sql!
CASE WHEN 條件 1 THEN 值 1
     WHEN 條件 2 THEN 值 2
      ..
     WHEN 條件 n THEN 值 n
     ELSE 不符合任何條件時的值
END AS 欄位名稱;
```
:::

:::success
### 視圖的限制

* 無法透過視圖更新參照的表格
* 視圖不能使用 ORDER BY 陳述式

> 視圖雖然能像操作表格般操作，卻不等於表格。

==**子查詢:**== 是指可在 SQL 陳述式開頭以外的位置使用 SELECT 命令，而子查詢主要是於 WHERE 陳述句使用，但也可以在非 WHERE 陳述句的位置使用。

> 子查詢與試圖非常相似，差異在於子查詢會在 SQL 陳述式執行完畢後，不留下任何紀錄，但是視圖會存在資料庫裡，所以能重複使用。
:::

:::info
\-\- 替 AS 後面的 SQL 陳述式加上視圖的名稱
```sql!
CREATE VIEW [視圖名稱] AS SELECT [命令的 SQL 陳述式];
```

\-\- 列出所有表格(包含視圖)
```sql!
SHOW TABLES;
```

\-\- 刪除視圖
```sql!
DROP VIEW [視圖名稱];
```

\-\- 欄位與子查詢的資料部分一致(只寫出 WHERE 陳述式的部分)
```sql!
WHERE [欄位名稱] IN (子查詢);
```

\-\- 欄位與子查詢的資料都不一致(只寫出 WHERE 陳述式的部分)
```sql!
WHERE [欄位名稱] NOT IN (子查詢);
```
:::

:::success
新增、更新、刪除資料之後，都應該確認結果。
:::

:::info
\-\- 於表格新增一筆資料
```sql!
INSERT INTO [表格名稱] VALUES [(欄位 1 的值, 欄位 2 的值, ..., 欄位 n 的值)];
```

\-\- 以 VALUES 後面的括號所指定的值，在指定的表格與欄位裡新增一筆資料
```sql!
INSERT INTO [表格名稱][(欄位名稱 1, 欄位名稱 2, ..., 欄位名稱 n)] VALUES (欄位名稱 1 的值, 欄位名稱 2 的值, ..., 欄位名稱 n 的值);
```

\-\- 找出符合條件的資料，並在欄位指定更新之後的值，藉此更新表格的內容
```sql!
UPDATE [表格名稱] SET [欄位名稱] = [更新之後的值] WHERE [紀錄的更新條件];
```
> 若不在 UPDATE 命令指定條件，所有紀錄都會被更新。

\-\- 從表格刪除符合條件的資料
```sql!
DELETE FROM [表格名稱] WHERE [刪除條件];
```
> 若不在 DELETE 命令指定條件，所有紀錄都會被更新。
:::

:::success
### 啟動交易機制後，就能利用 Rollback 功能還原

1. 開始交易    `START TRANSACTION;`
2. 新增資料    `INSERT INTO ... ;`
3. 更新資料    `UPDATE ... ;`
4. 刪除資料    `DELETE FROM ... ;`
5. Rollback   `ROLLBACK;`
> 2、3、4 的處理都會被取消

==若不先執行「`START TRANSACTION;`」，就無法利用「`ROLLBACK;`」還原。==

==養成先執行「`START TRANSACTION;`」在更新資料的習慣。==
:::

:::info
\-\- 啟動交易機制
```sql!
START TRANSACTION;
```

\-\- 回到交易機制啟動之前的狀態
```sql!
ROLLBACK;
```

\-\- 提交吧
```sql!
COMMIT;
```
> 確定更新處理(無法還原的狀態)。
:::