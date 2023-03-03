---
###### tags: `Program` `PHP`
---
PHP Program 概念(2)
===

## PHP 基本語法
### Get Start: PHP標籤與程式碼
```php!
<?php
PHP 程式內容
?>
```
> PHP 只會執行 `<?php` 與 `?>` 所框住的內容。

### echo
用來顯示文字訊息
```php!
echo '文字訊息';
```
> 以 `'` 或 `"` 框住的內容會被當作是字串。
> 請留意每行程式的最後，都會有一個分號(;)。

* 有多個敘述並列時，將由上至下依序執行。

:::warning
```php!
<?php
echo Welcome;
?>
```
![](https://i.imgur.com/TrNs6DY.png)
> 未定義常數 Welcome，錯誤發生於 C:\xampp\htdocs\php\chapter3\welcome-error.php 第 2 行
:::

### print
用來顯示文字訊息
```php!
print '文字訊息';
```
> echo 和 print 都可用來顯示文字訊息。

* 通常認為 echo 的處理速度較快(執行到顯示出訊息所需的時間較短)。
* echo 具有將多個字串、數值連接起來顯示的功能。

:::info
**HTML 區塊:** 用於顯示固定不變的內容。
**PHP 區塊:** 用於顯示會因情況而變動的內容。

```php!
<?php echo '歡迎光臨'; ?>
```
可以化簡為
```php!
<?= '歡迎光臨'; ?>
```
:::

### require
在 PHP 中，要載入並執行放在其他檔案中的程式，必須使用 require 敘述。
```php!
require '檔案名稱';
```

* 可將程式中重複使用的內容存為獨立檔案，再用 require 載入他。
* 節省反覆輸入重複內容的時間，讓程式看起來更簡潔。
* 要修改共通使用的部分時，不需在多支程式裡分別修改，只要修改單一檔案的內容即可。

> user-input.php
```php!
<?php require '../header.php'; ?>

<p>請輸入姓名：</p>
<form action="user-output.php" method="post">
  <!-- post v.s. get -->
  <input type="text" name="user"> 
  <!-- user 是用在 PHP 中接收的 REQUEST參數名 -->
  <input type="submit" value="確定">
</form>

<?php require '../footer.php'; ?>
```

### request
取得表單回傳的參數(字串)
```php!
$_REQUEST['REQUEST參數名']
```

> user-output.php
```php!
<?php require '../header.php'; ?>

<?php
echo '午安，', $_REQUEST['user'], '您好。';
?>

<?php require '../footer.php'; ?>
```

#### 優化程式
:::info
![](https://i.imgur.com/5JEZajT.png)

避免出現 **REQUEST參數名** 未被定義(Undefined) 的情形，可以用以下寫法。
```html!
<?php require '../header.php'; ?>

<?php
if (isset($_REQUEST['user'])) {
    echo '午安，', $_REQUEST['user'], '您好。';
}
?>

<?php require '../footer.php'; ?>
```
![](https://i.imgur.com/2Bpm0QH.png)
:::

:::info
![](https://i.imgur.com/6E79QJG.png)

![](https://i.imgur.com/Civ5Q3E.png)

避免出現 **輸入內容含有 HTML，並被執行** 的情形，可以用以下寫法。
```html!
<?php require '../header.php'; ?>

<?php
if (isset($_REQUEST['user'])) {
	echo '午安，', htmlspecialchars($_REQUEST['user']), '您好。';
}
?>

<?php require '../footer.php'; ?>

```
![](https://i.imgur.com/q5JHc7O.png)
:::

### 算符(或稱運算子 Operator)
進行計算的處理即稱為「運算」
| 算符 | 作用 |
| -------- | -------- |
| ** | 平方 |
| ++ \-\- | 加1、減1 |
| ! | 邏輯(反值) |
| * / % | 乘法、除法、餘數 |
| + - . | 加法、減法、字串相連 |
| < <= > >= | 比較(小於、小於等於、大於、大於等於) |
| == != | 比較(等於、不等於) |
| && | 邏輯(AND) |
| \|\| | 邏輯(OR) |
| = | 指派 |

==算符有優先順序之分==

### 變數
是用來存放資料的機制

1. 變數名稱的前面必須加上錢字號($)。
2. 開頭第1個字必須為英文字母或底線(_)。
3. 除了第1個字之外，其他可用英文字母、數字、底線隨意組成。
4. 英文字母的大小寫視為不同文字。

:::success
**預先定義好的變數**

自行定義的變數名稱不可與這些 PHP 預先定義的變數名稱相同

| 變數名稱 | 功能 |
| -------- | -------- |
| \$\_REQUEST | HTTP 的所有 REQUEST 參數(無論是以 GET 或 POST 方式傳送) |
| \$\_GET | HTTP 以 GET 方式傳送的 REQUEST 參數 |
| \$\_POST | HTTP 以 POST 方式傳送的 REQUEST 參數 |
| \$\_FILES | 上傳檔案的資料 |
| \$\_SESSION | Session |
| \$\_COOKIE | Cookie |
:::

### 指派
「=」被稱為指派算符，可將他右邊的值寫入左邊的變數。
```php!
變數=值
```

### 變數之間的指派
```php!
變數A=變數B
```

### 常數
一開始指派了常數的值之後，就不能再對值做變更。
```php!
const 常數名=值
```

### 陣列(Array)
陣列內區分為多個區塊，用來存放多個資料值，這些區塊稱為陣列的**元素**。
為了區隔個別元素，必須使用 **索引(index)** 來存取。

![](https://i.imgur.com/rFG1NTN.png)

```php!
陣列=[值A, 值B, 值c, ...];
```

```php!
陣列=[
    值A, 
    值B, 
    值c, 
    ...
];
```

```php!
陣列=[索引A => 值A,索引B => 值B,索引c => 值c,...];
```

```php!
陣列=[
    索引A => 值A, 
    索引B => 值B, 
    索引c => 值c, 
    ...
];
```

#### 新增元素
```php!
陣列[]=新元素;
```

## 流程控制
:::success
**核取方塊**
> check-input.php
```php!
<?php require '../header.php'; ?>

<form action="check-output.php" method="post">
  <p><input type="checkbox" name="mail">訂閱特賣情報電子報</p>
  <p><input type="submit" value="確定"></p>
</form>

<?php require '../footer.php'; ?>
```
![](https://i.imgur.com/twAI9eY.png)

> check-output.php
```php!
<?php require '../header.php'; ?>

<?php
if (isset($_REQUEST['mail'])) {
	echo '已訂閱特賣情報電子報。';
} else {
	echo '未訂閱特賣情報電子報。';
}
?>

<?php require '../footer.php'; ?>
```
:::

### if
if 判斷式是 PHP 中用於條件分歧的語法之一。
```php!
if(條件){
  條件成立時進行的處理;
}
```

#### 以真偽值代入 if 判斷式
```php!
if(條件){
  條件為 TRUE 時執行的處理;
}
```

### if-else
在條件成立時與條件不成立時，分別執行對應的處理。
```php!
if(條件){
  條件成立時進行的處理;
} else{
  條件不成立時進行的處理;
}
```

#### 以真偽值代入 if 判斷式
```php!
if(條件){
  條件為 TRUE 時執行的處理;
} else{
  條件為 FALSE 時執行的處理;
}
```

### isset
檢查變數是否宣告，NULL 是用來表示變數內為空值的特殊值。
若變數中已代入值且其值不為 NULL 時，則回傳 TRUE。
```php!
isset(變數)
```

#### 在 if 判斷式中使用 REQUEST 參數
```php!
if(isset(REQUEST 參數中的變數)){
  變數已宣告時要執行的處理;
}
```

#### 在 if-else 判斷式中使用 REQUEST 參數
```php!
if(isset(REQUEST 參數中的變數)){
  變數已宣告時要執行的處理;
} else{
  變數未宣告時要執行的處理;
}
```

### 條件運算式
在 if 判斷式和 if-else 判斷式的條件中，也可使用運算式。

==常數 + 算符(運算子) + 常數==

#### 在 if 判斷式的條件中使用運算式
```php!
if(運算式){
  當運算結果條件為 TRUE 時執行的處理;
}
```

#### 在 if-else 判斷式的條件中使用運算式
```php!
if(運算式){
  當運算結果條件為 TRUE 時執行的處理;
} else{
  當運算結果條件為 FALSE 時執行的處理;
}
```

#### 省略大括號{}
當 if 判斷式的 {} 中只有 1 行程式時，可省略 {}。
但若 {} 有多行程式，則不可省略。
```php!
if(運算式) 當運算結果條件為 TRUE 時執行的處理;
```

```php!
if(運算式) 當運算結果條件為 TRUE 時執行的處理; else 當運算結果條件為 FALSE 時執行的處理;
```

:::success
**單選紐**
> radio-input.php
```php!
<?php require '../header.php'; ?>

請選擇餐點：
<form action="radio-output.php" method="post">
  <p><input type="radio" name="meal" value="日式套餐" checked>日式套餐</p>
  <p><input type="radio" name="meal" value="西式套餐">西式套餐</p>
  <p><input type="radio" name="meal" value="中式套餐">中式套餐</p>
  <p><input type="submit" value="確定"></p>
</form>

<?php require '../footer.php'; ?>
```
![](https://i.imgur.com/Wpl9TlS.png)

> radio-output.php
```php!
<?php require '../header.php'; ?>

<?php
switch ($_REQUEST['meal']) {
case '日式套餐':
	echo '烤魚、燉菜、味噌湯、白飯、水果';
	break;
case '西式套餐':
	echo '果汁、水波蛋、薯餅、麵包、咖啡';
	break;
case '中式套餐':
	echo '春捲、煎餃、蛋花湯、炒飯、杏仁豆腐';
	break;
}
echo '將稍候送達';
?>

<?php require '../footer.php'; ?>
```
:::

### switch
依照不同的輸入訊息顯示出不同訊息的程式，是常見將程式流程分成多段，再依使用者所選內容執行對應的流程。
```php!
switch (運算式) {
case 值A:
	運算結果為 A 時應執行的處理;
	break;
case 值B:
	運算結果為 B 時應執行的處理;
	break;
case 值C:
	運算結果為 C 時應執行的處理;
	break;
...
}
```

* 在 case 敘述的最後必須寫上 break 敘述，用來表示處理程序結果，跳出 switch 判斷式的區塊。請記得加上 break 敘述。

:::success
**下拉式選單**
> select-input.php
```php!
<?php require '../header.php'; ?>

<p>請選擇座位類型：</p>
<form action="select-output.php" method="post">
  <select name="seat">
  <option value="自由席">自由席</option>
  <option value="指定席">指定席</option>
  <option value="商務車廂">商務車廂</option>
  </select>
  <p><input type="submit" value="確定"></p>
</form>

<?php require '../footer.php'; ?>
```
![](https://i.imgur.com/aMcQD3g.png)

> select-output.php
```php!
<?php require '../header.php'; ?>

<?php
switch ($_REQUEST['seat']) {
case '指定席':
	echo '需加付120元補票。';
	break;
case '商務車廂':
	echo '需加付250元補票。';
	break;
default:
	echo '不需補票。';
	break;
}
?>

<?php require '../footer.php'; ?>
```
:::

### default
以上皆非可以使用 default 敘述撰寫。
```php!
switch (條件運算式) {
case 值A:
	運算結果為 A 時應執行的處理;
	break;
case 值B:
	運算結果為 B 時應執行的處理;
	break;
default:
	運算結果與所有 case 的值都不符合時應執行的處理;
	break;
...
}
```

:::success
**下拉式選單**
> select-for-input.php
```php!
<?php require '../header.php'; ?>

<p>請選擇訂購數量：</p>
<form action="select-for-output.php" method="post">
  <select name="count">
  <option value="0">0</option>
  <option value="1">1</option>
  <option value="2">2</option>
  <option value="3">3</option>
  <option value="4">4</option>
  <option value="5">5</option>
  <option value="6">6</option>
  <option value="7">7</option>
  <option value="8">8</option>
  <option value="9">9</option>
  </select>
  <p><input type="submit" value="確定"></p>
</form>

<?php require '../footer.php'; ?>
```

> select-for-input2.php
```php!
<?php require '../header.php'; ?>

<p>請選擇訂購數量：</p>
<form action="select-for-output.php" method="post">
  <select name="count">
    
  <?php
  for ($i=0; $i<10; $i++) {
  	echo '<option value="', $i, '">', $i, '</option>';
  }
  ?>

  </select>
  <p><input type="submit" value="確定"></p>
</form>

<?php require '../footer.php'; ?>
```
![](https://i.imgur.com/IBSbkgJ.png)

![](https://i.imgur.com/F622F0Z.png)

> select-for-output.php
```php!
<?php require '../header.php'; ?>

<?php
echo $_REQUEST['count'], '個商品放入購物車。';
?>

<?php require '../footer.php'; ?>
```
:::

### for
用來進行重複處理的語法之一。
```php!
for(開始處理; 條件式; 更新處理){
  重複執行的處理;
}
```

### 比較算符
| 演算子 | 讀法 | 判斷結果為 TRUE 的情況 |
| -------- | -------- | -------- |
| < | 小於 | 左邊值小於右邊 |
| > | 大於 | 左邊值大於右邊 |
| <= | 小於等於 | 左邊值小於等於右邊(左邊值在右邊以下) |
| >= | 大於等於 | 左邊值大於等於右邊(左邊值在右邊以上) |
| == | 相等 | 左右相等 |
| != | 不相等(!為否定之意) | 左右不相等 |

### while
用來進行重複處理的語法之一。
```php!
while(條件式){
  重複執行的處理;
}
```

* for 迴圈與他最大的差異，在於 while 迴圈沒有**開始處理**和**更新處理**。
```php!
開始處理
while(條件式){
  重複執行的處理;
  更新處理
}
```

### foreach
```php!
foreach(陣列 as 變數){
  使用變數進行的處理;
}
```

```php!
foreach($question as $item){
  echo '<option value="', $item, '">', $item, '</option>';
}
```

```php!
foreach(陣列 as 索引鍵的變數 => 值的變數){
  以索引鍵與值進行的處理;
}
```

```php!
foreach($store as $key => $value){
  echo '<option value="', $value, '">', $key, '</option>';
}
```

## 使用函式
### 呼叫函式
```php!
函式(傳入參數)
```

```php!
函式(傳入參數1, 傳入參數2, ...)
```

### 設定時區
```php!
data_default_timezone_set(地區)
```

### 顯示日期時間
data 函式執行時，會先取得現在的日期時間，再依指定的格式將它當做字串回傳。
```php!
data(格式)
```

| 文字 | 說明 |
| -------- | -------- |
| Y | 年。4位數 |
| m | 月。2位數，無十位數時自動補0 |
| d | 日。2位數，無十位數時自動補0 |
| H | 小時。2位數，無十位數時自動補0。24小時制 |
| i | 分。2位數，無十位數時自動補0 |
| s | 秒。2位數，無十位數時自動補0 |

### rand 函式
用來隨機產生一組數值，如果未傳入任何參數，則會回傳0以上，亂數最大值以下的亂數回來。
```php!
rand()
```

亂數最大值會依執行環境不同而有差異，利用 gettrandmax 函式就能取得最大值。
```php!
gettrandmax()
```

```php!
rand(最小值, 最大值)
```

### preg_match 函式
利用常規表達式檢查資料格式。
```php!
preg_match(模板, 輸入字串)
```

當傳入參數中指定的模板與輸入字串的格式相符，則 preg_match 函式會回傳「1」、「TRUE」; 若格式不符，則回傳「0」、「FALSE」。

```php!
preg_match('/^[0-9]{7}$/', $postcode)
```

* `^` :句首
* `[0-9]` :0 ~ 9 的數字1個
* `{7}` :符合前項格式的文字7個
* `$` :句尾
* `-` :連字號
* `(?=.*[a-z])` :包含小寫英文字母(a ~ z)
* `(?=.*[A-Z])` :包含大寫英文字母(A ~ Z)
* `(?=.*[0-9])` :包含數字(0 ~ 9)
* `[a-zA-Z0-9]` :小寫英文字母、大寫英文字母、數字各1個
* `{8,}` :符合前項格式的文字8個以上

首先是「.」與「\*」，「.」代表任意1字;「\*」則代表它前面的文字重複0次以上。
兩者合一的「.\*」，表示任意文字重複0次以上。

### 將半形轉換成全形
```php!
mb_convert_kana(字串, 類型代碼)
```

```php!
mb_convert_kana($_REQUEST['name'], 'R') // R 的代碼就表示將半形轉換成全形
```

| 類型代碼 | 意義 |
| -------- | -------- |
| r | 全形英文字母轉換成半形 |
| R | 半形英文字母轉換成全形 |
| n | 全形數字轉換成半形 |
| N | 半形數字轉換成全形 |
| a | 全形英數字轉換成半形 |
| A | 半形英數字轉換成全形 |
| s | 全形空格轉換成半形 |
| S | 半形空格轉換成全形 |

:::success
**檔案讀寫**
> board-input.php
```php!
<?php require '../header.php'; ?>

<p>請輸入留言內容。</p>
<form action="board-output.php" method="post">
  <input type="text" name="message">
  <input type="submit" value="送出">
</form>

<?php require '../footer.php'; ?>
```
![](https://i.imgur.com/C3WDzJ1.png)

> board-output.php
```php!
<?php require '../header.php'; ?>

<?php
$file='board.txt';
if (file_exists($file)) {
	$board=json_decode(file_get_contents($file));
}
$board[]=$_REQUEST['message'];
file_put_contents($file, json_encode($board));
foreach ($board as $message) {
	echo '<p>', $message, '</p><hr>';
}
?>

<?php require '../footer.php'; ?>
```
![](https://i.imgur.com/nSsdF2d.png)
:::

### 檔案存取
1. 從檔案讀出所有留言清單
2. 將新留言加入留言清單
3. 將所有留言寫入檔案
4. 顯示留言一覽

==要與留言內容儲存到檔案，必須使用 JSON 格式。JSON 為 JSON JavaScript Object Notation 的縮寫。==

:::info
JSON 是緣自於程式語言 JavaScript 的標記方式，除了 JavaScript，亦被運用在多種不同的程式語言。
在 PHP 中使用 JSON 的優點是可輕易地從檔案讀取、寫入字串與陣列等資料結構。
:::

### 讀取檔案
指定的檔案(資料夾)存在時，會回傳 TRUE；若檔案(資料夾)不存在則回傳 FALSE。
```php!
file_exists(檔案名稱)
```

讀取檔案
```php!
file_get_contents(檔案名稱)
```

由於讀取檔案是以 JSON 格式儲存，必須先轉換成 PHP 可用的格式，此時使用可以用來進行 JSON 編碼的 json_decode 函式。
```php!
json_decode(字串)
```

```php!
$board=json_decode(file_get_contents($file))
```

將陣列轉換成 JSON 格式
```php!
json_encode(值)
```

寫入檔案
```php!
file_put_contents(檔案名稱, 字串)
```
> 若要新增檔案不存在，則會新增一個檔案。

:::success
**檔案上傳**
> upload-input.php
```php!
<?php require '../header.php'; ?>

<p>請選擇要上傳的檔案。</p>
<form action="upload-output.php" method="post" enctype="multipart/form-data">
  <p><input type="file" name="file"></p>
  <p><input type="submit" value="開始上傳"></p>
</form>

<?php require '../footer.php'; ?>
```
![](https://i.imgur.com/WxrSmYs.png)

> upload-output.php
```php!
<?php require '../header.php'; ?>

<?php
if (is_uploaded_file($_FILES['file']['tmp_name'])) {
	if (!file_exists('upload')) {
		mkdir('upload');
	}
	$file='upload/'.basename($_FILES['file']['name']);
	if (move_uploaded_file($_FILES['file']['tmp_name'], $file)) {
		echo $file, '上傳成功。';
		echo '<p><img src="', $file, '"></p>';
	} else {
		echo '上傳失敗。';
	}
} else {
	echo '請選擇檔案。';
}
?>

<?php require '../footer.php'; ?>
```
![](https://i.imgur.com/ebdOS9v.png)
:::

### 取得暫存檔的檔名
```php!
$_FILES['檔案選擇按鈕的名稱']['tmp_name']
```
* `tmp_name` : 取得暫存檔檔名
* `name` : 取得原檔名

### 檢查是否是輸入畫面所上傳的檔案
```php!
is_uploaded_file(檔案名稱)
```

### 建立資料夾
```php!
mkdir(資料夾名稱)
```

### 只抽取出檔案名
```php!
basename(路徑)
```

### 儲存暫存檔
將上傳後產生的暫存檔移動到儲存上傳檔的位置。
```php!
move_uploaded_file(暫存檔的檔名, 儲存用的檔名)
```

[上一篇: PHP Program 概念(1)](https://hackmd.io/@leetcodeguy/php_1) 
[下一篇: PHP Program 概念(3)](https://hackmd.io/@leetcodeguy/php_3)