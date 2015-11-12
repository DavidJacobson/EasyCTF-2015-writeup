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
```ks
