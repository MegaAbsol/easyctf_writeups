# 50 - Reversing 1

*Written by Michael Zhang*

## Problem

Hmm this program seems to encrypt a statement depending on your input. Can you reverse it to get the original statement?

`/problems/reversing2` on the shell server.

## Hint

What happens when you change what you input?

## Solution

Well, the problem is down now, but you can still download the files:
https://dl.dropboxusercontent.com/u/51666491/ShareX/2015/10/reversing2.zip
(Note: the c++ source is there too which makes it a bit easier)
(Note: that binary doesn't really work so do a g++ on the source)
First, we run the program. We are prompted with 2 inputs. If we put a non-integer into those inputs, we get back a lot of "a"s. Not really interesting.

But, when we put a number for both of them, cool things happen. After playing around with those numbers for a while, we notice that the first number seems to be a "skip" value, and the second number seems to be which number we should start at.

With this, we can determine the original string (by putting 1 and then 0):
`aararShgoho biprrmup  pd0aomirwish ear lcoextoentit ey'kltiosr.'g get Vsose e'dt  llcsnhhf u  sxlerfhler e aSt.stht  ooe `.
Well, that's not the flag. Let's reverse the program and brute force it in python!
```python
s = "aararShgoho biprrmup  pd0aomirwish ear lcoextoentit ey'kltiosr.'g get Vsose e'dt  llcsnhhf u  sxlerfhler e aSt.stht  ooe "
def decode(s,a,b):
    i = 0
    pos = b
    out = ""
    for i in range(len(s)):
        out += s[pos]
        pos += a
        pos %= len(s)
    return out
for i in range(1,len(s)):
    for start in range(i):
        decrypted = decode(s, i, start)
        if '. ' in decrypted:
            print(decrypted, i, start)
        
```
We can assume the plaintext had spaces after periods, since everyone has good grammar :3
After running, we get some readable text with a skip of 19, but it makes no cohesive sense. Let's try putting 19 back in the program.

```
./reversing2_1
> 19
> 0
ap text with apostrophes that'll make things a li'l bit harder for you.google url shortener code is VfSS0x. here's some cr
```
:O so now we know the plaintext was `google url shortener code is VfSS0x. here's some crap text with apostrophes that'll make things a li'l bit harder for you.`.
So now we go to `https://goo.gl/VfSS0x`. Evidently this link is now broken, but it led to a minecraft world and the flag was something like `notch is proud`. Pay to win much?

## Flag

`notch is proud`






