# Domain: General Skills

## Magikarp Ground Mission

We press the `START INSTANCE` button and are given an ssh command to enter

```
ssh ctf-player@venus.picoctf.net -p 52353
```

It's given that we need to login as `ctf-player` with the password `a13b7f9d`. Once we do, we're greeted to this

```
ctf-player@venus.picoctf.net's password:
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 5.4.0-1041-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage
This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

ctf-player@pico-chall$
```

Running `ls`, we get

```
ctf-player@pico-chall$ ls
1of3.flag.txt  instructions-to-2of3.txt
```

Using `cat`, we get the first part of the flag and instructions to the next part

```
ctf-player@pico-chall$ cat *.txt
picoCTF{xxsh_
Next, go to the root of all things, more succinctly `/`
```

So we go to the root folder using `cd`

```
ctf-player@pico-chall$ cd /
ctf-player@pico-chall$ ls
2of3.flag.txt  bin  boot  dev  etc  home  instructions-to-3of3.txt  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

We see two `.txt` files. Running `cat` on them, we get the second part of the flag

```
ctf-player@pico-chall$ cat *.txt
0ut_0f_\/\/4t3r_
Lastly, ctf-player, go home... more succinctly `~`
```

So we go to the home directory. Running `cd` with no arguments takes us there

```
ctf-player@pico-chall$ cd
ctf-player@pico-chall$ ls
3of3.flag.txt  drop-in
```

Running `cat` for the third time, we get

```
ctf-player@pico-chall$ cat *.txt
71be5264}
```

As the third part of the flag.

Combining all the parts, we get the flag as `picoCTF{xxsh_` + `0ut_0f_\/\/4t3r_` + `71be5264}` = `picoCTF{xxsh_0ut_0f_\/\/4t3r_71be5264}`

**Flag:** `picoCTF{xxsh_0ut_0f_\/\/4t3r_71be5264}`

## 2Warm

The problem statement here is simple— to convert the number 42 from base 10 to binary. We use the `bc` (basic calculator) command for this

```
~ $ echo "obase=2; 42" | bc
101010
```

Here, we're setting the `obase` variable to 2, indicating we want to convert the decimal integer to its binary form. As a result, we get `101010`. Thus, our flag becomes `picoCTF{101010}`.

**Flag:** `picoCTF{xxsh_0ut_0f_\/\/4t3r_71be5264}`

