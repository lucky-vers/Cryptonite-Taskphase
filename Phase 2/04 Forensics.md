# tunn3l v1s10n

# Trivial Flag Transfer Protocol

# MacroHard WeakEdge

**Flag:** `picoCTF{D1d_u_kn0w_ppts_r_z1p5}`

We're given a file `Forensics is fun.pptm`. Since PPT files are essentially zip files, we change its extension to `.zip` and extract it.

```
~/Downloads $ mv Forensics\ is\ fun.pptm Forensics\ is\ fun.zip
renamed 'Forensics is fun.pptm' -> 'Forensics is fun.zip'
~/Downloads $ atool -x Forensics\ is\ fun.zip
Archive:  Forensics is fun.zip
  inflating: Unpack-3555/[Content_Types].xml
  inflating: Unpack-3555/_rels/.rels
  inflating: Unpack-3555/ppt/presentation.xml
  inflating: Unpack-3555/ppt/slides/_rels/slide46.xml.rels
  inflating: Unpack-3555/ppt/slides/slide1.xml
  inflating: Unpack-3555/ppt/slides/slide2.xml
  inflating: Unpack-3555/ppt/slides/slide3.xml
.
.
.
```

Entering the directory `Forensics is fun`, we find a file called `hidden` inside the `ppt/slideMasters` subdirectory. The contents of it are

```
…/Downloads/Forensics is fun/ppt/slideMasters $ cat hidden
Z m x h Z z o g c G l j b 0 N U R n t E M W R f d V 9 r b j B 3 X 3 B w d H N f c l 9 6 M X A 1 f Q
```

It seems to be a base64 cipher. Removing the spaces and using `base64` on it, we get the following output

```
…/Downloads/Forensics is fun/ppt/slideMasters $ sed -i 's/ //g' hidden
…/Downloads/Forensics is fun/ppt/slideMasters $ cat hidden
ZmxhZzogcGljb0NURntEMWRfdV9rbjB3X3BwdHNfcl96MXA1fQ
…/Downloads/Forensics is fun/ppt/slideMasters $ base64 -d hidden
flag: picoCTF{D1d_u_kn0w_ppts_r_z1p5}
```
