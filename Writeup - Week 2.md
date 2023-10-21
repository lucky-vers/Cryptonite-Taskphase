# Level 12 → 13

The password for this is stored in a hexdump of a file `data.txt` that has been repeatedly compressed.

We first copy the file, and move it to a newly created directory `/tmp/data`.

```
bandit12@bandit:~$ mkdir /tmp/data
bandit12@bandit:~$ cp data.txt /tmp/data
bandit12@bandit:~$ cd /tmp/data
bandit12@bandit:/tmp/data$
```

Using cat on `data.txt`, we can see the hex data
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

We use it with the `-r` to do a reverse hexdump — from hex data to binary data. We then transfer the output to `data.bin`.

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

Another gzip archive. We follow the same steps used in the previous bzip files.
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

Here the `x` argument signifies that we want to extract the archive, and `f` means the next argument will be the file to be extracted.

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
