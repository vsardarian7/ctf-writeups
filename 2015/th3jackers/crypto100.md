### Solved by et0x

For Crypto 100, you are given a 7z file, which when unzipped contains a binary and a file called "crypto.txt" which contains the following text:

Name: Automated Crypter
Description:
    Decrypt this:
    019t-0-080-3-1b-19t-25z-080-03f-8j-1b-12n-12n
    Using this program.
    (Note: the - is just a separator)
Hint: Not all letters chars are crypted

Running the binary without an argument outputs the following --very hostile-- output:

```bash
root@kali:~# ~/Desktop/crypto
Enter the f****** argument
```

Alright!  So we need an argument apparently :)  Upon running the program with single character arguments, you get various different outputs.  After testing each possible character I see that each character in the key we need to decrypt is included in our tests.  So apparently all we need to do in this challenge is translate each part of our "ciphertext" to it's printable character counterpart.  I decided to script this up to make it less tedious.

```
#!/usr/bin/python
import subprocess, string

goal = "019t-0-080-3-1b-19t-25z-080-03f-8j-1b-12n-12n".split("-") # from challenge crypto.txt

a = string.printable[:-5]
d = {}
b = ""
solution = ""

for x in a:
        try:
                b = subprocess.check_output(["./crypto", "'%s'"%x],stderr=None)[14:]
        except:
                pass
        if b.strip() != "": d[b.strip()] = x

for i in goal:
        solution += d[i]

print "[+] Solution: 'flag\{%s\}'"%solution
```


After running our script, we get the flag.

```bash
root@kali:~/Desktop# python crypto.py 
[+] Solution: 'flag{S0.3asy.Chall}'
```

Not really "crypto", but that's how it was solved :)

