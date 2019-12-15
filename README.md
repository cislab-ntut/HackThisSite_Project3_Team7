# Project7-3_Hackthissite
## 1063337 張仲緯
### Extbasic 3. FindaFake 1
>Often times you will need to decipher a language which you can not find on google, or is encrypted in some wayI have made up a language for you to decipher. What is the output of this program?  

>某人設計了一種程式語言，並寫了一支只有五行的程式，希望你去分析這支程式會輸出什麼內容
```
BEGIN notr.eal
CREATE int AS 2
DESTROY int AS 0
ANS var AS Create + TO
out TO
```
#### 思路
1. 首先第一行`BEGIN notr.eal`，看到`BEGIN`很自然地會想到傳統程式語言的function head
2. 最後一行`out TO`，必然會想到C++的`cout`等`print`功能，可以猜測最後一行就是**輸出`TO`變數**
3. 接著分析第二行`CREATE int AS 2`，看到`CREATE int`就會理解為**建立一個整數型態變數名稱`AS`且初始值為2**
4. 第三行`DESTROY int AS 0`我認為比較不容易分析，首先假設這裡是刪除變數`AS`，但從第四行看到`AS`仍然有被使用到，於是採用第二種假設，`DESTROY`為減法，所以**變成`AS = AS - 0`**
5. 第四行`ANS var AS Create + TO`一開始沒有什麼頭緒，第一種假設，這裡的`+ TO`是串接一個字串`TO`，也就是`AS = AS + TO`，第二種假設，不管`ANS var`跟`+`的意義，**單純假設`TO = AS`**
6. 首先嘗試內容中的關鍵字組合，TO、20、20TO、2TO、AS、ANS、var等皆錯誤，經過以上**粗體字**的思路後，得到**答案為2**
```
function notr.eal
variable named 'AS' = 2
AS = AS - 0
variable named 'TO' = AS
print TO
```
#### 其他思路
後來試著搜尋其他人的[想法](http://htstutorial.blogspot.com/2011/01/extbasic-mission-3.html)，看到其中一種想法是當作組合語言來思考
1. 第一行一樣是function宣告
2. 第二、三行將其理解為PUSH變數2、0到名為`AS`的stack裡面
3. 第四行是將`AS`的變數POP兩個出來放到`TO`，此時`TO = 02`也就是2
4. 輸出變數`TO`
```
function notr.eal
PUSH 2 into AS
PUSH 0 into AS
POP two values into TO
print TO
```
#### 總結
參考他人思路後，可以看到對於第一行及最後一行的理解大部分人應該都是差不多的，關鍵分析主要仍然在二三四行。由於題目只有給五行的範例程式，難以分析出此程式語言的詳細語法規範，因此可以推測這是在考我們分析的同時發揮一點想像力，也並不需要全部依照關鍵字的英文意義去推論，~~在這個網站上過於死板的黑客過程只會讓自己心態炸裂。~~
### Extbasic 13. I do validate. I really do.
```php
<?php
        if (isset($_GET['name']) && isset($_GET['email'])) {
                $user = mysql_real_escape_string($_GET['name']);
                $email = mysql_real_escape_string($_GET['email']);
                $result= mysql_fetch_assoc(mysql_query("SELECT `email` FROM `members` WHERE name = '$user'"));
                $reply = false;
                if ($email == $result['email'])
                {
                        $reply = true;
                }
        } else {
                $reply = false;
        }
        echo ($reply) ? 1 : 0;
?>
```
>The script's filename is vrfy.php Make the script reply 1.  Use the relative path. You don't know any users or emails.  

>給你一段php程式碼並告訴你它的檔案名稱是vrfy.php，希望你讓這段程式碼印出1
#### 思路
1. 看到程式碼出現`$_GET`關鍵字，就知道跟GET request method有關，首先修改網站網址路徑找看看有沒有這個網頁，但是找不到。
2. 看到`mysql_query()`，嘗試在變數中使用SQL injection`vrfy.php?name=or1=1/*&email=or1=1/*`，嘗試放入亂數、字串`vrfy.php?name=a&email=a`，以上測試於HackThisSite輸入答案皆錯誤
3. 既然沒有給我網頁測試，那我就自己放到[網站](https://molrobot.azurewebsites.net/vrfy.php?name=a&email=a)上去跑，測試發現`vrfy.php?name=a&email=a`可以印出1，但並不是題目想要的答案
4. 搜尋[論壇](https://www.hackthissite.org/forums/viewtopic.php?f=22&t=2753)得到標準答案為`vrfy.php?name=&email=`，發現論壇也有人說答案應該不唯一
5. 修改[網站](https://molrobot.azurewebsites.net/vrfy1.php?name=abc&email=cba)程式碼，嘗試將變數印出並觀察變化
```php
<?php
        if (isset($_GET['name']) && isset($_GET['email'])) {
                $user = mysql_real_escape_string($_GET['name']);
                $email = mysql_real_escape_string($_GET['email']);
                $result= mysql_fetch_assoc(mysql_query("SELECT `email` FROM `members` WHERE name = '$user'"));
                $reply = false;
		echo "\$_GET['name']=" . $_GET['name'] . '<br>';
		echo "\$_GET['email']=" . $_GET['email'] . '<br>';
		echo gettype($user) . ' $user=' . ($user ? 'true' : 'false') . '<br>';
		echo gettype($email) . ' $email=' . ($email ? 'true' : 'false') . '<br>';
		echo gettype($user) . ' $user=' . $user . '<br>';
		echo gettype($email) . ' $email=' . $email . '<br>';
		echo gettype($result) . " \$result=" . $result . '<br>'; 
		echo gettype($result['email']) . " \$result['email']=" . $result['email'] . '<br>';
                if ($email == $result['email'])
                {
                        $reply = true;
                }
        } else {
		echo "\$_GET['name']=" . $_GET['name'] . '<br>';
		echo "\$_GET['email']=" . $_GET['email'] . '<br>';
                $reply = false;
        }
        echo ($reply) ? 1 : 0;
?>
```
#### Key Point 1
```php
$user = mysql_real_escape_string($_GET['name']);
$email = mysql_real_escape_string($_GET['email']);
```
>`mysql_real_escape_string()`使用前需要先建立MySQL連線，否則執行失敗回傳False
#### Key Point 2
```php
if ($email == $result['email'])
{
        $reply = true;
}
```
>`NULL == NULL`在php為True，在其他程式語言或系統上不一定相同
#### 總結
似乎是因為兩年沒有碰php，三年沒碰mysql，我竟然沒有在第一時間發現**mysql_query()之前沒有建立MySQL連線**，就直接做了SQL injection測試。這一題如果硬要下去猜的話其實有很高的機會猜出正確答案，但要了解整個前因後果仍然需要具備一點php的概念。裡面使用的跳脫字元處理函數`mysql_real_escape_string()`過去從未使用過，透過官方[使用手冊](https://www.php.net/manual/en/function.mysql-real-escape-string.php)了解在php7.0以後正式移除功能。而`NULL == NULL`更是第一次碰到的觀念，搜尋相關資料的同時也找到了`False == False`為False的觀念，著實學到了不少東西。

## 1063304 陳無忌

## 1061418 葉亭妤
### Extbasic 9 - Captain Kirk learns perl!

#### 題目
![](https://i.imgur.com/5ZHJ8rw.png)

> 題目描述
> 1. 前情概要：Kirk 隊長編寫 Perl 腳本給其他人使用
> 2. 腳本功能：自動執行日誌紀錄，讓其他人能key進去並存檔
> 3. 腳本問題：日誌只能記錄一個，會自動刪除過往所有日誌
> 4. 修復要求：使日誌可以全部保存(修改程式碼)

#### 解題想法
1. 按下F12看code發現有好幾個超連結(hp.php / print.html / open.html) ~~看到超連結當然就是點下去啦~~
2. 進去超連結找尋有用資訊
   - [hp.php](https://www.hackthissite.org/hp.php) 看起來沒什麼幫助
   - [print.html](https://perldoc.perl.org/functions/print.html) 看起來語法也都對，先看下一個
   - [open.html](https://perldoc.perl.org/functions/open.html) 有找到相關資訊
3. 在open.html中找到與題目相關的內容
觀察到題目上第二行 open 的 code
```perl
open(STARTREKLOG, '>/var/log/startrek');
``` 
其中的 `>` 與網頁中的一段敘述有相關，仔細閱讀後發現 open 有不同模式  
1. `>` or `NULL` mode：read 模式(讀取文件)
2. `<` mode：write模式(截斷清空文件再創建新的文件)
3. `>>` mode：write模式(打開文件並 append 在後面)

由以上可推測出，由於Kerk隊長使用錯誤的open模式，才導致文件一直只能保存最後一筆的紀錄，所以我將open的模式從 `>` 改成 `>>` 得到答案是 
```perl
open(STARTREKLOG, '>>/var/log/startrek');
```
#### 總結
第一次接觸 perl ，也算蠻幸運有觀察到有用訊息，一開始題目就回答正確，也學習到 perl open()這個函式含有許多不同的模式，不僅上述說的幾點，還有像是前方加上加號的 `+>` 模式等等，學習到不少東西。

## 1051406 徐崑華

## 1060346 唐瑨
