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

## 细心的大象

下载下来后解压，得到一张图片

日常`binwalk`

发现有rar，`dd`分割一下

````
dd if=1.jpg of=11.rar skip=5005118 bs=1
````

尝试打开rar，发现要密码

去原图里进行信息取证，右键详细信息里，备注中很可疑，尝试base64解密

> MSDS456ASD123zz

使用明文打开压缩包，发现一张png

拉进linux，发现crc报错，按照老套路修改高度

发现flag

flag{a1e5aSA}

---

## 爆照

下载图片，~~woc穹妹~~

日常`binwalk`

发现一大堆，直接`foremost`

打开提取出的zip，发现里面有许多888文件还有个gif

gif好像看到了二维码，`stegsolve`逐帧查看

发现第二帧有二维码，放大，发现太糊了，容差拉大也扫不了，看来是个诱导项

再把目光回到那几个8开头的文件，linux下可以无后缀查看文件

发现就是gif的八帧

在88下端发现了很明显的二维码，扫描得到

> bilibili

继续，发现88,888,8888是jpeg，其余都是bmp，jpeg要注意信息写在属性里，所以查看这三个的属性

~~取证的时候意外发现这图是美图秀秀做的哈哈哈哈~~

> c2lsaXNpbGk=

是base64，解密

> silisili

之后不知道怎么搞了，就全`binwalk`走了一遍，意外发现8888下有压缩包，`dd`分割

````
dd if=8888 of=1.zip bs=1 skip=10976
````

打开压缩包，里面是二维码，扫描得到

> panama

根据题目提示，按顺序拼接字符串，就是flag了

其实解完题目后才发现题目还是稍微给了一点提示的，隐写了信息的888和8888的大小分别是303x299和293x303其余全是303x300，然而发现的太迟了都做完了

flag{bilibili_silisili_panama}

---

## 猫片

~~这题是bugku从安恒那偷的，就是那个只有签到题的比赛~~

**提示：难度与同分数的题目相比较高，完全不是一个水平，如果是新手建议别看writeup硬啃，先做其他题吧。大佬随意**

下载，发现一个文件

`binwalk`走一下，发现真的就是png，改后缀，发现图片是只猫

因为是没人解出来的杂项题，所以比赛才会放出hints，有了hints后题目难度大大下降了，起码有了明确的方向

拉进`stegsolve`

左右切一切色道，发现Blue plane 0，red plane 0和green plane 0头上都有一条，怀疑是隐写

选 analyse >  Data Extract

左侧勾上Blue plane 0，red plane 0和green plane 0，右侧根据提示勾上LSB和BGR

preview一下，看到数据头，那么明显的png，保存bin，名字直接写成11.png

但是png开头是89504E47 ，这里要把多余的删去，拉进`winhex`，删去头上的fffe

之后就能看到png了，是半张二维码

拉进linux，crc校检报错，去改宽高，二维码是方的，所以把高写成和宽一样

之后扫描一下

> https://pan.baidu.com/s/1pLT2J4f

下载下来一个压缩包，打开flag

![](https://img-blog.csdnimg.cn/20190807102413876.png)

心态炸了

看看提示，还有一个NTFS没用上

搜了一下NTFS隐写，发现了NTFS交换数据流的隐写方式

打开`NtfsStreamsEditor`

选择文件夹进行搜索，记得把文本文件解压出来

![](https://img-blog.csdnimg.cn/20190807102658610.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Zoa2pod2Jz,size_16,color_FFFFFF,t_70)

导出，出现了pyc文件

在网上摘抄了一段py和pyc的文章

> 原来Python的程序中，是把原始程序代码放在.py文件里，而Python会在执行.py文件的时候。将.py形式的程序编译成中间式文件（byte-compiled）的.pyc文件，这么做的目的就是为了加快下次执行文件的速度。
>
> 所以，在我们运行python文件的时候，就会自动首先查看是否具有.pyc文件，如果有的话，而且.py文件的修改时间和.pyc的修改时间一样，就会读取.pyc文件，否则，Python就会读原来的.py文件。
>
> 其实并不是所有的.py文件在与运行的时候都会产生.pyc文件，只有在import相应的.py文件的时候，才会生成相应的.pyc文件

接下来回到题目，可以用[在线工具](https://tool.lu/pyc/)解密pyc

````python
import base64


def encode():
    flag = '*************'
    ciphertext = []
    for i in range(len(flag)):
        s = chr(i ^ ord(flag[i]))
        if i % 2 == 0:
            s = ord(s) + 10
        else:
            s = ord(s) - 10
        ciphertext.append(str(s))

    return ciphertext[::-1]


ciphertext = ['96', '65', '93', '123', '91', '97', '22', '93', '70', '102',
              '94', '132', '46', '112', '64', '97', '88', '80', '82', '137',
              '90', '109', '99', '112']
````

这就变成了普通的算法题了。

写个解密脚本

````python
ciphertext = ['96', '65', '93', '123', '91', '97', '22', '93', '70', '102',
              '94', '132', '46', '112', '64', '97', '88', '80', '82', '137',
              '90', '109', '99', '112']
flag = ''
ciphertext.reverse()

for i in range(len(ciphertext)):
    if i % 2 == 0:
        s = int(ciphertext[i]) - 10
    else:
        s = int(ciphertext[i]) + 10
    s = chr(i ^ s)
    flag += s

print(flag)
````

逻辑大概是这样，很简单就不赘述了

![NIVGWT.png](https://s1.ax1x.com/2020/06/30/NIVGWT.png)

flag{Y@e_Cl3veR_C1Ever!}

---

## 多彩

bugku从n1ctf 2018偷的题目，比赛的杂项都是偏门的离谱

当初没做出来，看了writeup后感觉很难受，太偏了，直接贴我看的wp，也是业内老师傅ChaMD5写的wp，我仅仅优化了排版

首先使用隐写神器`Stegsolve`

![img](https://secpulseoss.oss-cn-shanghai.aliyuncs.com/wp-content/uploads/2018/03/beepress-image-69465-1521112115.jpeg)

在中间地带发现了YSL（杨树林，b（￣▽￣）d）这个口红品牌的字样。再继续深入，Analyse→Data Extract

![img](https://secpulseoss.oss-cn-shanghai.aliyuncs.com/wp-content/uploads/2018/03/beepress-image-69465-15211121151.jpeg)

Save Bin保存为一个zip包

![img](https://secpulseoss.oss-cn-shanghai.aliyuncs.com/wp-content/uploads/2018/03/beepress-image-69465-1521112116.jpeg)

这里用winrar打开会报错，得用7z等压缩工具打开才可以。

尝试了下伪加密，无果。于是整个过程就剩下一个密码。一般来说图片隐写的话，要么是二进制里藏了东西，要么就是图形藏了东西。这里二进制里藏了zip包，剩下的密码就只能从图形里入手。图形里是21个颜色格，我分别取色	

```
BC0B28D04179D47A6FC2696FEB8262CF1A77C0083EBC0B28BC0B28D132746A1319BC0B28BC0B28D4121DD75B59DD8885CE0A4AD4121D7E453AD75B59DD8885
```

这里折腾了好久，发现是要找颜色所对应的YSL口红的色号(lll￢ω￢)

搜到一个网址：

https://www.yslbeautyus.com/on/demandware.store/Sites-ysl-us-Site/en_US/Product-Variation?pid=194YSL

![img](https://secpulseoss.oss-cn-shanghai.aliyuncs.com/wp-content/uploads/2018/03/beepress-image-69465-15211121161.jpeg)

这里颜色值可以对应上色号，于是写脚本收集颜色值对应的色号，并把色号转换为二进制，再组合，再bin2text

```python
# -*- coding:utf8 -*-
__author__='pcat@chamd5.org'
import requestsimport re
import libnum

def foo(): 
url=r'https://www.yslbeautyus.com/on/demandware.store/Sites-ysl-us-Site/en_US/Product-Variation?pid=194YSL' 
cont=requests.get(url).content 
# print cont 
pattern=r'YSL_color=(.*?)%20[sS]*?background-color: #(.*?)"' 
rst=re.findall(pattern,cont) 
dYSL={} 
for num,color in rst:  
dYSL[color]=int(num.lstrip('0')) 
lst=['BC0B28','D04179','D47A6F','C2696F','EB8262', 'CF1A77','C0083E','BC0B28','BC0B28','D13274', '6A1319','BC0B28','BC0B28','D4121D','D75B59', 'DD8885','CE0A4A','D4121D','7E453A','D75B59', 'DD8885'] 
flag=''.join('{:b}'.format(dYSL[i]) for i in lst) 
print libnum.b2s(flag) 
pass

if __name__ == '__main__': 
foo() 
print 'ok'
```

打印出来是“白学家”，用7z进行解压缩

![img](https://secpulseoss.oss-cn-shanghai.aliyuncs.com/wp-content/uploads/2018/03/beepress-image-69465-15211121162.jpeg)

解压后打开flag.txt即可。

flag{White_Album_is_Really_worth_watching_on_White_Valentine's_Day}

~~出题人老白学家了，白雪家给爷死！不对我好像也是白学家，那没事了。~~

---

## 旋转跳跃

bugku从极客大挑战偷的题目

先下载解压，尝试播放

？？？这东京热就nm离谱

感觉室友看我的目光都变了

既然是mp3，那么先尝试拉入`Audacity`

没发现啥，仔细看了看题目注释，发现给了个KEY

打开`mp3stego`的根目录

````
Decode.exe -X -P syclovergeek sycgeek-mp3.mp3
````

会输出一个pcm和一个txt，打开txt获得flag

flag{Mp3_B15b1uBiu_W0W}

---

## 普通的二维码

bugku从XJNU偷的题目

解压，发现是张二维码

扫描，发现内容是

> 哈哈!就不告诉你flag就在这里!

用`winhex`打开，发现末尾有一段可疑的数据

> 146154141147173110141166145137171060125137120171137163143162151160164137117164143137124157137124145156137101163143151151041175@xjseck!

@后面是出题人的声明，所以只观察前面的数字，发现最大值是7，怀疑是八进制，应该是八进制转换成字符串

先去查ascii码表

| (二进制)  | Oct(八进制) | Dec(十进制) | Hex(十六进制) | 缩写/字符                   | 解释         |
| --------- | ----------- | ----------- | ------------- | --------------------------- | ------------ |
| 0000 0000 | 00          | 0           | 0x00          | NUL(null)                   | 空字符       |
| 0000 0001 | 01          | 1           | 0x01          | SOH(start of headline)      | 标题开始     |
| 0000 0010 | 02          | 2           | 0x02          | STX (start of text)         | 正文开始     |
| 0000 0011 | 03          | 3           | 0x03          | ETX (end of text)           | 正文结束     |
| 0000 0100 | 04          | 4           | 0x04          | EOT (end of transmission)   | 传输结束     |
| 0000 0101 | 05          | 5           | 0x05          | ENQ (enquiry)               | 请求         |
| 0000 0110 | 06          | 6           | 0x06          | ACK (acknowledge)           | 收到通知     |
| 0000 0111 | 07          | 7           | 0x07          | BEL (bell)                  | 响铃         |
| 0000 1000 | 010         | 8           | 0x08          | BS (backspace)              | 退格         |
| 0000 1001 | 011         | 9           | 0x09          | HT (horizontal tab)         | 水平制表符   |
| 0000 1010 | 012         | 10          | 0x0A          | LF (NL line feed, new line) | 换行键       |
| 0000 1011 | 013         | 11          | 0x0B          | VT (vertical tab)           | 垂直制表符   |
| 0000 1100 | 014         | 12          | 0x0C          | FF (NP form feed, new page) | 换页键       |
| 0000 1101 | 015         | 13          | 0x0D          | CR (carriage return)        | 回车键       |
| 0000 1110 | 016         | 14          | 0x0E          | SO (shift out)              | 不用切换     |
| 0000 1111 | 017         | 15          | 0x0F          | SI (shift in)               | 启用切换     |
| 0001 0000 | 020         | 16          | 0x10          | DLE (data link escape)      | 数据链路转义 |
| 0001 0001 | 021         | 17          | 0x11          | DC1 (device control 1)      | 设备控制1    |
| 0001 0010 | 022         | 18          | 0x12          | DC2 (device control 2)      | 设备控制2    |
| 0001 0011 | 023         | 19          | 0x13          | DC3 (device control 3)      | 设备控制3    |
| 0001 0100 | 024         | 20          | 0x14          | DC4 (device control 4)      | 设备控制4    |
| 0001 0101 | 025         | 21          | 0x15          | NAK (negative acknowledge)  | 拒绝接收     |
| 0001 0110 | 026         | 22          | 0x16          | SYN (synchronous idle)      | 同步空闲     |
| 0001 0111 | 027         | 23          | 0x17          | ETB (end of trans. block)   | 结束传输块   |
| 0001 1000 | 030         | 24          | 0x18          | CAN (cancel)                | 取消         |
| 0001 1001 | 031         | 25          | 0x19          | EM (end of medium)          | 媒介结束     |
| 0001 1010 | 032         | 26          | 0x1A          | SUB (substitute)            | 代替         |
| 0001 1011 | 033         | 27          | 0x1B          | ESC (escape)                | 换码(溢出)   |
| 0001 1100 | 034         | 28          | 0x1C          | FS (file separator)         | 文件分隔符   |
| 0001 1101 | 035         | 29          | 0x1D          | GS (group separator)        | 分组符       |
| 0001 1110 | 036         | 30          | 0x1E          | RS (record separator)       | 记录分隔符   |
| 0001 1111 | 037         | 31          | 0x1F          | US (unit separator)         | 单元分隔符   |
| 0010 0000 | 040         | 32          | 0x20          | (space)                     | 空格         |
| 0010 0001 | 041         | 33          | 0x21          | !                           | 叹号         |
| 0010 0010 | 042         | 34          | 0x22          | "                           | 双引号       |
| 0010 0011 | 043         | 35          | 0x23          | #                           | 井号         |
| 0010 0100 | 044         | 36          | 0x24          | $                           | 美元符       |
| 0010 0101 | 045         | 37          | 0x25          | %                           | 百分号       |
| 0010 0110 | 046         | 38          | 0x26          | &                           | 和号         |
| 0010 0111 | 047         | 39          | 0x27          | '                           | 闭单引号     |
| 0010 1000 | 050         | 40          | 0x28          | (                           | 开括号       |
| 0010 1001 | 051         | 41          | 0x29          | )                           | 闭括号       |
| 0010 1010 | 052         | 42          | 0x2A          | *                           | 星号         |
| 0010 1011 | 053         | 43          | 0x2B          | +                           | 加号         |
| 0010 1100 | 054         | 44          | 0x2C          | ,                           | 逗号         |
| 0010 1101 | 055         | 45          | 0x2D          | -                           | 减号/破折号  |
| 0010 1110 | 056         | 46          | 0x2E          | .                           | 句号         |
| 0010 1111 | 057         | 47          | 0x2F          | /                           | 斜杠         |
| 0011 0000 | 060         | 48          | 0x30          | 0                           | 字符0        |
| 0011 0001 | 061         | 49          | 0x31          | 1                           | 字符1        |
| 0011 0010 | 062         | 50          | 0x32          | 2                           | 字符2        |
| 0011 0011 | 063         | 51          | 0x33          | 3                           | 字符3        |
| 0011 0100 | 064         | 52          | 0x34          | 4                           | 字符4        |
| 0011 0101 | 065         | 53          | 0x35          | 5                           | 字符5        |
| 0011 0110 | 066         | 54          | 0x36          | 6                           | 字符6        |
| 0011 0111 | 067         | 55          | 0x37          | 7                           | 字符7        |
| 0011 1000 | 070         | 56          | 0x38          | 8                           | 字符8        |
| 0011 1001 | 071         | 57          | 0x39          | 9                           | 字符9        |
| 0011 1010 | 072         | 58          | 0x3A          | :                           | 冒号         |
| 0011 1011 | 073         | 59          | 0x3B          | ;                           | 分号         |
| 0011 1100 | 074         | 60          | 0x3C          | <                           | 小于         |
| 0011 1101 | 075         | 61          | 0x3D          | =                           | 等号         |
| 0011 1110 | 076         | 62          | 0x3E          | >                           | 大于         |
| 0011 1111 | 077         | 63          | 0x3F          | ?                           | 问号         |
| 0100 0000 | 0100        | 64          | 0x40          | @                           | 电子邮件符号 |
| 0100 0001 | 0101        | 65          | 0x41          | A                           | 大写字母A    |
| 0100 0010 | 0102        | 66          | 0x42          | B                           | 大写字母B    |
| 0100 0011 | 0103        | 67          | 0x43          | C                           | 大写字母C    |
| 0100 0100 | 0104        | 68          | 0x44          | D                           | 大写字母D    |
| 0100 0101 | 0105        | 69          | 0x45          | E                           | 大写字母E    |
| 0100 0110 | 0106        | 70          | 0x46          | F                           | 大写字母F    |
| 0100 0111 | 0107        | 71          | 0x47          | G                           | 大写字母G    |
| 0100 1000 | 0110        | 72          | 0x48          | H                           | 大写字母H    |
| 0100 1001 | 0111        | 73          | 0x49          | I                           | 大写字母I    |
| 01001010  | 0112        | 74          | 0x4A          | J                           | 大写字母J    |
| 0100 1011 | 0113        | 75          | 0x4B          | K                           | 大写字母K    |
| 0100 1100 | 0114        | 76          | 0x4C          | L                           | 大写字母L    |
| 0100 1101 | 0115        | 77          | 0x4D          | M                           | 大写字母M    |
| 0100 1110 | 0116        | 78          | 0x4E          | N                           | 大写字母N    |
| 0100 1111 | 0117        | 79          | 0x4F          | O                           | 大写字母O    |
| 0101 0000 | 0120        | 80          | 0x50          | P                           | 大写字母P    |
| 0101 0001 | 0121        | 81          | 0x51          | Q                           | 大写字母Q    |
| 0101 0010 | 0122        | 82          | 0x52          | R                           | 大写字母R    |
| 0101 0011 | 0123        | 83          | 0x53          | S                           | 大写字母S    |
| 0101 0100 | 0124        | 84          | 0x54          | T                           | 大写字母T    |
| 0101 0101 | 0125        | 85          | 0x55          | U                           | 大写字母U    |
| 0101 0110 | 0126        | 86          | 0x56          | V                           | 大写字母V    |
| 0101 0111 | 0127        | 87          | 0x57          | W                           | 大写字母W    |
| 0101 1000 | 0130        | 88          | 0x58          | X                           | 大写字母X    |
| 0101 1001 | 0131        | 89          | 0x59          | Y                           | 大写字母Y    |
| 0101 1010 | 0132        | 90          | 0x5A          | Z                           | 大写字母Z    |
| 0101 1011 | 0133        | 91          | 0x5B          | [                           | 开方括号     |
| 0101 1100 | 0134        | 92          | 0x5C          | \                           | 反斜杠       |
| 0101 1101 | 0135        | 93          | 0x5D          | ]                           | 闭方括号     |
| 0101 1110 | 0136        | 94          | 0x5E          | ^                           | 脱字符       |
| 0101 1111 | 0137        | 95          | 0x5F          | _                           | 下划线       |
| 0110 0000 | 0140        | 96          | 0x60          | `                           | 开单引号     |
| 0110 0001 | 0141        | 97          | 0x61          | a                           | 小写字母a    |
| 0110 0010 | 0142        | 98          | 0x62          | b                           | 小写字母b    |
| 0110 0011 | 0143        | 99          | 0x63          | c                           | 小写字母c    |
| 0110 0100 | 0144        | 100         | 0x64          | d                           | 小写字母d    |
| 0110 0101 | 0145        | 101         | 0x65          | e                           | 小写字母e    |
| 0110 0110 | 0146        | 102         | 0x66          | f                           | 小写字母f    |
| 0110 0111 | 0147        | 103         | 0x67          | g                           | 小写字母g    |
| 0110 1000 | 0150        | 104         | 0x68          | h                           | 小写字母h    |
| 0110 1001 | 0151        | 105         | 0x69          | i                           | 小写字母i    |
| 0110 1010 | 0152        | 106         | 0x6A          | j                           | 小写字母j    |
| 0110 1011 | 0153        | 107         | 0x6B          | k                           | 小写字母k    |
| 0110 1100 | 0154        | 108         | 0x6C          | l                           | 小写字母l    |
| 0110 1101 | 0155        | 109         | 0x6D          | m                           | 小写字母m    |
| 0110 1110 | 0156        | 110         | 0x6E          | n                           | 小写字母n    |
| 0110 1111 | 0157        | 111         | 0x6F          | o                           | 小写字母o    |
| 0111 0000 | 0160        | 112         | 0x70          | p                           | 小写字母p    |
| 0111 0001 | 0161        | 113         | 0x71          | q                           | 小写字母q    |
| 0111 0010 | 0162        | 114         | 0x72          | r                           | 小写字母r    |
| 0111 0011 | 0163        | 115         | 0x73          | s                           | 小写字母s    |
| 0111 0100 | 0164        | 116         | 0x74          | t                           | 小写字母t    |
| 0111 0101 | 0165        | 117         | 0x75          | u                           | 小写字母u    |
| 0111 0110 | 0166        | 118         | 0x76          | v                           | 小写字母v    |
| 0111 0111 | 0167        | 119         | 0x77          | w                           | 小写字母w    |
| 0111 1000 | 0170        | 120         | 0x78          | x                           | 小写字母x    |
| 0111 1001 | 0171        | 121         | 0x79          | y                           | 小写字母y    |
| 0111 1010 | 0172        | 122         | 0x7A          | z                           | 小写字母z    |
| 0111 1011 | 0173        | 123         | 0x7B          | {                           | 开花括号     |
| 0111 1100 | 0174        | 124         | 0x7C          | \|                          | 垂线         |
| 0111 1101 | 0175        | 125         | 0x7D          | }                           | 闭花括号     |
| 0111 1110 | 0176        | 126         | 0x7E          | ~                           | 波浪号       |
| 0111 1111 | 0177        | 127         | 0x7F          | DEL (delete)                | 删除         |

先看看14的ascii是换页符

146的ascii是f

再次验证154是l

是flag的前几个字符，已经能看见规律了，三个分为一组然后转为ascii

写个脚本

````python
text = "146154141147173110141166145137171060125137120171137163143162151160164137117164143137124157137124145156137101163143151151041175"
flag = ''

for i in range(0, len(text), 3):
    flag+=chr(int(text[i:i+3], 8))
print(flag)
````

逻辑很简单，就是简单的for循环，注意下步长和切片就行

flag{Have_y0U_Py_script_Otc_To_Ten_Ascii!}

---

## 乌云邀请码

又是bugku从XJNU偷的题目

用`Stegsolve`打开

查看色道，发现Blue plane 0，red plane 0和green plane 0头上都有点点，怀疑是LSB隐写

![](https://img-blog.csdn.net/20181020165922664?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BlcmZlY3QwMDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

选择extract，bitplane选r0 g0 b0，bit order选LSB，Plane order 选BGR（基本上要是出题都是BGR）

发现flag

flag{Png_Lsb_Y0u_K0nw!}

---

## 神秘的文件

bugku咋啥题目都偷啊，山东的官方赛题也偷

先解压，发现一个png和一个压缩包

打开压缩包，发现里面有个一样的png和一个文档，是有密码的

打开`ARCHPR`，进行明文攻击

把logo.png压成zip进行明文攻击

![](https://img-blog.csdnimg.cn/20181205112002504.png)

> q1w2e3r4

输入密码解压文档，打开报错一气呵成

尝试`binwalk`，发现zip，进行`foremost`

在压缩包内找到flag.txt

> ZmxhZ3tkMGNYXzFzX3ppUF9maWxlfQ==

是base64，进行解码

出现flag

flag{d0cX_1s_ziP_file}

---

## 论剑

*这题和新bugku有什么关系呢，新bugku就叫做论剑场*

这题弯路走到有点多，就不写尝试的过程了，直接写成功的步骤

先修改jpeg的高度，之前只做过png的文件分析，没有做过jpeg的，jpeg比png复杂得多。

简单来说，先找**FFC2**，3 字节后即图片的高与宽信息。

[![NT2qW4.png](https://s1.ax1x.com/2020/07/01/NT2qW4.png)](https://imgchr.com/i/NT2qW4)

这个就是高度，把它改大

[![NTRE6A.png](https://s1.ax1x.com/2020/07/01/NTRE6A.png)](https://imgchr.com/i/NTRE6A)

发现了一串字符串，中间被遮挡了几位，而且提示不是flag，应该是某种编码，要解码后才能发现flag

用`winhex`打开，开始寻找可疑信息

在000024D0的末尾开始有了一大串的二进制

[![NTrhsf.png](https://s1.ax1x.com/2020/07/01/NTrhsf.png)](https://imgchr.com/i/NTrhsf)

~~像个鬼一样放在不前不后的地方，还是看别人的wp后才发现的~~

> 01101101 01111001 01101110 01100001 01101101 01100101 01101001 01110011 01101011 01100101 01111001 00100001 00100001 00100001 01101000 01101000 01101000

写个脚本吧

````python
text = "01101101 01111001 01101110 01100001 01101101 01100101 01101001 01110011 01101011 01100101 01111001 00100001 00100001 00100001 01101000 01101000 01101000"
flag = ''

x=text.split(' ')

for i in range(0, len(x)):
    flag+=chr(int(x[i], 2))
print(flag)
````

> mynameiskey!!!hhh

之后把目光转回`winhex`

在刚刚的二进制之后有一串38 7B BC AF 27 1C

[![NTrhsf.png](https://s1.ax1x.com/2020/07/01/NTrhsf.png)

7z压缩文件的文件头是37 7A BC AF 27 1C，都在16进制位上改动了1

修改后提取出7z，解压

需要密码，输入上面用二进制转换的ascii，密码正确

又是一张jpg，试试老办法改高度

成功，出来了和第一张图差不多的效果

![NTqVXT.png](https://s1.ax1x.com/2020/07/01/NTqVXT.png)

左边是1右边是2

能够拼出字符串

> 666C61677B6D795F6E616D655F482121487D

提示这不是字符串，怀疑是某种编码

没有超过F的，怀疑是hex，转换一下

发现flag

flag{my_name_H!!H}

---

## 图穷匕见

下载下来，发现是jpg

先检视详细信息

[![NTj9K0.png](https://s1.ax1x.com/2020/07/01/NTj9K0.png)](https://imgchr.com/i/NTj9K0)

题目叫做图穷匕见，所以以为是改高度

但是拉进`winhex`后发现，文件尾有一大串字符串

一般给出这么多数据，题目又与图片有关，那么肯定是要作图了，而这些数据一般都是坐标信息

仔细分析，没有大于F的，应该是hex

刚开始以为是转换成dec，但是不知道怎么划分，尝试多次后发现是转换成ascii

按惯例是要贴上结果的，但是太长了有35019行，就不放上来了

[![NTxRET.png](https://s1.ax1x.com/2020/07/01/NTxRET.png)](https://imgchr.com/i/NTxRET)

全是坐标，先另存为txt，然后`gnuplot `作图

但是用`gnuplot `有格式要求，得先去掉括号和逗号

直接**ctrl+h**替换，当然也能用python，不过这里没有直接替换方便



处理好后把文本扔到`kali`，运行`gnuplot `

[![N7QoD0.png](https://s1.ax1x.com/2020/07/01/N7QoD0.png)](https://imgchr.com/i/N7QoD0)

![N7lCVK.png](https://s1.ax1x.com/2020/07/01/N7lCVK.png)

做出图后扫描，发现flag

flag{40fc0a979f759c8892f4dc045e28b820}

---

## convert

题目叫做convert，转换的意思

下载后打开

发现全部是0和1，应该是二进制，尝试转换为16进制

发现开头是52617221，也就是rar文件的文件头

保存为rar，然后打开

发现一张jpg，但是没有flag

右键查看属性，在详细信息中发现一段base64

解码后发现flag

flag{01a25ea3fd6349c6e635a1d0196e75fb}

---

## 听首音乐

下载文件，发现是个wav

拖进`Audacity`

发现只有右声道，左声道是一点一点的

怀疑是摩斯电码

短. 长- 空 

表现出来就是

> ..... -... -.-. ----. ..--- ..... -.... ....- ----. -.-. -... ----- .---- ---.. ---.. ..-. ..... ..--- . -.... .---- --... -.. --... ----- ----. ..--- ----. .---- ----. .---- -.-. 

解码，得到flag

flag{5BC925649CB0188F52E617D70929191C}

---

## 好多数值

下载后打开，发现了很多的数值

最大值为255，且有三个数，怀疑是RGB

用PIL作图，把三个数字对应的RGB铺到每个像素上

写个python脚本吧

````python
#-*- coding:utf-8 -*-
from PIL import Image
x =  503#x坐标  
y = 122 #y坐标  x*y = 行数

im = Image.new("RGB",(x,y))#创建图片
file=open('1.txt','r') #打开文件
for i in range(0,x):
    for j in range(0,y):
        line = file.readline()#获取一行
        rgb = line.split(",")#分离rgb
        im.putpixel((i,j),(int(rgb[0]),int(rgb[1]),int(rgb[2])))#rgb转化为像素
im.show()
````

这里的x，y值就是所作图的x，y坐标，是通过对总像素数，也就是RGB文件的行数分解因子得到的

| **Result:** |        |                      |
| ----------- | ------ | -------------------- |
| status      | digits | number               |
| FF          | 5      | 61366 = 2 · 61 · 503 |

一共有61366行，也就是61366个像素，对因子进行排列组合一个一个试过去就能发现flag

![img](https://img-blog.csdnimg.cn/2019081321023916.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Zoa2pod2Jz,size_16,color_FFFFFF,t_70)

flag{youc@n'tseeme}

---

## 普通的数独

bugku从ISCC偷的题目 

题目叫普通的数独，普通个鬼，爪巴！

下载后是一个文件，看了一下文件头，504b是zip，改名后打开解压

发现25张数独图片

到这里开始还是正常操作，下面开始不当人了↓↓↓

先把图片铺成5*5

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200709153549446.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NmMjYwNDY5MDgw,size_16,color_FFFFFF,t_70)

就像这样，然后注意到角落特别像是二维码的定位符，但是顺序不对

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200709153549447.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NmMjYwNDY5MDgw,size_16,color_FFFFFF,t_70)

去把5放21，把21放1，把1放5，就行，然后把有数字的涂黑，就能出现二维码了

当然不想ps也可以用PIL或者gnu画，只是要先手动扒

空白还是留白(255,255,255)，有数字涂黑(0,0,0)

脚本只要在上一题的脚本基础上稍作修改就行了，很简单

但是扒数据就不简单了，头都痛了

> 111111101010101000101000001111110000101111111
> 100000101100111101010011101100011001001000001
> 101110101110011111010011111101000101001011101
> 101110101101100010001010000011110001101011101
> 101110100011100100001111101111111011101011101
> 100000101100100000011000100001110100001000001
> 111111101010101010101010101010101011101111111
> 000000000011001101001000110100110011100000000
> 110011100100100001111111100100101000000101111
> 101001001011111111101110101011110101101001100
> 100000111100100100000110001101001101010001010
> 001100010011010001010011000100000010110010000
> 010110101010001111110100011101001110101101111
> 100011000100011100111011101101100101101110001
> 001100110100000000010010000111100101101011010
> 101000001011010111110011011111101001110100011
> 110111110111011001101100010100001110000100000
> 110101000010101000011101101101110101101001100
> 010011111110001011111010001000011011101101100
> 011001011001010101100011110101001100001010010
> 010111111111101011111111101101101111111111100
> 011110001100000100001000101000100100100011110
> 111110101110011100111010110100110100101010010
> 110010001011101011101000111100000011100010000
> 101011111011100111101111111100001010111110010
> 110100011000111000100111101101111101000100010
> 111101111110001001000011010110001111110111110
> 011001010101000110010100010001000101101010001
> 011101110101101101100100001101101000111101001
> 110110001001101100010101101111110100101100110
> 000011100111000000000100001010101111100010010
> 111010010011110011101110010100001011111010010
> 101001100010111111110100000100001010101010100
> 000010011001001101110101001111100101111101101
> 000010111101110001101011000001000101110100110
> 011110011010100010100000011011000001110010000
> 100110100100001101111111101100101110111110011
> 000000001111110101101000101011100100100011010
> 111111100011111011011010101101110011101011110
> 100000101110101101101000111110010001100010001
> 101110101011100001111111101101001000111111011
> 101110100110111101101000001001101100011101101
> 101110100000011101100001101010110010010010001
> 100000101011001011111011001011000011010110000
> 111111101010101001111011110101101110000101101

````python
# -*- coding:utf-8 -*-
from PIL import Image
x = 45
y = 45

im = Image.new("RGB", (x, y))  # 创建图片
file = open('1.txt', 'r')  # 打开rbg值文件
for i in range(0, x):
    line = file.readline()  # 获取一行
    for j in range(0, y):
        if line[j] == '0':
            im.putpixel((i, j), (255, 255, 255))  # rgb转化为像素
        else:
            im.putpixel((i, j), (0, 0, 0))  # rgb转化为像素
im.show()
````

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200709160652866.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NmMjYwNDY5MDgw,size_16,color_FFFFFF,t_70#pic_center)

`CQR`扫描得到

> Vm0xd1NtUXlWa1pPVldoVFlUSlNjRlJVVGtOamJGWnlWMjFHVlUxV1ZqTldNakZIWVcxS1IxTnNhRmhoTVZweVdWUkdXbVZHWkhOWGJGcHBWa1paZWxaclpEUmhNVXBYVW14V2FHVnFRVGs9



然后base64解码，多次之后发现flag（全是base64，这里不来点其他的没难度啊，浪费好题了，有点虎头蛇尾的感觉）

flag{y0ud1any1s1}

---

