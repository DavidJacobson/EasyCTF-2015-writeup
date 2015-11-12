This is a writeup for EasyCTF 2015. 
-------------------------
##[Programming](programming.md)
##[Web](web.md)
##[Binary Exploitation](binary_exploitation.md)
##[Cryptography](crypto.md)

Firstly, the easter eggs!


If we checkout the robots.txt we find
```
User-agent: *
Disallow: /95125f09551360c5294d180b013d047d.html
```
When we visit that webpage we are greeted with an easter egg interface. I entered the first one that's given, and another I had found but had no use for.
```
egg{gotta_catch_'em_all}
```
I found the other one on Anomat's website, http://anomat.cf/ In the source:
```
egg{lel_st4lk3r_much}
```
