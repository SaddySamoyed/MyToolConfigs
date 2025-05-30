中国大陆如何

分享一个回到国内, 更新 wsl2 的时候出现的一点问题，，发现 

```shell
sudo apt update
```

用不了, 很多包不能 retrieve.

查看了一下 pin:

```shell
ping -c 3 archive.ubuntu.com
ping6 -c 3 archive.ubuntu.com
```

发现掉包率是 100%. 开了 VPN 之后也没用. 所以是被墙了.

遂检索发现: 对 ubuntu 系统相关的 retrieve 路径被 specify 在:

```shell
/etc/apt/sources.list
```

这个文件里. 我们只要把这个文件里所有的 `http://archive.ubuntu.com/ubuntu/` 和 `http://security.ubuntu.com/ubuntu/` 替换为国内的镜像源就可以了.

我使用的是 

```
http://mirrors.ustc.edu.cn/ubuntu/
```







直接 `sudo vim` 把这个文件内容改成:

```list
# 使用中科大镜像源（Ubuntu 22.04 jammy）

deb http://mirrors.ustc.edu.cn/ubuntu/ jammy main restricted
# deb-src http://mirrors.ustc.edu.cn/ubuntu/ jammy main restricted

deb http://mirrors.ustc.edu.cn/ubuntu/ jammy-updates main restricted
# deb-src http://mirrors.ustc.edu.cn/ubuntu/ jammy-updates main restricted

deb http://mirrors.ustc.edu.cn/ubuntu/ jammy universe
# deb-src http://mirrors.ustc.edu.cn/ubuntu/ jammy universe

deb http://mirrors.ustc.edu.cn/ubuntu/ jammy-updates universe
# deb-src http://mirrors.ustc.edu.cn/ubuntu/ jammy-updates universe

deb http://mirrors.ustc.edu.cn/ubuntu/ jammy multiverse
# deb-src http://mirrors.ustc.edu.cn/ubuntu/ jammy multiverse

deb http://mirrors.ustc.edu.cn/ubuntu/ jammy-updates multiverse
# deb-src http://mirrors.ustc.edu.cn/ubuntu/ jammy-updates multiverse

deb http://mirrors.ustc.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
# deb-src http://mirrors.ustc.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse

deb http://mirrors.ustc.edu.cn/ubuntu/ jammy-security main restricted
# deb-src http://mirrors.ustc.edu.cn/ubuntu/ jammy-security main restricted

deb http://mirrors.ustc.edu.cn/ubuntu/ jammy-security universe
# deb-src http://mirrors.ustc.edu.cn/ubuntu/ jammy-security universe

deb http://mirrors.ustc.edu.cn/ubuntu/ jammy-security multiverse
# deb-src http://mirrors.ustc.edu.cn/ubuntu/ jammy-security multiverse
# 使用中科大镜像源（Ubuntu 22.04 jammy）

deb http://mirrors.ustc.edu.cn/ubuntu/ jammy main restricted
# deb-src http://mirrors.ustc.edu.cn/ubuntu/ jammy main restricted

deb http://mirrors.ustc.edu.cn/ubuntu/ jammy-updates main restricted
# deb-src http://mirrors.ustc.edu.cn/ubuntu/ jammy-updates main restricted

deb http://mirrors.ustc.edu.cn/ubuntu/ jammy universe
# deb-src http://mirrors.ustc.edu.cn/ubuntu/ jammy universe

deb http://mirrors.ustc.edu.cn/ubuntu/ jammy-updates universe
# deb-src http://mirrors.ustc.edu.cn/ubuntu/ jammy-updates universe

deb http://mirrors.ustc.edu.cn/ubuntu/ jammy multiverse
# deb-src http://mirrors.ustc.edu.cn/ubuntu/ jammy multiverse

deb http://mirrors.ustc.edu.cn/ubuntu/ jammy-updates multiverse
# deb-src http://mirrors.ustc.edu.cn/ubuntu/ jammy-updates multiverse

deb http://mirrors.ustc.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
# deb-src http://mirrors.ustc.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse

deb http://mirrors.ustc.edu.cn/ubuntu/ jammy-security main restricted
# deb-src http://mirrors.ustc.edu.cn/ubuntu/ jammy-security main restricted

deb http://mirrors.ustc.edu.cn/ubuntu/ jammy-security universe
# deb-src http://mirrors.ustc.edu.cn/ubuntu/ jammy-security universe

deb http://mirrors.ustc.edu.cn/ubuntu/ jammy-security multiverse
# deb-src http://mirrors.ustc.edu.cn/ubuntu/ jammy-security multiverse

```

然后就可以正常使用 `sudo apt`.