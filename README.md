# Project7-3_Hackthissite
## 1063337 張仲緯
### Extbasic 3. FindaFake 1
>Often times you will need to decipher a language which you can not find on google, or is encrypted in some wayI have made up a language for you to decipher. What is the output of this program?
```
BEGIN notr.eal
CREATE intAS 2
DESTROY intAS 0
ANS varAS Create + TO
out TO
```
#### 思路1

#### 思路2

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
>The script's filename is vrfy.phpMake the script reply 1.  
Use the relative path. You don't know any users or emails.
####


## 1063304 陳無忌

## 1061418 葉亭妤

## 1051406 徐崑華

## 1060346 唐瑨
