---
title: BUGKU[Misc]
author: Stan
date: 2020-10-22 21:47:33 +0800
categories: [CTF,BugKu]
tags: [Misc]
---

## 签到题

扫描二维码，跳转到公众号，关注后显示Flag

> flag{BugKu-Sec-pwn}

---

## 这是一张单纯的图片

下载图片

将图片拉入`WinHex`，进行审计

发现末端有一段**HTML实体字符**

````
&#107;&#101;&#121;&#123;&#121;&#111;&#117;&#32;&#97;&#114;&#101;&#32;&#114;&#105;&#103;&#104;&#116;&#125;
````

运用`Converter`进行解码

> key{you are right}

---

## 隐写

下载解压，把2.png拉入`WinHex`
这里需要了解PNG的16进制文件结构

| 名称         | 16进制                        |
| ------------ | ----------------------------- |
| 文件头       | 89 50 4E 47 0D 0A 1A 0A       |
|              | 00 00 00 0D 49 48 44 52       |
| 图像宽度     | 00 00 01 F4（像素）           |
| 图像高度     | 00 00 01 A4（像素）           |
| 图像深度     | 08（真彩色图像）              |
| 颜色类型     | 06（带α通道数据的真彩色图像） |
| 压缩方法     | 00                            |
| 滤波器方法   | 00                            |
| 隔行扫描方法 | 00                            |
| CRC          | CB D6 DF 8A                   |

注意图像高度很小，把A4改大为F4，保存
出现flag

> BUGKU{a1e5aSA}

---

## telnet

下载并解压zip，发现一个PCAP文件

用`WireShark`打开

因为题目叫做TELNET，所以着重注意一下telnet协议

追踪一下TCP流，发现flag

>  flag{d316759c281bf925d600be698a4973d5}

---

## 眼见非实

先把文件下载下来

先`Binwalk`跑一下

````
binwalk zip
````

发现是zip，直接改名为1.zip，打开

发现一个叫眼见非实的word文档

继续`Binwalk`跑一下

````
binwalk 眼见非实.docx
````

发现还是zip，改后缀为zip，打开

里面有一个眼见非实的文件夹，里面有一些xml

网上的writeup都是直接去xml里找的，我换种方法

如果你熟悉的话这些xml很容易看出是word文档的xml

新建一个压缩文件，把文件夹里的东西全部扔进去，然后改后缀为**docx**

打开

> Flag在这里呦！

去word选项里开启显示隐藏文字

![NZam6S.png](https://s1.ax1x.com/2020/06/18/NZam6S.png)

发现flag

> flag{F1@g}

---

## 啊哒

下载压缩包，解压出图片

`Binwalk`跑一下

> DECIMAL       HEXADECIMAL     DESCRIPTION
> --------------------------------------------------------------------------------
> 0             0x0             JPEG image data, JFIF standard 1.01
> 30            0x1E            TIFF image data, big-endian, offset of first image directory: 8
> 5236          0x1474          Copyright string: "Copyright Apple Inc., 2018"
> 218773        0x35695         Zip archive data, encrypted at least v2.0 to extract, compressed size: 34, uncompressed size: 22, name: flag.txt
> 218935        0x35737         End of Zip archive, footer length: 22

发现zip，用`dd`分割

```
 dd if=ada.jpg of=1.zip skip=218773 bs=1
```

if=输入文件

of=输出文件

skip=跳过blocks



分割出来了一个zip，打开，发现需要密码

查看原图的属性，发现照相机型号很可疑

> 73646E6973635F32303138

没有一个超过F的，目测16进制，直接Hex转Text

> sdnisc_2018

解压出来，发现flag

> flag{3XiF_iNf0rM@ti0n}

---

## 又一张图片，还单纯吗

下载图片

`binwalk`跑一下

发现很多

尝试`foremost`

````
 foremost 2.jpg
````

然后去**output**文件夹里找

发现两张jpg，其中一张就是flag

> falg{NSCTF-e6532a34928a3d1dadd0b049d5a3cc57}

---

