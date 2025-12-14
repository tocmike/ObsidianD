---
source: https://u.sb/debian-prefer-ipv4/#%E7%A6%81%E7%94%A8-ipv6
---
[烧饼博客](https://u.sb/)
[首页](https://u.sb/)[标签](https://u.sb/tags/)[归档](https://u.sb/archives/)[友链](https://u.sb/links/)[关于](https://u.sb/about/)[联系](https://u.sb/contact/)[捐赠](https://nm.sl/)[免责声明](https://u.sb/disclaimer/)<https://u.sb/atom.xml><https://cse.google.com/cse?cx=c0d0aa03d9c7e4e61>

# Debian 双栈网络时开启 IPv4 优先

Feb 10th, 2022

本文理论上适合任何 Linux 系统，其他系统未经测试，请自行测试使用。

## <https://u.sb/debian-prefer-ipv4/#%E8%83%8C%E6%99%AF%E4%BB%8B%E7%BB%8D>背景介绍

双协议栈技术就是指在一台设备上同时启用 IPv4 协议栈和 IPv6 协议栈，这样就可以同时使用 IPv4 和 IPv6 的网络。

所有现代化的操作系统和浏览器均会以 IPv6 优先，只有 IPv6 无法访问的时候才会尝试访问 IPv4，某些特定的应用和场景下，我们并不想要 IPv6 优先，这时候就需要修改一些配置文件让 IPv4 优先。

## <https://u.sb/debian-prefer-ipv4/#%E4%BF%AE%E6%94%B9-etc-gai-conf>修改 /etc/gai.conf

在 Debian 等 Linux 系统下，有一个 `/etc/gai.conf` 文件，用于系统的 `getaddrinfo` 调用，默认情况下，它会使用 IPv6 优先，如果您安装了 `curl` 并且本地支持 IPv6，那么可以使用 `curl ip.sb` 测试：

    root@debian ~ # curl ip.sb
    2001:db8::2
    

效果等同于 `curl ip.sb -6`

如果你不想使用 IPv6 优先，可以在这个文件中找到：

    #precedence ::ffff:0:0/96  100
    

取消注释，修改为：

    precedence ::ffff:0:0/96  100
    

一句话命令：

    sed -i 's/#precedence ::ffff:0:0/96  100/precedence ::ffff:0:0/96  100/' /etc/gai.conf
    

此时再使用 `curl ip.sb` 测试

    root@debian ~ # curl ip.sb
    192.0.2.2
    

效果等同于 `curl ip.sb -4`

有时候又会需要强制 IPv6 优先（怎么有些系统和用户那么奇怪？），因为目前 IANA 分配的公网 IPv6 还未进行到 `3000:0000::/4`，所以我们只要把这段之前的 IPv6 加到优先级列表即可，加入这两行 `label` 的优先级：

    label 2002::/16    1
    label 2001:0::/32   1
    

## <https://u.sb/debian-prefer-ipv4/#%E7%A6%81%E7%94%A8-ipv6>禁用 IPv6

有一些极端情况下，我们可能需要禁止系统的 IPv6 功能，这时候就需要修改 `/etc/sysctl.conf` 文件，首先找到你的网卡名称，这里以 `eth0` 为例，然后加入如下内容：

    net.ipv6.conf.all.autoconf = 0
    net.ipv6.conf.default.autoconf = 0
    net.ipv6.conf.all.accept_ra = 0
    net.ipv6.conf.default.accept_ra = 0
    net.ipv6.conf.all.disable_ipv6 = 1
    net.ipv6.conf.default.disable_ipv6 = 1
    net.ipv6.conf.lo.disable_ipv6 = 1
    net.ipv6.conf.eth0.disable_ipv6 = 1
    

如果需要其他网卡则更改或添加 `net.ipv6.conf.eth0.disable_ipv6 = 1` 即可。

一句话命令

    cat >> /etc/sysctl.conf << EOF
    net.ipv6.conf.all.autoconf = 0
    net.ipv6.conf.default.autoconf = 0
    net.ipv6.conf.all.accept_ra = 0
    net.ipv6.conf.default.accept_ra = 0
    net.ipv6.conf.all.disable_ipv6 = 1
    net.ipv6.conf.default.disable_ipv6 = 1
    net.ipv6.conf.lo.disable_ipv6 = 1
    net.ipv6.conf.eth0.disable_ipv6 = 1
    EOF
    

注意 `cat` 命令后的 `>>` 即为添加文件内容，如果使用 `>` 则是覆盖文件内容。

然后使用 `sysctl -p` 来重新加载配置文件，此时查看 `ip a` 就可以发现 IPv6 已经被禁止了。

使用前，我们可以看到无论是本地还是公网网卡都有 `inet6`，即都有 IPv6 地址:

![[./_resources/Debian_双栈网络时开启_IPv4_优先_-_烧饼博客.resources/oyKXC1YLbu2Mi6j.png]]

使用后，无论本地还是公网网卡均无 IPv6 地址：

![[./_resources/Debian_双栈网络时开启_IPv4_优先_-_烧饼博客.resources/qi2nwjWeLfQXJH4.png]]

## <https://u.sb/debian-prefer-ipv4/#%E5%85%B6%E4%BB%96%E7%B3%BB%E7%BB%9F%E5%92%8C%E8%BD%AF%E4%BB%B6>其他系统和软件

Windows 下请参考[这篇回答](https://superuser.com/questions/436574/ipv4-vs-ipv6-priority-in-windows-7)

Firefox 下打开 `about:config` 然后把 `network.dns.disableIPv6` 改成 `true` 即可禁止 Firefox 请求 IPv6

Debian 双栈网络时开启 IPv4 优先
<https://u.sb/debian-prefer-ipv4/>

无责任编辑
Showfom

发布时间
Feb 10th, 2022

版权协议
[WTFPL](https://u.sb/license.txt)

[#Debian](https://u.sb/tags/Debian/)

人過留名，雁過留聲，翺翔的飛鳥總會尋到屬於自己的領地
記得，要讓簡樸而純粹的心靜靜地感受，你要相信，美好的世界總會自己尋路向你走來

Copyright © 2023 烧饼博客
·[Mastodon](https://acg.mn/@Showfom)·[冀 ICP 备 16001626 号 - 4](https://beian.miit.gov.cn/)
Powered by [Next.js](https://nextjs.org/)·Designed by [Baoshuo](https://baoshuo.ren/?utm_source=u.sb&utm_medium=footer)
