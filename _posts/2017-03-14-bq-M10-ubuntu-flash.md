---
layout: post
category: "ubuntu"
title: "Ubuntu版BQ Aquaris M10平板刷机方法"
tags: ["ubuntu, Aquaris, M10, tablet, flash"]
summary: "我只是一条摘要"

---

去年Ubuntu平板刚上市的时候，从[BQ官网](https://www.bq.com/en/)海淘回来一台，没玩几天就让我把系统给捣鼓坏了，一直扔到现在。昨天想起来了，就刷刷系统吧。

首先，要确保有一台装了Ubuntu桌面系统的电脑，虚拟机也行。在这个系统里要安装刷机工具。

```bash
$ sudo add-apt-repository ppa:ubuntu-sdk-team/ppa
$ sudo apt-get update
$ sudo apt-get install ubuntu-device-flash
$ sudo apt-get install phablet-tools
```

然后把平板用USB线连接到电脑的Ubuntu系统上，并确保打开平板系统上的开发者模式（系统设置 > 关于 > 开发者模式），允许电脑进行调试。在平板开机的状态下执行以下命令进行刷机操作。

查询并列出设备的所有可用渠道，并选出stable的稳定版。

```bash
$ ubuntu-device-flash query --list-channels --device=frieza | grep stable
```

刷机，要多等一会，特别是不动的时候，不要急着退出！

```bash
$ ubuntu-device-flash --server http://cache-origin.system-image.ubuntu.com touch --developer-mode --channel=ubuntu-touch/stable/bq-aquaris-pd.en --password=0000 --wipe
```

如果上面的刷机命令执行失败了，再执行之前可能需要清空上次的缓存，这时就用下面这条命令。

```bash
$ ubuntu-device-flash —clean-cache --server http://cache-origin.system-image.ubuntu.com touch --developer-mode --channel=ubuntu-touch/stable/bq-aquaris-pd.en --password=0000 --wipe
```

重新启动以后，就是一个崭新的系统啦～

p.s. 新系统默认是没有写入权限的，可以执行以下命令启用读写模式，这样就可以用`sudo apt-get update`等命令来更新系统了：

```bash
$ phablet-config writable-image
```

#### 参考资料
1. [CSDN: Ubuntu手机系统目前支持的装置及刷Ubuntu OS到你的装置中](http://blog.csdn.net/ubuntutouch/article/details/38403179)
2. [Installing Ubuntu for devices](https://developer.ubuntu.com/en/phone/devices/installing-ubuntu-for-devices/)
3. [Ubuntu image channels](https://developer.ubuntu.com/en/phone/devices/image-channels/)
