---
###### tags: `Program` `PHP`
---
PHP Program 概念(3)
===

## 與資料庫的結合運用
:::success
**在 PHP 中連結資料庫**
> all.php
```php!
<?php require '../header.php'; ?>

<?php
$pdo=new PDO('mysql:host=localhost;dbname=shop;charset=utf8', 
	'帳號', '密碼');
?>

<?php require '../footer.php'; ?>
```

網頁沒有顯示錯誤訊息就表示連接成功。
:::

### 利用 PDO 連接資料庫
:::warning
在 PHP 中要連結資料庫，通常使用提供了 PHP 與資料庫間的連線機制的 PDO。
在 PHP 中，可以使用 類別(Class) 來統整定義相關的變數與函式。
PDO 即是一種類別，裡面包含了操作資料庫時會使用到的變數與函式。

類別中的變數稱為 屬性(property)，類別內的函式稱為 方法(Method)。
要使用類別之前，一定要產生 instance。
:::

#### 產生 PDO 的 instance
```php!
$pdo=new PDO(...);
```

* 用來識別資料庫的參數(DSN)
    ```php!
    'mysql:host=localhost;dbname=shop;charset=utf8'
    ```
    * **mysql**
        表示要連線到 MySQL。在冒號( : )之後列出連線資料庫所需的資料，並以分號( ; )分隔每項資料。
    * **host=localhost**
        是指 MySQL 存在於 localhost。在此所用的 MySQL，是與 XAMPP 一起安裝在您手邊的電腦上，因此指定位置為 localhost。
    * **dbname=shop**
        表示 shop 資料庫。
    * **charset=utf8**
        則表示資料庫所用的文字編碼為 UTF-8。

* 登入的使用者名稱
    ```php!
    'staff'
    ```
    第2個參數用來設定登入資料庫的使用者名稱。
    這裡指定為建立 shop 資料庫時建立的使用者 staff。必須以單引號( ' )框住。

* 登入密碼
    ```php!
    'password'
    ```
    第3個參數用來設定登入密碼。
    這裡指定為建立使用者所設定的密碼 password。必須以單引號( ' )框住。
    
:::success
**用 PHP 顯示 table 中資料**
> all2.php
```php!
<?php require '../header.php'; ?>

<?php
$pdo=new PDO('mysql:host=localhost;dbname=shop;charset=utf8', 
	'staff', 'password');
foreach ($pdo->query('select * from product') as $row) {
	echo '<p>';
	echo $row['id'], ':';
	echo $row['name'], ':';
	echo $row['price'];
	echo '</p>';
}
?>

<?php require '../footer.php'; ?>
```

> all3.php
```php!
<?php require '../header.php'; ?>

<?php
$pdo=new PDO('mysql:host=localhost;dbname=shop;charset=utf8', 
	'staff', 'password');
foreach ($pdo->query('select * from product') as $row) {
	echo "<p>$row[id]:$row[name]:$row[price]</p>";
}
?>

<?php require '../footer.php'; ?>
```
==以雙引號框住的字串，具有可在字串之中插入變數值的功能。==

![](https://i.imgur.com/H3yWQdP.png)
:::

#### 執行 SQL 指令
```php!
PDO的變數->query('SQL指令')
```

變數 PDO 名字是 $pdo
```php!
$pdo->query('SQL指令')
```

==呼叫方法的程式寫法為( 變數->方法 )==

#### 取得資料欄內的資料
```php!
陣列名稱['資料欄名稱']
```

==利用 foreach 迴圈，就可將資料逐筆處理。==

:::success
**用 PHP 顯示 Table 中資料**
> all4.php
```php!
<?php require '../header.php'; ?>

<table>
<tr><th>商品編號</th><th>商品名稱</th><th>商品價格</th></tr>

<?php
$pdo=new PDO('mysql:host=localhost;dbname=pratice;charset=utf8', 
	'帳號', '密碼');
foreach ($pdo->query('select * from person') as $row) {
	echo "<tr><td>$row[person_id]</td><td>$row[person_name]</td><td>$row[person_birth]</td></tr>";
	echo "\n";
}
?>

</table>

<?php require '../footer.php'; ?>
```
==以雙引號框住的字串，具有可在字串之中插入變數值的功能。==

> all5.php
```php!
<?php require '../header.php'; ?>

<table>
<tr><th>商品編號</th><th>商品名稱</th><th>商品價格</th></tr>

<?php
$pdo=new PDO('mysql:host=localhost;dbname=shop;charset=utf8', 
	'staff', 'password');
foreach ($pdo->query('select * from product') as $row) {
	echo '<tr>';
	echo '<td>', htmlspecialchars($row['id']), '</td>';
	echo '<td>', htmlspecialchars($row['name']), '</td>';
	echo '<td>', htmlspecialchars($row['price']), '</td>';
	echo '</tr>';
	echo "\n";
}
?>

</table>

<?php require '../footer.php'; ?>
```
==資料中若有可能包含在 HTML 中有特殊意義的字，顯示前應先經 htmlspecialchars 函式處理。==

![](https://i.imgur.com/BpEk2uR.png)
:::

### 函式的定義
PHP 不僅提供了許多現成的函式，程式設計師也可自行撰寫需要的函式。
```php!
function 函式名稱(傳入參數, ...){
    執行的處理;
    ...
    return 回傳值;
}
```

### 資料的搜尋
:::success
**用欄位中的名稱搜尋資料**
> search-input.php
```php!
<?php require '../header.php'; ?>

請輸入商品名稱：
<form action="search-output.php" method="post">
<input type="text" name="keyword">
<input type="submit" value="搜尋">
</form>

<?php require '../footer.php'; ?>
```

> search-output.php
```php!
<?php require '../header.php'; ?>

<table>
<tr><th>商品編號</th><th>商品名稱</th><th>商品價格</th></tr>

<?php
$pdo=new PDO('mysql:host=localhost;dbname=shop;charset=utf8', 
	'staff', 'password');
$sql=$pdo->prepare('select * from product where name=?');
$sql->execute([$_REQUEST['keyword']]);
foreach ($sql->fetchAll() as $row) {
	echo '<tr>';
	echo '<td>', $row['id'], '</td>';
	echo '<td>', $row['name'], '</td>';
	echo '<td>', $row['price'], '</td>';
	echo '</tr>';
	echo "\n";
}
?>

</table>

<?php require '../footer.php'; ?>
```
:::

#### where
在 SQL 的 select 敘述中，用來指定搜尋條件，在他之後直接寫出條件式。
```php!
where 資料欄名稱 ='搜尋的鍵值'
```

可以用 ? 來代替之後才要代入的值。
```php!
select * from product where 資料欄名稱 =?
```

#### prepare
是用來進行 SQL 指令執行前的準備。
```php!
PDO的變數->prepare('SQL指令')
```

prepare 方法在執行後，會回傳已設定好 SQL 指令的 PDOStatement 實例。
這個實例在之後執行 SQL 指令時還是會用到，因此要先將它指定給變數。
```php!
$sql=$pdo->prepare('select * from product where name=?');
```

![](https://i.imgur.com/yV1leFh.png)

#### execute
要執行以傳入參數傳入 prepare 方法的 SQL 指令，必須利用 PHP 內建的 PDOStatement 類別的 execute 方法。
```php!
變數->execute(值)
```

若指令中有多個( ? )時，可以用( , )分隔多值。
```php!
[$_REQUEST['first_keyword'], $_REQUEST['second_keywork']]
```

==利用 prepare 方法預處理好 SQL 指令，要利用 execute 方法執行。==

#### fetchAll
利用 execute 方法執行 SQL 指令後，可再利用 PDOStatement 類別的 fetchAll 方法取的執行結果。
```php!
foreach(PDO的變數->fetchAll() as 要將取的結果代入的變數)
```

```php!
foreach ($sql->fetchAll() as $row) {
```

#### like
搜尋部分符合的商品
```php!
select * from product where name like ?;
```

==? => %word%==
指就算 word 前後有其他字也符合條件

```php!
$sql=$pdo->prepare('select * from product where name like ?');
$sql->execute(['%'.$_REQUEST['keyword'].'%']);
```

#### not like
找出不含搜尋關鍵字的資訊
```php!
select * from product where name not like ?;
```

#### like and not like
```php!
select * from product where name like ? and name not like ?;
```

### 在資料表內新增資料
:::success
> insert-input.php
```php!
<?php require '../header.php'; ?>

<p>資料新增：</p>
<form action="insert-output.php" method="post">
商品名稱<input type="text" name="name">
價格<input type="text" name="price">
<input type="submit" value="確定新增">
</form>

<?php require '../footer.php'; ?>
```

> insert-output.php
```php!
<?php require '../header.php'; ?>

<?php
$pdo=new PDO('mysql:host=localhost;dbname=shop;charset=utf8', 
	'staff', 'password');
$sql=$pdo->prepare('insert into product values(null, ?, ?)');
if ($sql->execute([$_REQUEST['name'], $_REQUEST['price']])) {
	echo '新增成功。';
} else {
	echo '新增失敗。';
}
?>

<?php require '../footer.php'; ?>
```

> insert-output2.php
```php!
<?php require '../header.php'; ?>

<?php
$pdo=new PDO('mysql:host=localhost;dbname=shop;charset=utf8', 
	'staff', 'password');
$sql=$pdo->prepare('insert into product values(null, ?, ?)');
if (empty($_REQUEST['name'])) {
	echo '請輸入商品名稱。';
} else if (!preg_match('/[0-9]+/', $_REQUEST['price'])) {
	echo '請以整數輸入商品價格。';
} else
    if ($sql->execute(
        [htmlspecialchars($_REQUEST['name']), $_REQUEST['price']]
    )) {
        echo '新增成功。';
    } else {
        echo '新增失敗。';
    }
?>

<?php require '../footer.php'; ?>
```
:::

#### empty
在值為空時，回傳 TRUE。
```php!
empty(值)
```

### 資料隱碼攻擊(SQL injection)
:::danger
說到要預防資料庫存入有問題的資料，就必須提到 **資料隱碼攻擊(SQL injection)**。

**資料隱碼攻擊** 是指系統執行了非開發者撰寫的 SQL 指令，導致資料庫被非法存取的問題。
:::

### 修改資料庫資料
:::success
> update-input.php
```php!
<?php require '../header.php'; ?>

<table>
<tr><th>商品編號</th><th>商品名稱</th><th>商品價格</th></tr>

<?php
$pdo=new PDO(
	'mysql:host=localhost;dbname=shop;charset=utf8', 'staff', 'password'
);
foreach ($pdo->query('select * from product') as $row) {
	echo '<tr><form action="update-output.php" method="post">';
	echo '<input type="hidden" name="id" value="', $row['id'], '">';
	echo '<td>', $row['id'], '</td>';
	echo '<td>';
	echo '<input type="text" name="name" value="', $row['name'], '">';
	echo '</td>';
	echo '<td>';
	echo '<input type="text" name="price" value="', $row['price'], '">';
	echo '</td>';
	echo '<td><input type="submit" value="確定修改"></td>';
	echo '</form></tr>';
	echo "\n";
}
?>

</table>

<?php require '../footer.php'; ?>
```

> update-output.php
```php!
<?php require '../header.php'; ?>

<?php
$pdo=new PDO('mysql:host=localhost;dbname=shop;charset=utf8', 
	'staff', 'password');
$sql=$pdo->prepare('update product set name=?, price=? where id=?');
if (empty($_REQUEST['name'])) {
	echo '請輸入商品名稱。';
} else
if (!preg_match('/[0-9]+/', $_REQUEST['price'])) {
	echo '請以整數輸入商品價格。';
} else
if ($sql->execute(
	[htmlspecialchars($_REQUEST['name']), 
	$_REQUEST['price'], $_REQUEST['id']]
)) {
	echo '修改成功。';
} else {
	echo '修改失敗。';
}
?>

<?php require '../footer.php'; ?>
```
:::

### 刪除資料庫資料
:::success
> delete-input.php
```php!
<?php require '../header.php'; ?>

<table>
<tr><th>商品編號</th><th>商品名稱</th><th>商品價格</th></tr>

<?php
$pdo=new PDO('mysql:host=localhost;dbname=shop;charset=utf8', 
	'staff', 'password');
foreach ($pdo->query('select * from product') as $row) {
	echo '<tr>';
	echo '<td>', $row['id'], '</td>';
	echo '<td>', $row['name'], '</td>';
	echo '<td>', $row['price'], '</td>';
	echo '<td>';
	echo '<a href="delete-output.php?id=', $row['id'], '">確定刪除</a>';
	echo '</td>';
	echo '</tr>';
	echo "\n";
}
?>

</table>

<?php require '../footer.php'; ?>
```

> delete-output.php
```php!
<?php require '../header.php'; ?>

<?php
$pdo=new PDO('mysql:host=localhost;dbname=shop;charset=utf8', 
	'staff', 'password');
$sql=$pdo->prepare('delete from product where id=?');
if ($sql->execute([$_REQUEST['id']])) {
	echo '刪除成功。';
} else {
	echo '刪除失敗。';
}
?>

<?php require '../footer.php'; ?>
```
:::

#### 含有 REQUEST 參數的連結
```php!
要開啟的檔案名稱?REQUEST參數名=值
```

```php!
要開啟的檔案名稱?REQUEST參數名=值 & REQUEST參數名=值 &...
```

```php!
echo '<a href="delete-output.php?id=', $row['id'], '">確定刪除</a>';
```

### 整合新增、修改、刪除
:::success
> edit3.php
```php!
<?php require '../header.php'; ?>

<table>
<tr><th>商品編號</th><th>商品名稱</th><th>價格</th></tr>

<?php
$pdo=new PDO('mysql:host=localhost;dbname=shop;charset=utf8', 
	'staff', 'password');
if (isset($_REQUEST['command'])) {
	switch ($_REQUEST['command']) {
	case 'insert':
		if (empty($_REQUEST['name']) || 
			!preg_match('/[0-9]+/', $_REQUEST['price'])) break;
		$sql=$pdo->prepare('insert into product values(null,?,?)');
		$sql->execute(
			[htmlspecialchars($_REQUEST['name']), $_REQUEST['price']]);
		break;
	case 'update':
		if (empty($_REQUEST['name']) || 
			!preg_match('/[0-9]+/', $_REQUEST['price'])) break;
		$sql=$pdo->prepare(
			'update product set name=?, price=? where id=?');
		$sql->execute(
			[htmlspecialchars($_REQUEST['name']), $_REQUEST['price'], 
			$_REQUEST['id']]);
		break;
	case 'delete':
		$sql=$pdo->prepare('delete from product where id=?');
		$sql->execute([$_REQUEST['id']]);
		break;
	}
}
foreach ($pdo->query('select * from product') as $row) {
	echo '<tr>';
	echo '<form action="edit3.php" method="post">';
	echo '<input type="hidden" name="command" value="update">';
	echo '<input type="hidden" name="id" value="', $row['id'], '">';
	echo '<td>', $row['id'], '</td>';
	echo '<td>';
	echo '<input type="text" name="name" value="', $row['name'], '">';
	echo '</td>';
	echo '<td>';
	echo '<input type="text" name="price" value="', $row['price'], '">';
	echo '</td>';
	echo '<td><input type="submit" value="確定修改"></td>';
	echo '</form>';
	echo '<form action="edit3.php" method="post">';
	echo '<input type="hidden" name="command" value="delete">';
	echo '<input type="hidden" name="id" value="', $row['id'], '">';
	echo '<td><input type="submit" value="確定刪除"></td>';
	echo '</form>';
	echo '</tr>';
	echo "\n";
}
?>

<tr>
<form action="edit3.php" method="post">
<input type="hidden" name="command" value="insert">
<td></td>
<td><input type="text" name="name"></td>
<td><input type="text" name="price"></td>
<td><input type="submit" value="確定新增"></td>
</form>
</tr>
</table>

<?php require '../footer.php'; ?>
```
:::

## 實用的 PHP 程式 - 以購物網站為例
### Session
:::info
是在網頁應用程式中用來放置各使用者資料的機制。
利用 Session 機制，就能管理每個使用者的個別資料。
在 PHP 中，可透過 \$\_SESSION 陣列存取 Session 資料。
:::

## 網站上線的實務知識
### 限制錯誤訊息的顯示
```php!
error_reporting(級別);
```

#### 級別
| 常數 | 意義 |
| -------- | -------- |
| 0 | 隱藏所有錯誤訊息 |
| E_ERROR | 出現重大的執行錯誤時，中斷程式的執行 |
| E_WARNING | 執行時出現的警告不會中斷程式的執行 |
| E_PARSE | 解釋程式時出現錯誤。會在語法錯誤時發生 |
| E_NOTICE | 執行時出現注意。會在懷疑程式可能有誤發生時 |
| E_ALL | 顯示所有錯誤訊息 |

### 利用框架(Framework)
框架(Framework)與函式庫一樣，都是用來支援應用程式開發的軟體。

但是框架提供的不是好用的函式與類別，而且用來規範應用程式的技術方式。

框架並不是用來提供應用程式會用到的部分功能，而是用來提供建構應用程式的整體框架。
因此和函式庫一樣，若能善用框架，就能在短時間開發出功能更好的應用程式。

此外，在多人同時開發應用程式時，利用框架就能統一應用程式的技術方式。
可以讓開發團隊更容易共享資訊，提高開發效率。

:::warning
**Laravel**
> https://laravel.com/
:::

[上一篇: PHP Program 概念(2)](https://hackmd.io/@leetcodeguy/php_2)
[下一篇: SQL Program 概念](https://hackmd.io/@leetcodeguy/sql)