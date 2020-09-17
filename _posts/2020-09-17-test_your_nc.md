---
title: test_your_nc
author: Stan
date: 2020-09-17 09:10:33 +0800
categories: [BUUCTF,Pwn]
tags: [Pwn]
---
```
nc node3.buuoj.cn 25795
```

![2020-09-17_13-42-26](../assets/img/posts/2020-09-17_13-42-26.png)

直接nc过去，ls即可看到文件，发现flag，cat即可发现flag

```
cat flag
```

