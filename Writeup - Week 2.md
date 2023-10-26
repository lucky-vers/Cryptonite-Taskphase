# Level 12 → 13

The password for this is stored in a hexdump of a file `data.txt` that has been repeatedly compressed.

We first copy the file, and move it to a newly created directory `/tmp/data`
```
bandit12@bandit:~$ mkdir /tmp/data
bandit12@bandit:~$ cp data.txt /tmp/data
bandit12@bandit:~$ cd /tmp/data
bandit12@bandit:/tmp/data$
```

Using `cat` on `data.txt`, we can see the hex data
```
bandit12@bandit:/tmp/data$ cat data.txt
00000000: 1f8b 0808 6855 1e65 0203 6461 7461 322e  ....hU.e..data2.
00000010: 6269 6e00 013d 02c2 fd42 5a68 3931 4159  bin..=...BZh91AY
00000020: 2653 5948 1b32 0200 0019 ffff faee cff7  &SYH.2..........
00000030: f6ff e4f7 bfbc ffff bff7 ffb9 39ff 7ffb  ............9...
00000040: bd31 eeff b9fb fbbb b9bf f77f b001 3b2c  .1............;,
00000050: d100 0d03 d200 6868 0d00 0069 a00d 0340  ......hh...i...@
00000060: 1a68 00d0 0d01 a1a0 0001 a680 0003 46d4  .h............F.
00000070: 6434 3234 611a 340d 07a4 c351 068f 5000  d424a.4....Q..P.
00000080: 069a 0680 0000 0006 8006 8da4 681a 6868  ............h.hh
00000090: 0d06 8d00 6834 3400 d07a 9a00 01a0 0341  ....h44..z.....A
000000a0: ea1e a190 da40 3d10 ca68 3468 6800 00c8  .....@=..h4hh...
000000b0: 1a1a 1b50 0683 d434 d069 a0d0 3100 d000  ...P...4.i..1...
000000c0: 001e a680 00d0 1a00 d0d0 6864 d0c4 d0d0  ..........hd....
000000d0: 000c 8641 7440 0108 032e 86b4 4cf0 22bb  ...At@......L.".
000000e0: 6682 2b7e b3e2 e98d aa74 dacc 0284 330d  f.+~.....t....3.
000000f0: bbb2 9494 d332 d933 642a 3538 d27e 09ce  .....2.3d*58.~..
00000100: 53da 185a 505e aada 6c75 59a2 b342 0572  S..ZP^..luY..B.r
00000110: 249a 4600 5021 25b0 1973 c18a 6881 1bef  $.F.P!%..s..h...
00000120: 3f9b 1429 5b1d 3d87 68b5 804f 1d28 42fa  ?..)[.=.h..O.(B.
00000130: 16c2 3241 98fb 8229 e274 5a63 fe92 3aca  ..2A...).tZc..:.
00000140: 70c3 a329 d21f 41e0 5a10 08cb 888f 30df  p..)..A.Z.....0.
00000150: f3da ce85 418b 0379 6a65 cfa2 eeb7 9f01  ....A..yje......
00000160: 782c da0e 288b e0c3 fe13 7af5 45ab 2b22  x,..(.....z.E.+"
00000170: a432 bf2f e32d b9e6 1465 2296 d805 a45e  .2./.-...e"....^
00000180: d1c1 eacb 7483 6aac ca0e cf24 8864 bd40  ....t.j....$.d.@
00000190: 118c 644a 1dc6 a127 375c b7a6 c124 bdae  ..dJ...'7\...$..
000001a0: 6d31 63a0 a223 3ea0 61d4 bdf0 450f 56fb  m1c..#>.a...E.V.
000001b0: a546 8d34 08a2 4f1d 43d3 9063 404d dd43  .F.4..O.C..c@M.C
000001c0: b4f2 e65d bcb7 5932 0f5e 6802 3892 a988  ...]..Y2.^h.8...
000001d0: 443d 8e89 7e09 4fb0 499d ee4e 4470 46c0  D=..~.O.I..NDpF.
000001e0: 2ba6 7c62 234a 7f76 151b aec0 23ee 4a97  +.|b#J.v....#.J.
000001f0: bc64 e34c de8a 5724 a1c3 9b89 cd96 1879  .d.L..W$.......y
00000200: d560 0cbb 5c26 09e4 efaf 5b94 402a 7780  .`..\&....[.@*w.
00000210: 4d87 30ce b8a3 946e 72c1 a643 1db7 a060  M.0....nr..C...`
00000220: 6524 629c 0c7e 8e7b e0f8 820c d5cb 60a0  e$b..~.{......`.
00000230: 003c a584 d4c1 61ef eb02 3f65 3a54 a3a2  .<....a...?e:T..
00000240: a565 c154 34c2 b162 d206 1ff8 bb92 29c2  .e.T4..b......).
00000250: 8482 40d9 9010 b3a9 e478 3d02 0000       ..@......x=...
```

A new command is to be used here, `xxd`
```
bandit12@bandit:/tmp/data$ xxd -r data.txt >> data.bin
```

We use it with the `-r` to do a reverse hexdump — from hex data to binary data. We then transfer the output to `data.bin`, the `.bin` extension indicating that it is a generic binary file we don't know the type of yet.

Running the `file` command on `data.bin`, we get
```
bandit12@bandit:/tmp/data$ file data.bin
data.bin: gzip compressed data, was "data2.bin", last modified: Thu Oct  5 06:19:20 2023, max compression, from Unix, original size modulo 2^32 573
```

So this is a compressed gzip file. We first rename it to a file with the `.gz` extension since it shows us an error if we don't
```
bandit12@bandit:/tmp/data$ file data.bin
data.bin: gzip compressed data, was "data2.bin", last modified: Thu Oct  5 06:19:20 2023, max compression, from Unix, original size modulo 2^32 573
bandit12@bandit:/tmp/data$ gzip -d data.bin
gzip: data.bin: unknown suffix -- ignored
```

Then we extract it with the command `gzip`, with the `-d` flag for decompress
```
bandit12@bandit:/tmp/data$ mv data.bin data.gz
bandit12@bandit:/tmp/data$ gzip -d data.gz
bandit12@bandit:/tmp/data$ ls
data  data.txt
```

Running file on the extracted file `data`
```
bandit12@bandit:/tmp/data$ file data
data: bzip2 compressed data, block size = 900k
```

Now this is a bzip/bunzip archive. We use the `bzip` command with the `-d` flag for this
```
bandit12@bandit:/tmp/data$ bzip2 -d data
bzip2: Can't guess original name for data -- using data.out
bandit12@bandit:/tmp/data$ ls
data.out  data.txt
```

Running file the third time, now on `data.out`, we get
```
bandit12@bandit:/tmp/data$ file data.out
data.out: gzip compressed data, was "data4.bin", last modified: Thu Oct  5 06:19:20 2023, max compression, from Unix, original size modulo 2^32 20480
```

Another gzip archive. We follow the same steps used in the previous bzip files
```
bandit12@bandit:/tmp/data$ mv data.out data.gz
bandit12@bandit:/tmp/data$ gzip -d data.gz
bandit12@bandit:/tmp/data$ ls
data  data.txt
```

Now, running `file` on `data` gives us
```
bandit12@bandit:/tmp/data$ file data
data: POSIX tar archive (GNU)
```

This is new, a tarball. We use the following command for it
```
bandit12@bandit:/tmp/data$ tar xf data
bandit12@bandit:/tmp/data$ ls
data  data5.bin  data.txt
```

Here the `x` argument signifies that we want to extract the archive, and `f` means the next argument will be the file to be extracted

`data5.bin` is again a tarball
```
bandit12@bandit:/tmp/data$ file data5.bin
data5.bin: POSIX tar archive (GNU)
bandit12@bandit:/tmp/data$ tar xf data5.bin
bandit12@bandit:/tmp/data$ ls
data  data5.bin  data6.bin  data.txt
```

`data6.bin` is again a bzip archive
```
bandit12@bandit:/tmp/data$ file data6.bin
data6.bin: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/data$ bzip2 -d data6.bin
bzip2: Can't guess original name for data6.bin -- using data6.bin.out
bandit12@bandit:/tmp/data$ ls
data  data5.bin  data6.bin.out  data.bin  data.txt
```

`data6.bin.out` is a tarball
```
bandit12@bandit:/tmp/data$ file data6.bin.out
data6.bin.out: POSIX tar archive (GNU)
bandit12@bandit:/tmp/data$ tar xf data6.bin.out
bandit12@bandit:/tmp/data$ ls
data  data5.bin  data6.bin.out  data8.bin  data.bin  data.txt
```

`data8.bin` is a gzip archive
```
bandit12@bandit:/tmp/data$ file data8.bin
data8.bin: gzip compressed data, was "data9.bin", last modified: Thu Oct  5 06:19:20 2023, max compression, from Unix, original size modulo 2^32 49
bandit12@bandit:/tmp/data$ mv data8.bin data8.gz
bandit12@bandit:/tmp/data$ gzip -d data8.gz
bandit12@bandit:/tmp/data$ ls
data  data5.bin  data6.bin.out  data8  data.bin  data.txt
```

Now `data8` is a plaintext file
```
bandit12@bandit:/tmp/data$ file data8
data8: ASCII text
bandit12@bandit:/tmp/data$ cat data8
The password is wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw
```

We finally get the password of level 13 as `wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw`.

# Level 13 → 14

For this level, we don't get a password but a private SSH key that can be used to login to `bandit14`
```
bandit13@bandit:~$ cat sshkey.private
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAxkkOE83W2cOT7IWhFc9aPaaQmQDdgzuXCv+ppZHa++buSkN+
gg0tcr7Fw8NLGa5+Uzec2rEg0WmeevB13AIoYp0MZyETq46t+jk9puNwZwIt9XgB
ZufGtZEwWbFWw/vVLNwOXBe4UWStGRWzgPpEeSv5Tb1VjLZIBdGphTIK22Amz6Zb
ThMsiMnyJafEwJ/T8PQO3myS91vUHEuoOMAzoUID4kN0MEZ3+XahyK0HJVq68KsV
ObefXG1vvA3GAJ29kxJaqvRfgYnqZryWN7w3CHjNU4c/2Jkp+n8L0SnxaNA+WYA7
jiPyTF0is8uzMlYQ4l1Lzh/8/MpvhCQF8r22dwIDAQABAoIBAQC6dWBjhyEOzjeA
J3j/RWmap9M5zfJ/wb2bfidNpwbB8rsJ4sZIDZQ7XuIh4LfygoAQSS+bBw3RXvzE
pvJt3SmU8hIDuLsCjL1VnBY5pY7Bju8g8aR/3FyjyNAqx/TLfzlLYfOu7i9Jet67
xAh0tONG/u8FB5I3LAI2Vp6OviwvdWeC4nOxCthldpuPKNLA8rmMMVRTKQ+7T2VS
nXmwYckKUcUgzoVSpiNZaS0zUDypdpy2+tRH3MQa5kqN1YKjvF8RC47woOYCktsD
o3FFpGNFec9Taa3Msy+DfQQhHKZFKIL3bJDONtmrVvtYK40/yeU4aZ/HA2DQzwhe
ol1AfiEhAoGBAOnVjosBkm7sblK+n4IEwPxs8sOmhPnTDUy5WGrpSCrXOmsVIBUf
laL3ZGLx3xCIwtCnEucB9DvN2HZkupc/h6hTKUYLqXuyLD8njTrbRhLgbC9QrKrS
M1F2fSTxVqPtZDlDMwjNR04xHA/fKh8bXXyTMqOHNJTHHNhbh3McdURjAoGBANkU
1hqfnw7+aXncJ9bjysr1ZWbqOE5Nd8AFgfwaKuGTTVX2NsUQnCMWdOp+wFak40JH
PKWkJNdBG+ex0H9JNQsTK3X5PBMAS8AfX0GrKeuwKWA6erytVTqjOfLYcdp5+z9s
8DtVCxDuVsM+i4X8UqIGOlvGbtKEVokHPFXP1q/dAoGAcHg5YX7WEehCgCYTzpO+
xysX8ScM2qS6xuZ3MqUWAxUWkh7NGZvhe0sGy9iOdANzwKw7mUUFViaCMR/t54W1
GC83sOs3D7n5Mj8x3NdO8xFit7dT9a245TvaoYQ7KgmqpSg/ScKCw4c3eiLava+J
3btnJeSIU+8ZXq9XjPRpKwUCgYA7z6LiOQKxNeXH3qHXcnHok855maUj5fJNpPbY
iDkyZ8ySF8GlcFsky8Yw6fWCqfG3zDrohJ5l9JmEsBh7SadkwsZhvecQcS9t4vby
9/8X4jS0P8ibfcKS4nBP+dT81kkkg5Z5MohXBORA7VWx+ACohcDEkprsQ+w32xeD
qT1EvQKBgQDKm8ws2ByvSUVs9GjTilCajFqLJ0eVYzRPaY6f++Gv/UVfAPV4c+S0
kAWpXbv5tbkkzbS0eaLPTKgLzavXtQoTtKwrjpolHKIHUz6Wu+n4abfAIRFubOdN
/+aLoRQ0yBDRbdXMsZN/jvY44eM+xRLdRVyMmdPtP8belRi2E2aEzA==
-----END RSA PRIVATE KEY-----
```

We use the `ssh` command once again, this time inside our SSH session
```
bandit13@bandit:~$ ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
The authenticity of host '[bandit.labs.overthewire.org]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit13/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit13/.ssh/known_hosts).
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames
.
.
.
bandit14@bandit:~$
```

The `-i` flag indicates that a private key file is to be read for authentication.

# Level 14 → 15

The password for level 15 is found by submitting the password of the current level to port 30000 on localhost. Here, we use the netcat (`nc`) command, and send a message to the first argument (`localhost`) with the port number in the second one (`30000`)
```
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14 | nc localhost 30000
Correct!
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt
bandit14@bandit:~$
```

So the password for level 15 is `jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt`.


# Level 15 → 16

The password for level 16 is found by sending the current level's password to port 30001 on localhost using SSL encryption

We use `openssl` here, the CLI implementation of the popular network library OpenSSL
```
bandit15@bandit:~$ openssl s_client -connect localhost:30001
```

The command structure is as follows
- `s_client` tells OpenSSL to act as an SSL/TLS client.
- `-connect` connects the client to the given network.
- `localhost:30001` is the hostname (`localhost`) and the port (`30001`) of the connection we're trying to make.

The output of the previous command and its result upon entering the current password is
```
.
.
.
    0050 - 3e 70 75 e9 dc 8f 85 17-54 3b 73 38 e1 86 d1 f8   >pu.....T;s8....
    0060 - 46 c8 31 be b7 bc c3 e4-d2 61 2d e4 70 ee 35 18   F.1......a-.p.5.
    0070 - 82 70 59 cf 70 e4 1e ba-5d c4 2d 00 42 4b 25 6b   .pY.p...].-.BK%k
    0080 - 81 c4 98 e0 a7 60 34 98-bd 29 d7 f2 60 b2 b9 74   .....`4..)..`..t
    0090 - 5b 45 fc f2 d9 bc fe 23-a3 67 a3 55 8d bb fc 08   [E.....#.g.U....
    00a0 - 25 3f ac 4c d8 27 d2 76-38 d9 79 e9 45 8b 96 31   %?.L.'.v8.y.E..1
    00b0 - 14 4f e0 c3 72 8b c1 d0-68 40 5a ad 38 8d 68 f4   .O..r...h@Z.8.h.
    00c0 - 44 5c 20 fb 73 c4 47 dc-72 dc 2e ba 53 b5 1a 1b   D\ .s.G.r...S...

    Start Time: 1697917064
    Timeout   : 7200 (sec)
    Verify return code: 10 (certificate has expired)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt
Correct!
JQttfApK4SeyHwDlI9SXGR50qclOAil1

closed
```

The sixteenth password is thus found to be `JQttfApK4SeyHwDlI9SXGR50qclOAil1`.

# Level 16 → 17

It is said that the credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. We use the program `nmap` to scan all the ports within that range and find out which ones are open
```
bandit16@bandit:~$ nmap -p 31000-32000 localhost
Starting Nmap 7.80 ( https://nmap.org ) at 2023-10-21 19:47 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00011s latency).
Not shown: 996 closed ports
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 0.06 seconds
```

The open ports are shown to be `31046`, `31518`, `31691`, `31790`, and `31960`.

Running the command `openssl s_client -connect localhost:PORTNUMBER` replacing `PORTNUMBER` with the numbers above, we finally get the following output with port `31790`
```
bandit16@bandit:~$ openssl s_client -connect localhost:31790
.
.
.
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
JQttfApK4SeyHwDlI9SXGR50qclOAil1
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----

closed
```

It turns out to not be a password but an RSA key, which can be used to log in to level 17. We copy it and save it to a local file.

# Level 17 → 18

We login to the server with the RSA key
```
ssh -i ../OTW/otw_17.key bandit17@bandit.labs.overthewire.org -p 2220
```

The password is given to be the only line changed between `passwords.old` and `passwords.new`. So, we use the `diff` command
```
bandit17@bandit:~$ ls
passwords.new  passwords.old
bandit17@bandit:~$ diff pass*
42c42
< hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg
---
> p6ggwdNHncnmCNxuAt0KtKVq185ZU7AW
```

Hence the password comes out to be `hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg`.

Logging into the server, we get the following result
```
~ $ ssh bandit18@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password:
.
.
.
 in the following locations:

    * gef (https://github.com/hugsy/gef) in /opt/gef/
    * pwndbg (https://github.com/pwndbg/pwndbg) in /opt/pwndbg/
    * peda (https://github.com/longld/peda.git) in /opt/peda/
    * gdbinit (https://github.com/gdbinit/Gdbinit) in /opt/gdbinit/
    * pwntools (https://github.com/Gallopsled/pwntools)
    * radare2 (http://www.radare.org/)

--[ More information ]--

  For more information regarding individual wargames, visit
  http://www.overthewire.org/wargames/

  For support, questions or comments, contact us on discord or IRC.

  Enjoy your stay!

Byebye !
Connection to bandit.labs.overthewire.org closed.
```

It seems someone modified the `.bashrc` to log us out whenever we attempt log in with SSH. To sidestep this, we use the `-t` flag to log in using `/bin/sh` instead of `/bin/bash`.

```
~ $ ssh bandit18@bandit.labs.overthewire.org -p 2220 -t /bin/sh
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password:
$
```

We are greeted to a plain shell, with none of the comfort features of bash.

# Level 18 → 19

The password is just stored in plaintext on a file named `readme`. As we've already logged in and bypassed the malicious `.bashrc`, we just need to run `cat` on `readme`.

```
$ cat readme
awhqfNnAbc1naukrpqDYcF95h7HoMTrC
```

So the password for the next level is `awhqfNnAbc1naukrpqDYcF95h7HoMTrC`.

# Level 19 → 20

Here, we're given a setuid binary named `bandit20-do` that allows us to run commands as another user. We use this to access the password for `bandit20` directly
```
bandit19@bandit:~$ ./bandit20-do
Run a command as another user.
  Example: ./bandit20-do id
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
VxCazJaVykI6W36BkBU0mJTCM8rR95XT
```

The password for level 20 turns out to be `VxCazJaVykI6W36BkBU0mJTCM8rR95XT`.

# Level 20 → 21

Here, we have an binary, `suconnect` which connects to the localhost on the port specified in the first argument, reads a line of text from it. If its equal to the password for `bandit20`, it transmits the password for `bandit21`.
We `echo` the current password into the `nc` tool, setup a listener on a port, and run `suconnect` on the same port to get the password
```
bandit20@bandit:~$ echo VxCazJaVykI6W36BkBU0mJTCM8rR95XT | nc -l localhost 29000 &
[1] 1852238
bandit20@bandit:~$ ./suconnect 29000
Read: VxCazJaVykI6W36BkBU0mJTCM8rR95XT
Password matches, sending next password
NvEJF7oVjkddltPSrdKEFOllh9V1IBcq
[1]+  Done                    echo VxCazJaVykI6W36BkBU0mJTCM8rR95XT | nc -l localhost 29000
```

Here, the `-l` flag is used to setup a listener to `localhost` on port `29000`, `&` is used for running an app in the background, and `./suconnect 29000` for obtaining the password.

The password comes out to be `NvEJF7oVjkddltPSrdKEFOllh9V1IBcq`.

# Level 21 → 22

We receive a hint to look into the `/etc/cron.d/` folder, so we `cat` all the files from the folder
```
bandit21@bandit:~$ cat /etc/cron.d/*
* * * * * root /usr/bin/cronjob_bandit15_root.sh &> /dev/null
* * * * * root /usr/bin/cronjob_bandit17_root.sh &> /dev/null
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * root /usr/bin/cronjob_bandit25_root.sh &> /dev/null
30 3 * * 0 root test -e /run/systemd/system || SERVICE_MODE=1 /usr/lib/x86_64-linux-gnu/e2fsprogs/e2scrub_all_cron
10 3 * * * root test -e /run/systemd/system || SERVICE_MODE=1 /sbin/e2scrub_all -A -r
cat: /etc/cron.d/otw-tmp-dir: Permission denied
# The first element of the path is a directory where the debian-sa1
# script is located
PATH=/usr/lib/sysstat:/usr/sbin:/usr/sbin:/usr/bin:/sbin:/bin

# Activity reports every 10 minutes everyday
5-55/10 * * * * root command -v debian-sa1 > /dev/null && debian-sa1 1 1

# Additional run at 23:59 to rotate the statistics file
59 23 * * * root command -v debian-sa1 > /dev/null && debian-sa1 60 2
```

We attempt to `cat` all the shell scripts with `bandit` in their name. Only `/usr/bin/cronjob_bandit22.sh` lets us without throwing any permission errors.
```
bandit21@bandit:~$ cat /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

Running `cat` on the file mentioned in the script, we get
```
bandit21@bandit:~$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
WdDozAdTM2z9DiFEQ2mGlwngMfj4EZff
```

Therefore, `WdDozAdTM2z9DiFEQ2mGlwngMfj4EZff` is the password for `bandit22`.
