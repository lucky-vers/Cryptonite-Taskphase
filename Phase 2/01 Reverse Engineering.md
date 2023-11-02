# keygenme-py

**Key:** `picoCTF{1n_7h3_|<3y_of_ac73dc29}`

For this, we're given a file `keygenme-trial.py`. In its source code block, we're given the following variable declarations

```
username_trial = "FRASER"
bUsername_trial = b"FRASER"

key_part_static1_trial = "picoCTF{1n_7h3_|<3y_of_"
key_part_dynamic1_trial = "xxxxxxxx"
key_part_static2_trial = "}"
key_full_template_trial = key_part_static1_trial + key_part_dynamic1_trial + key_part_static2_trial
```

It seems that the flag we want is in the template of `key_full_template_trial`.

There's also a function `check_key` that presumably we need to test with to check whether the flag/key we enter is also a valid activation key or not. It performs the following checks

```
        # Check static base key part --v
        i = 0
        for c in key_part_static1_trial:
            if key[i] != c:
                return False

            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[4]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[5]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[3]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[6]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[2]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[7]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[1]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[8]:
            return False

```

So it seems it checks whether the dynamic part of the given key is equal to the combination of elements of the variable `hashlib.sha256(username_trial).hexdigest()` with the following indices:

- 4
- 5
- 3
- 6
- 2
- 7
- 1
- 8

The value of `hashlib.sha256(username_trial).hexdigest()` is controlled by the value of `username_trial`, which in this case is `b"FRASER"`. Using this, we can get its value

```
~ $ python3
Python 3.11.5 (main, Sep  2 2023, 18:29:07) [GCC 13.2.1 20230801] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import hashlib
>>> username_trial = b"FRASER"
>>> hashlib.sha256(username_trial).hexdigest()
'92d7ac3c9a0cf9d527a5906540d6c59c80bf8d7ad5bb1885f5f79b5b24a6d387'
```

Looping through the required indices on the hashed value, we get

```
>>> for i in [4, 5, 3, 6, 2, 7, 1, 8]:
...     print(hashlib.sha256(username_trial).hexdigest()[i])
...
a
c
7
3
d
c
2
9
```

Therefore the value of the dynamic key turns out to be `ac73dc29`, leading to the full key being `picoCTF{1n_7h3_|<3y_of_ac73dc29}`.

Using it on `keygenme-trial.py`, we get a successful activation

```
~/Downloads $ python3 keygenme-trial.py

===============================================
Welcome to the Arcane Calculator, FRASER!

This is the trial version of Arcane Calculator.
The full version may be purchased in person near
the galactic center of the Milky Way galaxy.
Available while supplies last!
=====================================================


___Arcane Calculator___

Menu:
(a) Estimate Astral Projection Mana Burn
(b) [LOCKED] Estimate Astral Slingshot Approach Vector
(c) Enter License Key
(d) Exit Arcane Calculator
What would you like to do, FRASER (a/b/c/d)? c

Enter your license key: picoCTF{1n_7h3_|<3y_of_ac73dc29}

Full version written to 'keygenme.py'.

Exiting trial version...

===================================================

Welcome to the Arcane Calculator, tron!
```

# GDB baby step 0

# ARMssembly 0
