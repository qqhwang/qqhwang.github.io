---
layout: post
title: 'BBR+BBR魔改+Lotsever(锐速)一键脚本 for Centos/Debian/Ubuntu'
tag: site
---
这个脚本包含了多种加速工具，能加速ss，十分强大。实测能让vps的速度提升几个数量级。

## 安装

```
wget -N --no-check-certificate "https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh"
chmod +x tcp.sh
./tcp.sh
```

之后会出现一些选项：

TCP加速 一键安装管理脚本

 就是爱生活 | 94ish.me   

升级脚本

————————————内核管理————————————

安装 BBR/BBR魔改版内核

安装 Lotserver(锐速)内核

————————————加速管理————————————

使用BBR加速

使用BBR魔改版加速

使用暴力BBR魔改版加速(不支持部分系统)

使用Lotserver(锐速)加速

————————————杂项管理————————————

卸载全部加速

系统配置优化

退出脚本

————————————————————————————————

根据自己需求操作，重启后再使用./tcp.sh命令接着操作。

## 兼容性
支持系统：
Centos 6+ / Debian 7+ / Ubuntu 14+
BBR魔改版不支持Debian 8

## 参考资料

- https://www.94ish.me/1635.html
- https://www.moerats.com/archives/387/
