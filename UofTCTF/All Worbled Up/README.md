# Main idea

Pretty easy reverse task, but with some intersting aspects.
From the challenge we have encrpyted flag in hex:

```
                      _     _             
                     | |   | |            
  __      _____  _ __| |__ | | ___ _ __   
  \ \ /\ / / _ \| '__| '_ \| |/ _ \ '__|  
   \ V  V / (_) | |  | |_) | |  __/ |     
    \_/\_/ \___/|_|  |_.__/|_|\___|_|     
                                          
==========================================
Enter flag: *redacted*
Here's your flag:  a81c0750d48f0750
```

And python's byte code, that encryptes our flag.

# Solution

With me teammate, tried to reverse byte code using our hands and Chat-GPT 3.5.
And that code we reconctructed from byte code.

```python
import re

def worble(s):
    s1 = 5
    s2 = 31

    for n in range(len(s)):
        s1 = (s1 + ord(s[n]) + 7) % 65521
        s2 = (s1 * s2) % 65521

    return (s2 << 16) | s1

def shmorble(s):
    r = ''

    for i in range(len(s)):
        r += s[len(s) - i - 1]

    return r

def blorble(a, b):
    result = format(a, 'x') + format(b, 'x')
    return result

flag = input('Enter flag:')

pattern = re.compile('^uoftctf\\{([bdrw013]){9}\\}$')

if pattern.match(flag):
    a = worble(flag)
    b = worble(flag[::-1])
    print("Here's your flag:")
    print(shmorble(blorble(a, b)))
else:
    print('Incorrect format!')
```

# Reverse code

The program takes as input some `flag` with format in regular expression `pattern`.
If flag passes regex, than flag converts to hex format after some mathematical operations in `worble`, `shmorble` and `blorble` functions (I will not break down this function beacuse they are pretty easy to unserstand). 

So from output we have some `encrypted flag` in hex format.

# Solution

We know that flag is in some specified format from regex. So we can write brute force function to test all combinations, encrypted combinations with `worble`, `shmorble` and `blorble` functions like in main program and compare with our original encrypted flag from description. 


# Solution script

```python
import re
import itertools

enc_flag = 'a81c0750d48f0750'
pattern = re.compile('^uoftctf\\{([bdrw013]){9}\\}$')

def worble(s):
    s1 = 5
    s2 = 31

    for n in range(len(s)):
        s1 = (s1 + ord(s[n]) + 7) % 65521
        s2 = (s1 * s2) % 65521           # s2 = 0

    return (s2 << 16) | s1

def shmorble(s):
    r = ''

    for i in range(len(s)):
        r += s[len(s) - i - 1]

    return r

def blorble(a, b):
    result = format(a, 'x') + format(b, 'x')
    return result

def brute_force_flag():
    # all possible combinations of 9 characters from the set {b, d, r, w, 0, 1, 3}
    words = [''.join(i) for i in itertools.product('bdrw013', repeat = 9)]

    for word in words:
        flag = 'uoftctf{' + word + '}'
        a = worble(flag)
        b = worble(flag[::-1])
        # print(blorble(a, b))
        if blorble(a, b) == enc_flag:
            print('Word ', word, ' works!')
            print('Flag: ', flag)
```

![](../../attachments/Pasted%20image%2020240115144420.png)

The interesting thing was in reversing operations from original, because in our brute force function we didnt use `shmorble()` (i tried with `shmorble()` but it falied and nothig was outputed). In some reasons when original encrypted flag from description was generated it didnt use `shmorble()`

