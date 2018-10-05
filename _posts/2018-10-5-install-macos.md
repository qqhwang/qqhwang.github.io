---
layout: post
title: E3-1230V5+GTX970实战安装macOS Sierra
tag: macOS
---

随着破解技术的进步，现在安装黑苹果已经非常简单了，映像、驱动、配置文件什么的都有现成的。下面详细讲述一下安装的细节吧。

# 准备工作

强烈建议先找个U盘装pe系统，后面选择config.plist文件时会用到，而且以后装系统时也很方便。（毕竟你总不能在macOS下双击setup.exe来装Windows吧）现在就连装个pe系统都是现成的了，完全傻瓜式操作。下面是一些常用的国产pe：

* [大白菜u盘启动盘制作工具](http://dabaicai.aichyi.cn)
* [老毛桃u盘启动盘制作工具](http://www.laomaotao.org)

如果要折腾的话，还可以选择Microsoft提供的pe制作工具：[WinPE：创建 USB 可引导驱动器](https://msdn.microsoft.com/zh-cn/library/windows/hardware/dn938386(v=vs.85).aspx)。

pe系统准备好了之后，就可以开始下载映像了。这篇文章中我是用的“[macOS Sierra 10.12.6 16G29 with Clover 4123.dmg](http://pan.baidu.com/s/1eRE7QZg)”这个映像，有点老了，但也不建议安装太新的版本，会遇到难以预料到的错误，并且网上还没有经验可循。远景论坛是最好找映像的，无奈注册需要邀请码。所以大家也可以去一个百度云搜索引擎[榆木搜](https://www.yumuso.com)搜索需要的映像。

之后需要用到“[TransMac](https://www.acutesystems.com/scrtm.htm)”这个软件来将dmg文件写入u盘（不要用装了pe的那个u盘！）这是个付费软件，不过我们只需要试用一次就可以了。打开这个软件后，在一个空u盘的盘符上右键，然后选择“restore with disk image”，然后再选择下载好的系统映像，再等个一会儿就可以了。至此，安装的原材料都准备好了。

# 选择正确的config.plist文件

将装好pe的u盘插入电脑，然后重启进入pe系统。然后插上烧录了安装映像的u盘，打开里面的EFI引导分区，进入clover文件夹，就能看到很多config.plist文件。将默认的文件删除，然后选择一个适合你电脑的文件重命名为config.plist，然后关机。

如果没有pe系统的话，在windows进行这项操作会非常麻烦，而选择正确的config.plist文件又是非常必要的。所以，最好还是要有一个pe引导u盘。

# 像从前那样安装系统

接着从装有macOS系统映像的u盘启动，会看到clover的引导界面。选择“Boot OS X Install from macOS Sierra 10.12.6”,然后就跟装Windows差不多了。你只需要点一点下一步、同意使用条款，安装便会顺利进行。

差点忘记说了，macOS是不支持在ntfs文件系统下安装的。所以你得先选择磁盘工具，将一个磁盘“抹掉”，方案要改成GUID分区图，这样安装时才能选择安装磁盘。

# 解决驱动问题

macOS不会自动下载驱动程序（这点就不如Windows了），所以对于GTX970，我们还得单独下载驱动程序。Nvidia显卡的驱动程序可以在这里找到：[Tonymacx86](https://www.tonymacx86.com/nvidia-drivers/)。下载时，要注意与自己的系统版本相对应。在左上角的苹果图标里有一个关于本机的选项，再点一下版本号（例如：版本10.12.6）就会出现更具体的系统版本（例如：16G29）。下载时，可以在页面上按下Command+F来搜索适合你系统的驱动。安装好驱动后，在Nvidia Driver Manager中选择Nvidia web driver。显卡解决了，系统就变得非常流畅了。

![sysinfo.jpeg](https://i.loli.net/2018/10/05/5bb78470a5800.jpeg)

不过，我的无线网卡没有装上驱动，所以一直都插着网线。如果要折腾的话，网上也应该找得到。

# 系统细节处理

最重要的是先关闭App Store自动更新，系统一更新，你的macOS能不能启动就很难说了。

在系统偏好设置的“鼠标”中，可以取消勾选“滚动方向：自然”。反正我是更习惯Windows的滚动方向。

到这里，系统已经完全可以正常使用了。不过别忘了，你的系统引导还在u盘上，所以最后，我们要把它转移到电脑的硬盘上。

# 设置EFI引导

首先下载[EFI Tools Clover](https://pan.baidu.com/s/1bpewwPL)，然后打开，会跳出一个终端窗口。

![efi_tool.jpeg](https://i.loli.net/2018/10/05/5bb78470a80ae.jpeg)

输入C，回车。找到你安装macOS的硬盘，例如是disk0的话就输入0.

![disk0.jpeg](https://i.loli.net/2018/10/05/5bb78470a70e8.jpeg)

然后找到TPYE为EFI的那一行（大小为209.7MB），前面的序号是几就输入几。

![disk0s1.jpeg](https://i.loli.net/2018/10/05/5bb78470a9e0b.jpeg)

之后输入y。注意不要按错键，否则会显示操作终止。

这里完成后我们再进一次pe，将u盘里的efi文件全都复制到硬盘上的efi分区上。这样，以后就可以不用再插着u盘开机了。

# 体验macOS

接下来，尽情体验macOS吧！（虽然我已经滚回Windows了）

![exp.jpeg](https://i.loli.net/2018/10/05/5bb78470ea225.jpeg)

最后更新于2018-10-5

