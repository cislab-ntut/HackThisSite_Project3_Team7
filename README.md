# Project7-3_Hackthissite
## 1063337 張仲緯
### Extbasic 3. FindaFake 1
>Often times you will need to decipher a language which you can not find on google, or is encrypted in some wayI have made up a language for you to decipher. What is the output of this program?  

>某人設計了一種程式語言，寫了一支只有五行的程式，希望你去分析這支程式會輸出什麼內容
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
6. 首先嘗試內容中的關鍵字組合，TO、20、20TO、2TO、AS、ANS、var等皆錯誤，**經過以上粗體字的思路後得到答案為2**
#### 其他思路
後來試著搜尋其他人的想法，看到其中一種想法是當作組合語言來思考
1. 第一行一樣是function宣告
2. 第二、三行將其理解為PUSH變數2、0到名為`AS`的stack裡面
3. 第四行是將`AS`的變數POP兩個出來放到`TO`，此時`TO=02`也就是2
4. 輸出變數`TO`
#### 總結
由於題目只有給五行的範例程式，難以分析出此程式語言的詳細語法規範，因此可以推測這是在考我們分析的同時發揮一點想像力，也並不需要全部依照關鍵字的英文意義去推論，在這個網站上過於死板的黑客過程只會讓自己心態炸裂。
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
>The script's filename is vrfy.phpMake the script reply 1.  Use the relative path. You don't know any users or emails.
#### 思路



## 1063304 陳無忌

## 1061418 葉亭妤

## 1051406 徐崑華

## 1060346 唐瑨
