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
### Extbasic 4 - Finda Fake 2
>Often times you will need to decipher a language which you can not find on google, or is encrypted in some way
>I have made up a language for you to decipher. This is slightly harder. What is the output of this program?
>This is a REAL language with REAL rules. This is practice for obfustication or encrypted functions.

`{user types 6,7}`
```
BEGIN F.ake
var int as in
int var as in
out var int
```
#### 思路
1. 本題與上一題**FindaFake 1**為類似的分析題，因此首先參考上一題與本題語法上的相似之處。
```
BEGIN notr.eal
CREATE int AS 2
DESTROY int AS 0
ANS var AS Create + TO
out TO
```
2. 依照個人分析的想法，上方`CREATE int AS 2`以及`DESTROY int AS 0`的宣告代表著`int`型態的`CREATE`與`DESTROY`分別被指派`2`與`0`的數值。
3. 依此類推，本題`var int as in`與`int var as in`分別代表`int`型態的`var`變數與`var`型態的`int`變數，指派的數值皆為`in`。
4. 根據本題程式上方`{user types 6,7}`判斷，`6`與`7`為本題的輸入，因此判斷前面所提到的`in`代表input的意思，分別為`6`與`7`。
5. 將`var`與`int`變數所輸入的`6`與`7`兩個數值，按照`out`的順序輸出，即為答案`67`。
#### 總結
本題與上一題相同，只能在短短幾行的程式中推測出程式的意思，在一開始的時候，由於沒有注意到最上方`{user types 6,7}`使用者輸入這一行，而被交錯混雜的`int`與`var`所混淆，但在注意到之後，由於`in`所代表的input非常好辨認，所以一下就試出答案了；本題需要一些能突破框架的思維能力，只要不過度執著於題目中的文字，便能順著input找到本題的答案。

### Extbasic 5 - Fix the script
>Notice: do not use sed -r. This only works for linux. Instead use sed -E.

>Sam wants certain users to be able to run limited commands from a PHP page. He created a function called safeeval to run these commands. However on one page he neglected to use safeeval and instead used eval(). Safeeval will fail if a command given should not run.
>Sam then created a shell script to fix the error.

>Sam's uname is:
>freeBSD 6.9

>Here is the script:
```
<?php
        include ('safe.inc.php');
        if ($access=="allowed") {
                eval($_GET['cmd']);
                if (!empty($_GET['cmd2'])) {
                        eval($_GET['cmd2']);
                }
        }
?>
```
>Here is his shell script (for freeBSD):
```
#!/bin/sh
rm OK
sed -E "s/eval/safeeval/" <exec.php >tmp && touch OK
if [ -f OK ]; then
        rm exec.php && mv tmp exec.php
fi
```
>Fix the incorrect line in the shell script (and use the SAME spacing).
#### 題目說明
本題的要求為修復上方shell script的錯誤，由於解題的方式為shell script的修正，因此將對於本題shell script的語法進行說明以及除錯。
#### 題目講解
`#!/bin/sh`

首行的`#!`宣告這個script所使用的shell，後面接著`/bin/sh`即為shell的路徑。

`rm OK`為移除名為`OK`的檔案

`sed -E "s/eval/safeeval/" <exec.php >tmp && touch OK`

`sed`：為stream editor，有著對資料特定的字串進行新增、刪除、取代等等的功能。

`-E`：參數可連接多個sed script，若只有一個sed script則可省略。

`"s/eval/safeeval/"`：此指令中的`s`為substitute的縮寫，其功能為將他之後的第一個字串`eval`用下個字串`safeeval`代替，要被代替的字串可以用reguler expression來表示。

`<exec.php`：sed支援std input，此行將`exec.php`作為其stdin作為輸入。

`>tmp`：將sed編輯過後的檔案輸出到`tmp`的檔案中。

`&&`：在左邊指令的執行結果回傳為true後(即執行成功)，才會執行右邊的指令。

`touch OK`：`touch`的功能可以建立檔案以及修改檔案、目錄等的時間戳記，在此沒有參數的用法中，若檔案不存在，便會建立一個空白的新檔案。

本行指令的目的為，當`sed`更改檔案成功之後，便建立一個名為`OK`的檔案。

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
### Extbasic 8 Perl is a bitch sometimes
>So Bill Gates was tired of VisualBasic and now did some Perl, too bad; this script has a security flaw that allows everyone access to the company records! Fix the flaw for him!

>比爾蓋茨厭倦了VB,他現在想做些perl,但不幸運的是,這個腳本有點缺陷,就是會允許所有人進入這個系統！請幫忙修好它！
```
#!/usr/bin/perl
 
chomp(my $User = `/usr/bin/whoami`);
 
print "Checking your access level...\n";
 
if ($User == 'BillGates')
{
    print "Authorized! Here are the company records:\n" . `cat /home/BillGates/CompanyRecords.db`;
    die("Closing...\n");
}
 
die("You're not authorized!\n");
```
#### 解題思路
1. 首先根據題目文字的意思，這段code應該是某個地方有一點瑕疵，並且正式因為這個瑕疵會讓所有人都可以得到系統權限，接著仔細看下代碼，發現有一個if語句會根據條件成立與否進行不同的操作。<br>
2. 接著分析下代碼：首先第一行是說獲得系統中的主機名稱，然後賦值給`$User`這個變量,第二行是說打印"檢查你的存取級別"這句話，第三行進入`if`語句判斷：如果`$User == 'BillGates'`,那麼你將登陸成功，獲得存取權限，否則會打印"你沒有權限"這句話。<br>
3. 因為題目是說所有人都會得到權限所以我猜應該問題出在`$User == 'BillGates'`這句話上面，即這個式子一定是`true`，下面的登錄成功語句一定會顯示。<br>
4. 接著我分析思考下`$User == 'BillGates'`這句話為什麼一定會是`true`,我聯想到python中的`is`語句和`==`是不一樣的，在python中，`is`用來判斷是不是同一個內存對象，而`==`用來判斷2個對象之間的值是否相同。基於這個啟發，我想到可能是`$User == 'BillGates'`中`=`是不是不對，因為通常`==`都是用來判斷數值大小的嘛，使用`==`的話可能會造成兩邊都演變成一樣的數值？<br>
5. 然後我查了下資料，發現perl中字符串比較的話，是使用`eq`語句而不是說使用`==`來比較，而題目中這個明顯是不對的，要用`eq`來替換。<br>
6. 但是我思考了下，即使使用`==`來比較字符串，為什麼一定會是`true`呢？哪怕兩邊根本不一樣啊。我查了相關文獻，了解到在`perl`中，如果用`==`來比較字符串，則字符串兩邊都會轉化為數值`0`,所以不管兩邊字符串是什麼，最終結果都會是`true`。<br>

#### 解決方法
更改`$User == 'BillGates'`這段代碼，把`==`改成`eq`,在輸入框中輸入完整的`$User eq 'BillGates'`這句代碼，然後回車就會顯示成功。

#### 知識擴展
因為搜索資料時發現很多語言都會出現 &nbsp;字符串比較&nbsp; 和&nbsp; 常規數值比較 &nbsp;的不同方法，所以查閱相關知識做了一個小的總結: <br>
>Java中：

  &nbsp; &nbsp; `==`比較的是變量之間的內存地址<br>
  &nbsp; &nbsp; `equals`比較的是兩變量的內容<br>
  &nbsp; &nbsp;  所以通常都用`equals`比較字符串內容，數字一般用`==`來比較<br>
  
>C#中：

 &nbsp; &nbsp;  `==`操作比較的是兩個變量的值是否相等，對於引用型變量表示的是兩個變量在堆中存儲的地址是否相同，即棧中的內容是否相同<br>
  &nbsp; &nbsp;  `equals`操作表示的兩個變量是否是對同壹個對象的引用，即堆中的內容是否相同。<br>
 
>Python中

  &nbsp; &nbsp;  `is`的作用是用來檢查對象的標示符是一致，也就是比較兩個對象在內存中的地址是否一樣<br>
  &nbsp; &nbsp;  `==`用來檢查兩個對象是否相等<br>
  
>C++中

  &nbsp; &nbsp;  `==`是判斷引用是否相同<br>
  &nbsp; &nbsp;  `equals()`指的是值是否相同。
  
#### 總結
  小小的語法錯誤可能會導致整個系統的安全性變得完全不可靠，所以在學習一門語言的時候需要學的比較細，比較扎實猜可以。
