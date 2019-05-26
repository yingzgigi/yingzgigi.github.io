---
layout: post
title: "Windows的Linux子系统：bash打开application的图形界面"
date: 2019-05-16
---

因为想要用linux环境下的LaTex，所以需要texmaker的图形界面。在没有配置前的ubuntu子系统只有terminal。然后整理了一下步骤：

1. 安装VcXsrv: https://sourceforge.net/projects/vcxsrv/files/latest/download 

2. 运行XLaunch
   
    **bash中运行texmaker就可以在VcXsrv中打开界面码字了**


_然后关于不是很需要的Unity桌面：_

3. bash输入：
    ```
    sudo apt-get install ubuntu-desktop unity compizconfig-settings-manager
    ```

    打开ccsm

    ```
    ccsm
    ```

    然后再VcXsrv Server勾选需要的模块，close。

    ```
    echo "export DISPLAY=:0.0" >> ~/.bashrc
    sudo sed -i 's$<listen>.*</listen>$<listen>tcp:host=localhost,port=0</listen>$' /etc/dbus-1/session.conf
    ```

    启动Unity桌面

    ```
    compiz
    ```