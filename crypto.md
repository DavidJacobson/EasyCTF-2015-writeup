Crypto
----------

Hardwood Floors -  200 pts
---------------------

This was a fun problem, we are given the following code:
```py
message = "<redacted>"
key = 3
encrypted = ' '.join([str(ord(c)//key) for c in message])
print(encrypted)
```
and this corresponding cipher text:
```
27 39 33 34 10 36 32 33 35 10 37 34 10 38 35 34 37 38 15 15 15 10 33 32 38 40 33 38 34 41 34 36 16 16 38 31 33 16 39 35 38 35 16 36 41
```
To see whats happening we just look at the code. For each character in the string to be 'encrypted', it is convered to an ordinal and divided by three.
What's interesting is the use of floor division. This means the value following the . is truncated. For example, both 4.999999 and 4.000001 when floor divided by 2 are 2.
This means there are 3 different possible values for each character (9//3 = 3, 10//3=3, 11//3=3).

So we need to compute all possible values for each character.

```py
import string
import itertools

key = 3
keys = {} # Dict to store values
for each in string.printable: # All possible values that could have been encrypted
	if not str(ord(each) // key) in keys.keys():
		keys[str(ord(each) // key)] = [each]
	else:
		keys[str(ord(each) // key)].append(each)
#print keys
message = "27 39 33 34 10 36 32 33 35 10 37 34 10 38 35 34 37 38 15 15 15 10 33 32 38 40 33 38 34 41 34 36 16 16 38 31 33 16 39 35 38 35 16 36 41"
message = message.split()
dec = [keys[x] for x in message]
for each in dec:
	print "|".join(each) # Print out a graph

```
Now we have all the possible values for each char, and it looks like this: (I put my guesses next to each place)
```
c|d|e e 
a|b|` a 
r|s|t s 
x|y|z y 
c|d|e c 
r|s|t t 
f|g|h f 
{|||} { 
f|g|h f 
l|m|n l 
0|1|2 0 
0|1|2 0 
r|s|t r 
]|^|_ _ 
c|d|e d 
0|1|2 1
u|v|w v
i|j|k i
r|s|t s
i|j|k i
0|1|2 0
l|m|n n
{|||} } 
```
We know the first characters will be easyctf{ and the last will be }, and from there it's just a matter of choosing the best possible fit.
```
flag: easyctf{fl00r_d1visi0n}
```

Pixels - 180 pts
--------------
This problem had two images, both titled 'mystery' with just a - between them.
I decided to take this literally, and attempt to subtract the images from eachother. This was acomplished using imagemagick, 
```zsh
redacted@redacted> convert mystery1.png mystery2.png -compose d
ifference -composite -colorspace Gray new.png
```


![lolmd](https://raw.githubusercontent.com/DavidJacobson/EasyCTF-2015-writeup/master/img/new.png)


And we get a picture with the flag
```
flag: easyctf{PRETTY_PIXEL_MATH}
```
