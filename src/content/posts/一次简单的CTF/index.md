---
title: 一次简单的CTF
published: 2022-01-21 
tags: [CTF, Crypto]
category: NetSecurity
draft: false

---
## 工具脚本
- 凯撒密码
```python
def caesar_offset(s, i):
    if not -25 <= i <= 25:
        return False
    origin = map(lambda c: chr((ord(c) - 65 + i) % 26 + 65) if 65 <= ord(c) <= 90
    else chr((ord(c) - 97 + i) % 26 + 97) if 97 <= ord(c) <= 122
    else c, s)
    return ''.join(origin)
```
- base64
```python
def b64de(s):
    print(" B64 start ".center(64, '-'))
    r = ""
    deb64 = base64.b64decode(s)
    try:
        print("try:utf-8 decode")
        print(deb64.decode("utf-8"))
    except:
        print("\nCantdecode UTF-8")
    print()
    for i in deb64:
        print(('%02x' % i).upper(), end=' ')
    print()
    print("try:chr decode")

    for i in deb64:
        print(chr(i), end='')
        r += chr(i)
    print()
    print(" B64 stop ".center(64, '-'))
    return r
```
- 二进制字符串转ASCII
```python
def binToASCII(bin_str):
    bin_str = bin_str.replace('\n', '').replace(' ', '')
    r = ""
    print(len(bin_str))
    for i in range(len(bin_str) // 8):
        byte_str = bin_str[i * 8:i * 8 + 8]
        r += chr(int(byte_str, 2)) if byte_str != '00000000' else ''
    return r
```
- 黑白图转二进制字符串
```python
def imgToBin(path, mode='B0W1'):
    im = Image.open(path)
    im = im.convert("1")

    bin_str = ""
    black, white = ('0', '1') if mode == 'B0W1' else ('1', '0') if mode == 'B1W0' else ('0', '0')
    for j in range(im.size[1]):
        for i in range(im.size[0]):
            if im.getpixel((i, j)) >= 128:
                bin_str += white
            else:
                bin_str += black
    return bin_str

```

## 源

![源](131.png)
+
[图片隐写术](http://tools.jb51.net/aideddesign/img_add_info/)
=
```txt
hJBCY1PhUCHoSBj6EpFoiJrbSMFuEL5YWAu0VCnMEqdjjfqcCIvbZYyqhYE2Qq0QIqO5SW==

ks6w!
```


ks6w
凯撒+6
```txt
bDVWS1JbOWBiMVd6YjZicDlvMGZoYF5SQUo0PWhGYkxddzkwWCpvTSskbSY2Kk0KCkI5MQ==
```

base64 -d 解出

```txt
l5VKR[9`b1Wzb6bp9o0fh`^RAJ4=hFbL]w90X*oM+$m&6*M

B91
```

base91.decode(s)解出

```txt
https://od.uuu.gay/33.7z  b1Wzb6bp9o0f
```

下载到文件33.7z 密码b1Wzb6bp9o0f
解压出33.wav

Rot36 decode 出二维码

扫得
```txt
l5VKR[9`b1Wzb6bp9o0fh`^R<1}$_YmL/1W<vnhdY%E?hF
```

base91.decode(s)解出

```txt
https://od.uuu.gay/321.7z  Sc3885s38z
```

下载到文件321.7z 密码Sc3885s38z
解压出321.png

扫得
```txt
kwwsv://bkmi.ff/w/Vf3885v38c
```
凯撒+3
```txt
https://yhjf.cc/t/Sc3885s38z
```


