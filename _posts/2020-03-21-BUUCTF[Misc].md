---
title: BUUCTF[Misc]
author: Stan
date: 2020-03-21 14:10:03 +0800
categories: [CTF,BUUCTF]
tags: [Misc]

---

## 签到

flag写在上面

 flag{buu_ctf}

---

## 金三胖

用stegosolver的frame browser，可以看出

21：flag{

![image-20201011100734557](https://i.loli.net/2020/10/11/EBmTivxz9XnOYFy.png)

51:he11o

![image-20201011100752325](https://i.loli.net/2020/10/11/iWfjaN4kF2xgXQJ.png)

79:hongke}

![image-20201011100816060](https://i.loli.net/2020/10/11/6JF3uRrgkYhidCZ.png)

flag{he11ohongke}

---

## 二维码

下载发现是张二维码

![image-20201011101303846](https://i.loli.net/2020/10/11/DQAdKZFWuSC4nlY.png)

> secret is here

`binwalk`走一下

![image-20201011101433054](https://i.loli.net/2020/10/11/dsN3xkOnUP2iDZX.png)

分离出压缩包，发现需要密码

`ARCHPR`爆破一下

![image-20201011101645409](https://i.loli.net/2020/10/11/58P7NBDzMqHI2ZQ.png)

解压出来的文档里就是flag

---

## N种方法解决

先双击，发现无法执行

`winhex`看一下

![image-20201011102128513](https://i.loli.net/2020/10/11/vClOTV9pZh3BWuG.png)

这是base64图片，用在线工具转一下

![image-20201011102239339](https://i.loli.net/2020/10/11/sqNbHlZmVLGkWnM.png)

扫描二维码

![image-20201011102306388](https://i.loli.net/2020/10/11/Z1XDnEKsOwbRIp2.png)

发现flag

---

## 大白

看这提示像是修改了高度

![image-20201011102633893](https://i.loli.net/2020/10/11/RXzeopliYQrgLPt.png)

修改一下后保存

![image-20201011102826591](https://i.loli.net/2020/10/11/rQ3ePKOAZXovnzf.png)

发现flag，太常规了

---

## 你竟然赶我走

`winhex`打开，翻到最后

![image-20201011103104520](https://i.loli.net/2020/10/11/rf1EvDcu6UtLNlo.png)

发现flag，太常规了

---

## 基础破解

提示是四位数字加密

`ARCHPR`跑一下

![image-20201011103426476](https://i.loli.net/2020/10/11/pbokumdfyZ2R5EK.png)

打开发现是base64

![image-20201011103457971](https://i.loli.net/2020/10/11/biQl259UqfStL6p.png)

解码

![image-20201011103531637](https://i.loli.net/2020/10/11/NjF3PwnTIxDRidA.png)发

发现flag，太常规了

---

## 乌镇峰会种图

种图是一种图片隐写（~~贴吧老哥公然开车的工具之一hhh~~）

`winhex`打开，拉到最后

![image-20201011103958448](https://i.loli.net/2020/10/11/nRMgVwIqlDNGWxC.png)

发现flag，太常规了

---

## LSB

根据题目名称怀疑是LSB隐写

`stegsolver`打开，发现rgb plane 0的头上都有点东西

![image-20201011104532020](https://i.loli.net/2020/10/11/TZ2QS6RLVnfB4Hx.png)

点data extract ，勾选RGB0

![image-20201011104707933](https://i.loli.net/2020/10/11/3qYBXbjsELeMrA7.png)

看这个文件头，是png文件

保存bin为png，得到一个二维码，扫描

![image-20201011104812762](https://i.loli.net/2020/10/11/JqEFWQj5HghAbOz.png)

发现flag，太常规了

---

## rar

提示4位纯数字，`ARCHPR`跑一下

![image-20201011105126259](https://i.loli.net/2020/10/11/oU82Nzd4sPSBEmk.png)

打开rar里的文档发现flag，太常规了

---

## 文件中的秘密

右击属性，发现flag，太常规了

![image-20201011105616982](https://i.loli.net/2020/10/11/L7dD1FYafynKxU9.png)

---

## qr

扫描就行，比赛的签到题

![image-20201011105818626](https://i.loli.net/2020/10/11/H2BC6JipY5yzjNS.png)

---

## wireshark

提示flag是登录网站的密码，所以筛选http协议，追踪http流

![image-20201011110459705](https://i.loli.net/2020/10/11/tH35r1hlN69OETm.png)

发现flag，太常规了

---

## 伪加密

提示是伪加密

`winhex`打开

![image-20201011110808742](https://i.loli.net/2020/10/11/nhiMjygreLaZbdY.png)

修改这位为0就行

打开压缩包里的文档发现flag，太常规了

---

## ningen

先分离出压缩包

![image-20201011111254764](https://i.loli.net/2020/10/11/Pu1Q4d6imCGR7gj.png)

提示是4位密码，`ARCHPR`跑一下

![image-20201011111340411](https://i.loli.net/2020/10/11/hsnkB95itN2JUv1.png)

打开压缩包里的文档发现flag，太常规了

---

## 镜子里面的世界

`stegsolver`打开，发现rgb plane 0的头上都有点东西

点data extract ，勾选RGB0

![image-20201011111936165](https://i.loli.net/2020/10/11/1a4VPIzXThJeq7o.png)

发现flag

---

## 被嗅探的流量

用`wireshark`打开，追踪TCP流

发现flag.png

![image-20201011115059926](https://i.loli.net/2020/10/11/NUd6GvmRISOKaAb.png)

拉到png ascII数据的最后面

![image-20201011115205583](C:\Users\cf260\AppData\Roaming\Typora\typora-user-images\image-20201011115205583.png)

发现flag

---

## 小明的保险箱

根据提示先分离压缩包

![image-20201011115331906](https://i.loli.net/2020/10/11/nsEGC1QmBd4S7uf.png)

`ARCHPR`爆破一下

![image-20201011115449644](C:\Users\cf260\AppData\Roaming\Typora\typora-user-images\image-20201011115449644.png)

打开压缩包中的问道，发现flag，太常规了

---

## 爱因斯坦

先`binwalk`,分离压缩包

![image-20201011115923607](https://i.loli.net/2020/10/11/48TzU9oDtpXrHhS.png)

发现要密码，回去找找

发现jpg属性里有备注

![image-20201011120344259](https://i.loli.net/2020/10/11/IdUu6QR8zgKnY5f.png)

用这个解压压缩包后打开文档发现flag

---

## easycap

追踪tcp流

![image-20201011120551676](https://i.loli.net/2020/10/11/hUsR7GpZagiHKSx.png)

发现flag，太常规了

---

## FLAG

`stegsolver`打开，发现rgb plane 0的头上都有点东西

点data extract ，勾选RGB0

![image-20201011121054323](https://i.loli.net/2020/10/11/wLZrQjHTh7sq1UM.png)

pk头是压缩包，保存bin为zip

解压出来1

直接输出可打印字符串

![image-20201011121542923](https://i.loli.net/2020/10/11/hIAE9GzfZvUsNW7.png)

发现flag

---

## 另外一个世界

`winhex`打开

![image-20201011122009197](https://i.loli.net/2020/10/11/uDkP1wHBzhUTI8e.png)

发现一串二进制，转为文本



![image-20201011122100876](https://i.loli.net/2020/10/11/iPkfmG8MXWtDLnJ.png)

包上flag{}就是flag

---

## 假如给我三天光明

一张图片和一个加密的压缩包，应该就是从图片获得密码，解密压缩包发现flag

打开图片，发现下面一串盲文

![image-20201011122420460](https://i.loli.net/2020/10/11/NEA4XpoO9vZUedS.png)

推一下就知道密码了

![image-20201011122458111](https://i.loli.net/2020/10/11/9h6M8HXlcNZApxz.png)

> kmdonowg

解压压缩包后发现是一个wav音频文件

`audacity`打开看波形

![image-20201011122805518](https://i.loli.net/2020/10/11/xNJLt9ypVsjqOrP.png)

应该是摩斯电码

> -.-. - ..-. .-- .--. . .. ----- ---.. --... ...-- ..--- ..--.. ..--- ...-- -.. --..

![image-20201011123029332](https://i.loli.net/2020/10/11/KG5kPNRv3aW1rpd.png)

加上flag提交不对，卡了半天，最后去看了别人的wp，结果发现题目是从别的地方搞来的，格式完全不一样，要去掉CTF，改为flag{}

> flag{wpei08732?23dz}

---

## 隐藏的钥匙

`winhex`打开

搜索flag

![image-20201011123629664](https://i.loli.net/2020/10/11/mpCuhRTicAZBNYs.png)

base64解密

![image-20201011123710769](https://i.loli.net/2020/10/11/CYDMka4NcWTtZBL.png)

包上flag{}就是flag，太常规了

---

## [BJDCTF 2nd]最简单的misc-y1ng

`winhex`打开压缩包，发现是伪加密

![image-20201011124453246](https://i.loli.net/2020/10/11/kjZt3pQWdT9y1Vx.png)

改为00就可以了

解压出secret，`winhex`打开

由AE 42 60 82知道是png文件尾，但是缺少了jpg文件头

![image-20201011124758534](https://i.loli.net/2020/10/11/Wd3nkjJElh4QRYV.png)

把文件头加到头上

![image-20201011124949543](https://i.loli.net/2020/10/11/rDxZBcwqMWH51Ko.png)

之后修改文件后缀png就可以打开了

![image-20201011125056872](https://i.loli.net/2020/10/11/dTlS4vU265Jh7wY.png)

> 424A447B79316E677A756973687561697D

十六进制转成文本

![image-20201011125129606](https://i.loli.net/2020/10/11/gwQEezVRP6DSO21.png)

---

## [BJDCTF 2nd]A_Beautiful_Picture

下载是一张png

![image-20201011163404832](https://i.loli.net/2020/10/11/F15aGPIOyecVKgo.png)

尝试改一下高度

![image-20201011163558038](https://i.loli.net/2020/10/11/g1FmZ5HfuVPhOqW.png)

![image-20201011163630877](https://i.loli.net/2020/10/11/i3c1vwPkZazgYtA.png)

发现flag，太常规了

---

## 神秘龙卷风

根据提示先用`ARCHPR`爆破一下密码

![image-20201011163854283](https://i.loli.net/2020/10/11/LDeZrzsmbiO7Xdk.png)

打开发现是**brainfuck**

![image-20201011164116552](https://i.loli.net/2020/10/11/uFMoXU5JI97sPD6.png)

在线解码一下

![image-20201011164041577](C:\Users\cf260\AppData\Roaming\Typora\typora-user-images\image-20201011164041577.png)

得到flag

---

