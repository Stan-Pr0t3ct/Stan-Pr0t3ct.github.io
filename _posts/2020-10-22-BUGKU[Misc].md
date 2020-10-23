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

## 猜

下载图片
注意题目描述

> flag格式key{某人名字全拼}

扔去识图
![NZT4Ej.png](https://s1.ax1x.com/2020/06/18/NZT4Ej.png)
发现了原图

> flag{liuyifei}

---

## 宽带信息泄露

没有做过的类型
看起来像是制式文件，先搜索一下
由搜索结果可知是路由器配置文件
根据提示去下载`routerpassview`
再看题目提示，得知要提交用户名
`ctrl+F`搜索username

> < Username val=053700357621 />

> flag{053700357621}

---

## 隐写2

下载图片

惯例先用`binwalk`跑一遍

发现有zip，用dd分割

````
 dd if=Welcome_.jpg  of=1.zip skip=52516 bs=1
````

解压得到flag.rar和提示

提示说密码是三位数，直接`ARCHPR`

爆破可得密码为871

解压可得一张jpg

用`winhex`打开

最下面发现

> f1@g{eTB1IEFyZSBhIGhAY2tlciE=}

b64加密，解密可得flag

> flag{y0u Are a h@cker!}

---

## 多种方法解决

下载后解压缩，发现一个exe

无法执行，拉进`winhex`

> data:image/jpg;base64,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

这个打头的是base64图片，[在线工具](http://tool.chinaz.com/tools/imgtobase/)跑一下

下载图片，`CQR`扫描

KEY{dca57f966e4e4e31fd5b15417da63269}

---

## 闪的好快

下载图片，发现是gif，二维码会动

拉进`stegsolve`

选择**Analyse**的**Frame browser**

一共18帧

逐帧扫

SYC{F1aSh-so-f4sT}

---

## come_game

因为题目分类不是逆向而是杂项，故使用杂项的方法解决，本题也可以使用逆向的方法。

下载压缩包后解压

得到一个`exe`和一个`__MACOSX`

__MACOSX文件是在苹果系统下压缩时产生的隐藏文件，苹果不可见win可见，不用在意。

运行exe，发现是一个游戏

通关游戏应该就会有flag，但是手残党不想努力了

用`WinHex`打开**save1**，看这个名字应该是存档文件

```
2  D
```

目测2是所在的关卡数，尝试修改为3

运行游戏，load后果然出现在了第三关

照本宣科

当改成5时，就可以看见flag了

flag{6E23F259D98DF153}

---

## 白哥的鸽子

下载下来

惯例`binwalk`跑一下，结果发现真的是一个jpg

改成1.jpg就能看到图像了

`winhex`跑一下，发现末尾段有些可疑

> fg2ivyo}l{2s3_o@aw__rcl@

因为顺序是错的，但是出现了flag和@{}这些flag格式的字符

尝试枚举栅栏密码

> 1：f2vol23oa_rlgiy}{s_@w_c@
> 2：fio{3@_cgv}2_a_l2ylsowr@
> 3：fvl3argy{_wc2o2o_li}s@_@
> 4：fo3_g}__2lori{@cv2alysw@
> 5：flag{w22_is_v3ry_cool}@@
> 6：f3g_2oi@vaywo_}_lr{c2ls@

也就是每组字符为3时解密成功

下面进行正向推导

> 1. flag{w22_is_v3ry_cool}@@
>
> 2. fla
>
>    g{w
>
>    22_
>
>    is_
>
>    v3r
>
>    y_c
>
>    ool
>
>    }@@
>
> 3. fg2ivyo}l{2s3_o@aw__rcl@

最后两个@是为了补齐，详见上面推导过程，所以提交时去掉就行

flag{w22_is_v3ry_cool}

---

## linux

解压，得到一个flag文件

`strings`输出可打印字符串

一步到位，发现flag

题目说是linux，但是完全没有用到linux啊，难道是想考察linux解压.tar.gz吗

````
tar -zxf *.tar.gz
````

flag{feb81d3834e2423c9903f4755464060b}

---

## 隐写3

解压得到图片，拉进kali看一下

crc错误，被改过宽高

拉进`winhex`，修改高度

具体分析请看曾经的隐写，wp中详述了png的16进制特点，这里不再赘述

flag{He1I0_d4_ba1}



---

## 做个游戏

~~虽然很简单但还应该算是逆向题，不知道扔在杂项是什么意思。~~

下载后，发现是个jar（java的归档文件）

在有JRE环境的情况下可以直接运行（JDK自带JRE，写java的肯定都装过，部分IDE也会自带，没有请自行安装）

双击打开猜测程序逻辑，死亡后显示成绩和不同的话，猜测可能有if判断，成绩达到一个值时输出flag

和以前一样，没有耐心玩，直接拆

因为jar是以ZIP格式构建的，所以直接用压缩工具拆也行，但是比较麻烦，我的建议是`jd-gui`，先熟悉一下这个软件，今后安卓逆向的时候经常会用到的（为什么？安卓和java的关系我就不用说了吧）

先去找主函数，cn.bjsxt > plane > PlaneGameFrame.class > PlaneGameFrame > main(String[]) : void

按着名字很容易就能找到主函数了，查看逻辑

````java
if (!this.p.isLive())
    {
      printInfo(g, "兄弟就死了吗", 50, 150, 200);
      
      int period = (int)((this.endTime.getTime() - this.startTime.getTime()) / 1000L);
      printInfo(g, "你的持久度才" + period + "秒", 50, 150, 250);
      switch (period / 10)
      {
      case 0: 
        printInfo(g, "真.头顶一片青青草原", 50, 150, 300);
        break;
      case 1: 
        printInfo(g, "这东西你也要抢着带？", 50, 150, 300);
        break;
      case 2: 
        printInfo(g, "如果梦想有颜色，那一定是原谅色", 40, 30, 300);
        break;
      case 3: 
        printInfo(g, "哟，炊事班长呀兄弟", 50, 150, 300);
        break;
      case 4: 
        printInfo(g, "加油你就是下一个老王", 50, 150, 300);
        break;
      case 5: 
        printInfo(g, "如果撑过一分钟我岂不是很没面子", 40, 30, 300);
        break;
      case 6: 
        printInfo(g, "flag{RGFqaURhbGlfSmlud2FuQ2hpamk=}", 50, 150, 300);
        break;
      }
````

好吧不需要查看逻辑，直接就有flag了，可能这就是题目放在杂项的原因吧。

但是我还是简单的说一下逻辑

switch很简单都熟悉，period就是经过的秒数，值得注意的是java中的除法运算是不计算余数的，比如11/3=3

得到flag后别急着提交，看括号里，是一串base64

解密一下再包上flag和花括号提交

flag{DajiDali_JinwanChiji}

---

## 想蹭网先解开密码

~~又是爷最烦的流量追踪题，还好很简单~~

根据提示，要破解密码，且给了前7位，提示是手机号所以长度也知道了，并且最后几位也是数字，妥妥的提示你要爆破啊

接下来看题，先下载，得到一个**cap**文件

**cap**和**pcap**文件都是抓包文件，用`wireshark`打开

提示是wifi密码，wifi验证走的是wpa的四次握手，协议叫做eapol

![img](https://img-blog.csdn.net/20160619143020385?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

大一学过的tcp是三次握手四次挥手，wpa是四次握手，不要搞混了哦

直接在wireshark搜索EAPOL协议，就能看到这个过程了，正好筛选出4个结果

当然，要爆破密码wireshark是不行的，接下来上`kali`

要爆破首先要一个字典，字典攻击在爆破中非常重要

根据已知情报（13910400000~13910409999）写一个字典

```
with open('123.txt','w+') as f:
    for i in range(0,10):
        for j in range(0,10):
            for k in range(0,10):
                for h in range(0,10):
                    f.write('1391040'+str(i)+str(j)+str(k)+str(h)+'\n')
f.close
```

这是用python写的，当然我个人更喜欢`crunch`,但是对新手不友好~~第一次用的时候生成了个30多t的字典，还好ctrl c按得快~~

```
crunch 11 11 -t 1391040%%%% -o 123.txt
```

第一个数字代表开始长度，第二个数字代表结束长度，因为手机号码11位所以不变，-t是定义输出格式，-o就是输出成文件

'%'是统配符，代表任意数字，类似的还有

'@'：小写字母

',': 大写字母

'^'：符号

字典好了接下来请出今天的大头`aircrack`

就像fcrackzip专门用来炸zip，aircrack专门炸WEP、WPA，连命令都差不多

```
aircrack-ng wifi.cap -w 123.txt 
```

-w 指定字典

之后选择目标网络，输入3，就是有wpa加密的那个网络的序号

KEY FOUND! 

包一下flag和花括号就能提交了

flag{13910407686}

---

## linux2

这让我怎么给你洗啊，两道linux题都不需要linux

下载后得到文件

根据提示直接找字符串

windows在cmd下跑这个 

````
 strings brave | findstr KEY
````

linux在terminal跑这个

````
 strings brave | grep KEY
````

大同小异

题目其实还有诱导

`binwalk`可以发现有图片

`foremost`分离一下

是一个带着flag的图片，但是根据题目的提示可知这是干扰项

flag{24f3627a86fc740a7f36ee2c7a1c124a}

---



