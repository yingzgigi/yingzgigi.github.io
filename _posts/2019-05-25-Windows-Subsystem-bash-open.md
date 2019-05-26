---
layout: post
title: "Windows的Linux子系统：bash打开application的图形界面"
date: 2019-05-16
---

因为想要用linux环境下的LaTex，所以需要texmaker的图形界面。在没有配置前的ubuntu子系统只有terminal。
1. 安装VcXsrv: https://sourceforge.net/projects/vcxsrv/files/latest/download 
2. 运行XLaunch
3. bash输入：
```
   sudo apt-get install ubuntu-desktop unity compizconfig-settings-manager
```

打开ccsm

```
ccsm
```

然后再VcXsrv Server勾选需要的模块，close。
bash中运行texmaker就可以在VcXsrv中打开界面码字。

Unity桌面就算了。