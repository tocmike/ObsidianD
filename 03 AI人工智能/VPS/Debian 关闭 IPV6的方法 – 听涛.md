---
source: https://www.tingtao.org/archives/781.html
---
![[./_resources/Debian_关闭_IPV6的方法_–_听涛.resources/svg_1.svg]]![[./_resources/Debian_关闭_IPV6的方法_–_听涛.resources/svg_2.svg]]![[./_resources/Debian_关闭_IPV6的方法_–_听涛.resources/svg_3.svg]]![[./_resources/Debian_关闭_IPV6的方法_–_听涛.resources/svg_4.svg]]![[./_resources/Debian_关闭_IPV6的方法_–_听涛.resources/svg_5.svg]]![[./_resources/Debian_关闭_IPV6的方法_–_听涛.resources/svg_6.svg]]![[./_resources/Debian_关闭_IPV6的方法_–_听涛.resources/svg_7.svg]]![[./_resources/Debian_关闭_IPV6的方法_–_听涛.resources/svg_8.svg]]
[跳至内容](https://www.tingtao.org/archives/781.html#content)

		

* 2023-04-08

		

[![[./_resources/Debian_关闭_IPV6的方法_–_听涛.resources/cropped-cropped-tingtao-1.png]]](https://www.tingtao.org/)

		

[__](https://www.tingtao.org/archives/781.html#)

* [](https://www.tingtao.org/)

* [首页](https://www.tingtao.org/)
* [技术及经验__](https://www.tingtao.org/archives/category/tech)
* [测试及优惠__](https://www.tingtao.org/archives/category/test-coupon)
* [未分类](https://www.tingtao.org/archives/category/uncategorized)
* [本站服务__](https://www.tingtao.org/archives/781.html#)

		

[Unix/Linux](https://www.tingtao.org/archives/category/tech/linux) [VPS/服务器](https://www.tingtao.org/archives/category/tech/vps_server)

# 

<https://www.tingtao.org/archives/author/tingtao>

#### 作者[听涛](https://www.tingtao.org/archives/author/tingtao)

__ 6月 10, 2017

官方给了两种方法。

第一种：

在/etc/default/grub文件的GRUB\_CMDLINE\_LINUX变量中添加IPV6\_DISABLE=1，然后运行update-grub，最后重启服务器。

第二种：

编辑/etc/sysctl.conf，添加或者编辑以下变量：

net.ipv6.conf.all.disable\_ipv6 = 1
net.ipv6.conf.default.disable\_ipv6 = 1
net.ipv6.conf.lo.disable\_ipv6 = 1
net.ipv6.conf.eth0.disable\_ipv6 = 1

最后sysctl -p即可。

附上官方原文：

How to turn off IPv6

Append IPV6\_DISABLE=1 to the GRUB\_CMDLINE\_LINUX variable in /etc/default/grub.
Run update-grub and reboot.
or better,

edit /etc/sysctl.conf and add those parameters to kernel. Also be sure to add extra lines for other network interfaces you want to disable IPv6.


net.ipv6.conf.all.disable\_ipv6 = 1
net.ipv6.conf.default.disable\_ipv6 = 1
net.ipv6.conf.lo.disable\_ipv6 = 1
net.ipv6.conf.eth0.disable\_ipv6 = 1
After editing sysctl.conf, you should run sysctl -p to activate changes or reboot system.

原文地址：https://wiki.debian.org/DebianIPv6

[__](https://www.facebook.com/sharer.php?u=https://www.tingtao.org/archives/781.html) [__](http://twitter.com/share?url=https://www.tingtao.org/archives/781.html&text=Debian%20%E5%85%B3%E9%97%AD%20IPV6%E7%9A%84%E6%96%B9%E6%B3%95) [__](https://www.tingtao.org/archives/781.htmlmailto:?subject=Debian%20%E5%85%B3%E9%97%AD%20IPV6%E7%9A%84%E6%96%B9%E6%B3%95&body=https://www.tingtao.org/archives/781.html) [__](https://www.linkedin.com/sharing/share-offsite/?url=https://www.tingtao.org/archives/781.html&title=Debian%20%E5%85%B3%E9%97%AD%20IPV6%E7%9A%84%E6%96%B9%E6%B3%95) <https://telegram.me/share/url?url=https://www.tingtao.org/archives/781.html&text&title=Debian%20%E5%85%B3%E9%97%AD%20IPV6%E7%9A%84%E6%96%B9%E6%B3%95>[[#|__]]

## 文章导航

[VPN原理非学术详解及一些网络科普知识
](https://www.tingtao.org/archives/754.html)
[
.htaccess 进行https/http和有无www转向的设置方法](https://www.tingtao.org/archives/793.html)

<https://www.tingtao.org/archives/author/tingtao>

#### 作者 [听涛](https://www.tingtao.org/archives/author/tingtao)

#### 相关文章

[VPS/服务器](https://www.tingtao.org/archives/category/tech/vps_server)

#### [关于web服务的性能提升，一个不太成熟的构想](https://www.tingtao.org/archives/3064.html)

__ 8月 8, 2022 [听涛](https://www.tingtao.org/archives/author/tingtao)

[Unix/Linux](https://www.tingtao.org/archives/category/tech/linux) [VPS/服务器](https://www.tingtao.org/archives/category/tech/vps_server)

#### [Nginx 使用不同的SSL证书](https://www.tingtao.org/archives/3056.html)

__ 7月 16, 2022 [听涛](https://www.tingtao.org/archives/author/tingtao)

[Unix/Linux](https://www.tingtao.org/archives/category/tech/linux)

#### [某些机器需要关闭ncq](https://www.tingtao.org/archives/3027.html)

__ 6月 13, 2022 [听涛](https://www.tingtao.org/archives/author/tingtao)

### 发表回复

您的电子邮箱地址不会被公开。 必填项已用\*标注

评论 \*

显示名称 \*

电子邮箱地址 \*

网站地址

**提示:** 评论内容需要包含中文，否则会被当做spam屏蔽!

###### 赞助本站原创

![[./_resources/Debian_关闭_IPV6的方法_–_听涛.resources/qrcode.png]]

###### 联系我

**QQ/微信 ： 396745**

###### 搜索

###### 分类目录

分类目录

###### 最新文章

* [总有人语重心长的说“不谈政治”](https://www.tingtao.org/archives/3130.html)

* [中国沙特伊朗在北京发表三方联合声明](https://www.tingtao.org/archives/3126.html)
* [腻了](https://www.tingtao.org/archives/3116.html)
* [在哥面前，谁敢称神？](https://www.tingtao.org/archives/3104.html)
* [Google的下作与反华，可能你想不到](https://www.tingtao.org/archives/3095.html)
* [分享一个多SSH/RDP/VNC管理工具：MobaXterm](https://www.tingtao.org/archives/3088.html)
* [将外部程序运行在自己的窗体内](https://www.tingtao.org/archives/3085.html)
* [python+flask日记：以开发者的视角来分解网站结构](https://www.tingtao.org/archives/3073.html)
* [关于web服务的性能提升，一个不太成熟的构想](https://www.tingtao.org/archives/3064.html)
* [Nginx 使用不同的SSL证书](https://www.tingtao.org/archives/3056.html)
* [分享一个有趣的下载软件：文件蜈蚣](https://www.tingtao.org/archives/3049.html)
* [被催更了。。。](https://www.tingtao.org/archives/3041.html)
* [打嘴炮 :)](https://www.tingtao.org/archives/3035.html)
* [占了个小便宜](https://www.tingtao.org/archives/3031.html)
* [某些机器需要关闭ncq](https://www.tingtao.org/archives/3027.html)
* [将MySQL/MariaDB的数据放进内存中](https://www.tingtao.org/archives/3012.html)
* [OVH/SoYouStart的附加IP分配给虚拟机](https://www.tingtao.org/archives/2995.html)
* [当西方媒体公开允许仇俄言论，你想到了什么？](https://www.tingtao.org/archives/2987.html)
* [婊子无情，戏子无义，可遇到了毛子和渣子……](https://www.tingtao.org/archives/2983.html)
* [将Nginx的日志放入MySQL (3)](https://www.tingtao.org/archives/2974.html)

###### 友情链接

		

#### You missed

<https://www.tingtao.org/archives/3130.html>

[未分类](https://www.tingtao.org/archives/category/uncategorized)

#### [总有人语重心长的说“不谈政治”](https://www.tingtao.org/archives/3130.html)

__ [3月 24, 2023](https://www.tingtao.org/archives/date/2023/03)

<https://www.tingtao.org/archives/3126.html>

[未分类](https://www.tingtao.org/archives/category/uncategorized)

#### [中国沙特伊朗在北京发表三方联合声明](https://www.tingtao.org/archives/3126.html)

__ [3月 10, 2023](https://www.tingtao.org/archives/date/2023/03)

<https://www.tingtao.org/archives/3116.html>

[未分类](https://www.tingtao.org/archives/category/uncategorized)

#### [腻了](https://www.tingtao.org/archives/3116.html)

__ [1月 22, 2023](https://www.tingtao.org/archives/date/2023/01)

<https://www.tingtao.org/archives/3104.html>

[未分类](https://www.tingtao.org/archives/category/uncategorized)

#### [在哥面前，谁敢称神？](https://www.tingtao.org/archives/3104.html)

__ [10月 26, 2022](https://www.tingtao.org/archives/date/2022/10)

		

<https://www.tingtao.org/>

		

[听涛](https://www.tingtao.org/) | [股票日记](https://www.tingtao.org/archives/category/stock/)

* [首页](https://www.tingtao.org/)

* [* 股票日记*](https://www.tingtao.org/archives/category/stock)
