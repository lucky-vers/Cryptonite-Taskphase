# New Caesar

# miniRSA

# basic-mod1

**Flag:** `picoCTF{R0UND_N_R0UND_ADD17EC2}`

In this challenge, a message is to be decrypted in the following fashion:

1. Each number is converted to its remainder after division by 37, i.e. *mod 37*
2. The subsequent remainders are then used as indices in the following manner:
    - Digits 0 to 25 are mapped as the uppercase alphabet.
    - Digits 26 to 35 are mapped as the ten digits.
    - 36 is an underscored `_`.

The python code for the decoding using this algorithm is this

```
import string

x = [350, 63, 353, 198, 114, 369, 346, 184, 202, 322, 94, 235, 114, 110, 185, 188, 225, 212, 366, 374, 261, 213] # Contents of `message.txt` converted to an array

keys = []

for i in string.ascii_uppercase + string.digits + '_':
    keys.append(i)

for i in x:
    print(keys[i % 37])

```

Executing it, we get the following output

```
~/Downloads $ python3 main.py
R
0
U
N
D
_
N
_
R
0
U
N
D
_
A
D
D
1
7
E
C
2
~/Downloads $ python3 main.py | tr -d '\n'
R0UND_N_R0UND_ADD17EC2
~/Downloads $
```

So the flag becomes `picoCTF{R0UND_N_R0UND_ADD17EC2}`.
