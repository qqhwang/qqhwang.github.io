---
layout: post
title: 实战！Surface RT安装Windows RT 10
tag: 2020
---

2020年了，于2012年6月发布的~~垃圾~~Surface RT还能装上于2015年发布的Windows 10？果然是亲儿子。

# 前期的准备

* 一个容量大于4GB的U盘
* 一个USB外接键盘（并不能用Touch Cover），主要是为了方便操作，似乎不用也可以。
* Surface RT或Surface 2（真是一对难兄难弟）

相关的文件可以在这里下载：[百度云链接](https://pan.baidu.com/s/12Uf6BQ_VFTCRKv_1GZgaYQ) 提取码：c7bf
最主要的是下载“软件工具”和Win10镜像、Win8镜像。

# Bitlocker的关闭

请先备份好文件。本教程将会带你暴力关闭Bitlocker。（没有想到如此低配的Surface还支持BitLocker）

## 创建Surface 恢复U盘

警告：请使用空U盘，因为此过程将擦除该驱动器上已存储的所有数据。在任意一台Windows 10中创建恢复驱动器，请执行以下操作：

1. 在“开始”按钮旁边的搜索框中，搜索“创建恢复驱动器”，然后选择搜索结果。系统可能会要求你输入管理员密码或确认你的选择。
2. 此工具打开后，确保不要选中“将系统文件备份到恢复驱动器”，然后选择“下一步”。
3. 将U盘连接到你的电脑，选中，然后选择“下一步”。
4. 选择“创建”。需要将大量文件复制到恢复驱动器，因此可能需要一些时间来完成。

解压恢复包(Windows RT 8.1)至U盘根目录，如提示重复，替换即可。

## 通过格盘并重装系统来关闭BitLocker

1. 进入安全模式 在「设置>更新和恢复>恢复」中找到高级启动选项，点击立即重新启动。
  **进入安全模式还有替代方法：**
* 打开计算机，然后在 Windows 启动之前按 F8 键。
* 长按调低音量按钮，同时按下并释放电源按钮。当显示Microsoft或Surface徽标时，释放调低音量按钮。
2. 选择“疑难解答”，然后选择“高级选项”>“命令提示符”。如果提示输入恢复密钥，选择屏幕底部的“跳过这个驱动器”。
3. 执行C盘格式化，请务必确保命令输入正确，执行无错误后继续操作。
```
diskpart
lis dis
sel dis 0
lis par
sel par 4
for quick fs=ntfs override
exit
```
4. 部署win 8系统（重新部署回win8，执行其他操作）输入：
```
dism /apply-image /imagefile:d:\sources\install.wim /applydir:c: /index:1
```
执行完毕后关闭对话框自动重启进入系统,其中d:\sources\install.wim是你放的恢复镜像的位置，c:为部署到系统盘。进入系统后，C盘的Bitlocker带锁的标志消失，则为破解成功。

# SecureBoot的破解

1. 关闭UAC。请右键以管理员身份运行软件工具目录下的—禁用UAC.reg，重启计算机
2. 关闭注册表挂载。
请使用win+R键打开“运行”，输入regedit打开注册表编辑器，选择HKEY_LOCAL_MACHINE\BCD00000000，点击“文件”>“卸载配置单元”
3. 运行SecureBootPatch程序
请打开软件工具中的secruebootpatch文件夹，右键管理员运行InstallPolicy.cmd，重启电脑。如果重启后，出现secure boot debug policy界面，则表示正常，请选择accept and install进行安装。（使用外接USB键盘按⬇进行选择，回车确定。此时Touch Cover不起作用。也可以使用音量键选择，Windows徽标键确定）
4. 开启测试模式
运行命令提示符，输入：
```
bcdedit /set {default} testsigning on
bcdedit /set {bootmgr} testsigning on
```
如果未出现报错，则重启电脑后，进入recovery的命令提示符模式，检查右下角提示"SecureBoot isn't configured correctly"即成功。

# Windows 10的部署

1. 进入安全模式（即recovery）
2. 执行C盘格式化。请务必确保命令输入正确，执行无错误后继续操作。（参见上文）
3. 部署win 10系统。修改对应win10包的名称为install.wim，无需解压，拷贝至U盘根目录。
输入以下命令
```
dism /apply-image /imagefile:d:\install.wim /applydir:c: /index:1
```
然而，我安装的时候U盘并未分配到d盘符。所以当提示错误3（找不到相应路径下的文件）时，可以尝试多试几个盘符：
```
dism /apply-image /imagefile:e:\install.wim /applydir:c: /index:1
```
执行完毕后关闭对话框重启进入系统。
4. 重启系统后，等待一段时间的安装后，会弹出报错对话框
修改注册表：用USB外接键盘，按SHIFT+F10启动“运行”，输入regedit，打开注册表下面路径：
HKEY_Local machine/SYSTEM/SETUP/STATUS/ChildCompletion修改“setup.exe”将1改成3，后点击OK，重启系统开始正常配置系统。
5. 系统配置
在进入系统配置后，语言请直接选择中文，提示无法使用Micosoft账户登录是正常的，进入系统后使用商店登录即可。
6. 系统激活
打开PowerShell，运行:
```
slmgr.vbs /upk
slmgr /ipk NPPR9-FWDCX-D2C8J-H872K-2YT43
slmgr /skms kms.03k.org
slmgr /ato
slmgr /skms zhang.y.t
```

# 安装好了Win10也~~并没有什么卵用~~

按照Windows RT的优良传统，此版本Windows仍然无法运行桌面exe应用程序。所以它并不是之前某些高通骁龙845设备所搭载的Windows 10 ARM系统。并且此版本Windows是测试版，还是1703（创意者更新）。可见Microsoft还是很闲的，竟然给已经毫无存在感的设备开发系统更新。

8年过去了，Surface产品线已经从当年的单一走向如今的丰富多彩。Surface RT，作为微软的第一次向平板电脑领域的涉足，虽然是失败的产品，但却有它独特的意义。

参考链接
[【IT之家学院】Surface1和2代部署安装Windows 10 ARM版教程](https://www.ithome.com/0/469/731.htm)

（最后更新于2020-1-31）
