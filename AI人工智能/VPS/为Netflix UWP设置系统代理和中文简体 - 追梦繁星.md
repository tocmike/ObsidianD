---
source: https://www.leeyiding.com/archives/53/
---
# 为 Netflix UWP 设置系统代理和中文简体

* [LeeYD](https://www.leeyiding.com/author/1/) •

* 2020 年 07 月 29 日
* ・阅读: 14371
* • [教程](https://www.leeyiding.com/category/jc/)

* * *

[【阿里云】爆款云产品，新客特惠全年最低价，云服务器低至 0.4 折起，11.1 开售](https://www.aliyun.com/activity/1111?userCode=hqn1ki6i)
[【腾讯云】爆款 1 核 2G 云服务器首年 48 元，还有 iPad Pro、Bose 耳机、京东卡等你来抽！](https://cloud.tencent.com/act/cps/redirect?redirect=35372&cps_key=870349d86c1abd80b3b835a06d057ffb)
[【华为云】上云特惠巨划算，免单抽奖享豪礼](https://activity.huaweicloud.com/828_promotion/index.html?fromacct=26a469bc-df4c-4292-94fc-0c840643d9be&utm_source=V1g3MDY4NTY=&utm_medium=cps&utm_campaign=201905)
[【七牛云】爆款云产品全年最低价，热门产品 0 元秒杀，参与抽奖赢新款 iPhone](https://s.qiniu.com/RNRfYz)

* * *

## 1\. UWP 添加系统代理

因为 Netflix 在 Chrome 上播放视频最高只支持 720P，所以很多人选择了支持 1080P + 的 Microsoft Edge 浏览器和微软应用商店的 UWP 版本的 Netflix，但是很多人打开 UWP 版本的 Netflix 会出现如下提示：**抱歉，与 Netflix 通信时出现问题。请重试**

![[./_resources/为Netflix_UWP设置系统代理和中文简体_-_追梦繁星.resources/200728-1.png]]

即使你开启了代理程序，依旧是上图提示，原因是 Win10 UWP 运行在沙盒里， UWP 应用出于安全考虑，内部网络和系统网络是相互隔离的。

这里有两个方法让 Netflix UWP 通过代理，第一种方法是在路由器设置透明代理，路由器下所有设备流量都会经过路由器网关处理。第二种方法是使用系统内置的 CheckNetIsolation 工具解除 UWP 与系统之间的网络隔离。路由器配置代理一劳永逸，但是对于部分人有一定门槛，所以本文着重介绍后者，此方法理论适用于所有 UWP 应用。

首先 Win+R，输入 regedit 打开注册表管理工具，定位到  `HKEY_CURRENT_USERSOFTWAREClassesLocal SettingsSoftwareMicrosoftWindowsCurrentVersionAppContainerMappings` 

![[./_resources/为Netflix_UWP设置系统代理和中文简体_-_追梦繁星.resources/200728-2.png]]

其中左侧是应用的 SID 值，右侧是 DisplayName 的值为应用名称，可以找到 Netflix 的 SID 值为  `S-1-15-2-444797119-353723001-3522112724-563070080-1809981734-922308773-1844997097` ，每个人的 SID 值应该都是这个

接下来看一下 CheckNetIsolation 应用使用方法

复制

    1用法:
    2   CheckNetIsolation LoopbackExempt [operation] [-n=] [-p=]
    3      操作列表:
    4          -a  -  向环回免除列表中添加 AppContainer 或程序包系列。
    5          -d  -  从环回免除列表中删除 AppContainer 或程序包系列。
    6          -c  -  清除环回免除的 AppContainer 和程序包系列的列表。
    7          -s  -  显示环回免除的 AppContainer 和程序包系列的列表。
    8
    9      参数列表:
    10          -n= - AppContainer 名称或程序包系列名称。
    11          -p= - AppContainer 或程序包系列安全标识符(SID)。
    12          -?  - 显示 LoopbackExempt 模块的此帮助消息。
    

所以在 CMD 执行下条命令解除隔离，执行成功后，终端返回 `完成` 

复制

    1CheckNetIsolation.exe loopbackexempt -a -p=S-1-15-2-444797119-353723001-3522112724-563070080-1809981734-922308773-1844997097
    

再次重启 Netflix UWP 客户端并启动系统代理，可以发现已经成功进入

![[./_resources/为Netflix_UWP设置系统代理和中文简体_-_追梦繁星.resources/200728-3.png]]

如果想要恢复隔离，只需将上条命令中的  `-a`  参数改为  `-d`  即可

## 2\. 设置中文简体

在网页中有两处可以设置语言，设置后可以应用到所有平台，其中语言选项里有中文选项，但是设置后是中文繁体

![[./_resources/为Netflix_UWP设置系统代理和中文简体_-_追梦繁星.resources/200728-4.png]]

![[./_resources/为Netflix_UWP设置系统代理和中文简体_-_追梦繁星.resources/200728-5.png]]

其实 Netflix 是可以设置为中文简体的，这里有两个方法。

方法一：使用新加坡原生 IP 节点登陆 Netflix 并在上图中将语言设置为中文，但是这对大部分人来说也是一条门槛。

方法二：通过修改 POST 请求将语言设置为中文简体

进入如下页面，F12 打开浏览器控制台，切换到 Network 选项，并将类型切换为 XHR

![[./_resources/为Netflix_UWP设置系统代理和中文简体_-_追梦繁星.resources/200728-6.png]]

将个人资料语言设置为 English 并保存，这时控制台会刷新出一条 pathEvaluator 开头的资源，点开详情，在 Form Data 中可以看到一条值为 "en-US" 的记录

![[./_resources/为Netflix_UWP设置系统代理和中文简体_-_追梦繁星.resources/200728-7.png]]

在该条记录上右键 —>Copy—>Copy as cURL (bash)

![[./_resources/为Netflix_UWP设置系统代理和中文简体_-_追梦繁星.resources/200728-8.png]]

将复制的内容粘贴到文本编辑器中，找到位于倒数第二行的  `en-US`  将其改为  `zh-SG`  后，再将所有内容复制

![[./_resources/为Netflix_UWP设置系统代理和中文简体_-_追梦繁星.resources/200728-9.png]]

打开[在线运行 CURL 命令网站](https://www.leeyiding.com/go/aHR0cHM6Ly9yZXFiaW4uY29tL2N1cmw=)，将修改后的内容粘贴到文本框内，再点击  `Send` ，执行完毕后，右侧出现  `Status: 200(OK)`  即代表成功

返回 Netflix 首页，使用 Shift+F5 强制刷新几遍，可以发现页面的语言已变为简体中文，同时 UWP 平台的也同步变更为中文简体了，安卓客户端需要清空应用数据重新登录。

![[./_resources/为Netflix_UWP设置系统代理和中文简体_-_追梦繁星.resources/200728-10.png]]

* * *

> 版权属于：[LeeYD · Blog](https://www.leeyiding.com/)
> 本文标题：[为 Netflix UWP 设置系统代理和中文简体](https://www.leeyiding.com/archives/53/)
> 本文链接：<https://www.leeyiding.com/archives/53/>
> 版权声明：本博客所有文章除特别声明外，均采用[__ BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh) 许可协议
> **若转载本文，请标明出处并告知本人**

* * *

[【阿里云】爆款云产品，新客特惠全年最低价，云服务器低至 0.4 折起，11.1 开售](https://www.aliyun.com/activity/1111?userCode=hqn1ki6i)
[【腾讯云】爆款 1 核 2G 云服务器首年 48 元，还有 iPad Pro、Bose 耳机、京东卡等你来抽！](https://cloud.tencent.com/act/cps/redirect?redirect=35372&cps_key=870349d86c1abd80b3b835a06d057ffb)
[【华为云】上云特惠巨划算，免单抽奖享豪礼](https://activity.huaweicloud.com/828_promotion/index.html?fromacct=26a469bc-df4c-4292-94fc-0c840643d9be&utm_source=V1g3MDY4NTY=&utm_medium=cps&utm_campaign=201905)
[【七牛云】爆款云产品全年最低价，热门产品 0 元秒杀，参与抽奖赢新款 iPhone](https://s.qiniu.com/RNRfYz)

* * *

[Netflix](https://www.leeyiding.com/tag/Netflix/)[UWP](https://www.leeyiding.com/tag/UWP/)
最后编辑于: 2022 年 08 月 10 日

[返回文章列表](https://www.leeyiding.com/archives.html)  

![&size=200](https://api.leeyiding.com/ewm/?text=https://www.leeyiding.com/archives/53/&size=200)
![[./_resources/为Netflix_UWP设置系统代理和中文简体_-_追梦繁星.resources/wxzs.png]]

[[#|下一篇:
没有更多了]] [上一篇:
阿里云云计算 7 天实践训练营 —— 基于 ECS 和 NAS 搭建个人网盘](https://www.leeyiding.com/archives/52/)

添加新评论

OωO
[[#|打卡]]

已有 8 条评论

1. [Best checknetisolation New - De.foci](https://www.leeyiding.com/go/aHR0cHM6Ly9kZS5mb2NpLmNvbS52bi9iZXN0LWNoZWNrbmV0aXNvbGF0aW9uLW5ldy8=)     Linux /   Google Chrome
	[回复](https://www.leeyiding.com/archives/53/comment-page-1?replyTo=750#respond-post-53)
	[2022 年 07 月 06 日](https://www.leeyiding.com/archives/53/comment-page-1#pingback-750)
	
	\[...\] + 여기서 자세히 보기\[...\]
	
2. ![[./_resources/为Netflix_UWP设置系统代理和中文简体_-_追梦繁星.resources/spinner.svg]] Z1yue     Windows 10 /   Google Chrome
	[回复](https://www.leeyiding.com/archives/53/comment-page-1?replyTo=532#respond-post-53)
	[2021 年 05 月 29 日](https://www.leeyiding.com/archives/53/comment-page-1#comment-532)
	
	现在没看到 F12 里面的 Param 写 "en-US" 了，我只看到了 accept-language，现在想要改中文该怎么办？
	
3. ![[./_resources/为Netflix_UWP设置系统代理和中文简体_-_追梦繁星.resources/spinner.svg]] [站元素主机](https://www.leeyiding.com/go/aHR0cDovL3d3dy56eXNncC5uZXQ=)     Windows 10 /   FireFox
	[回复](https://www.leeyiding.com/archives/53/comment-page-1?replyTo=524#respond-post-53)
	[2021 年 05 月 14 日](https://www.leeyiding.com/archives/53/comment-page-1#comment-524)
	
	感谢分享 赞一个
	
4. ![[./_resources/为Netflix_UWP设置系统代理和中文简体_-_追梦繁星.resources/spinner.svg]] [今日头条](https://www.leeyiding.com/go/aHR0cHM6Ly93d3cudGlhdGlhdG91dGlhby5jb20=)     Windows 7 /   Google Chrome
	[回复](https://www.leeyiding.com/archives/53/comment-page-1?replyTo=509#respond-post-53)
	[2021 年 03 月 25 日](https://www.leeyiding.com/archives/53/comment-page-1#comment-509)
	
	文章不错非常喜欢
	
5. ![[./_resources/为Netflix_UWP设置系统代理和中文简体_-_追梦繁星.resources/spinner.svg]] [今日](https://www.leeyiding.com/go/aHR0cHM6Ly93d3cuamlucmlyZXNvLmNvbQ==)     Windows 10 /   Google Chrome
	[回复](https://www.leeyiding.com/archives/53/comment-page-1?replyTo=494#respond-post-53)
	[2021 年 03 月 08 日](https://www.leeyiding.com/archives/53/comment-page-1#comment-494)
	
	支持一下交个朋友
	
6. ![[./_resources/为Netflix_UWP设置系统代理和中文简体_-_追梦繁星.resources/001517d5c00315f5f237950f007e2978.jpeg]] [席晓欢](https://www.leeyiding.com/go/aHR0cDovL3dvemhpZGFvbGUuY29tLmNuL3RpYW96aHVhbi8%7CdXJsPWh0dHA6Ly93b3poaWRhb2xlLmNvbS5jbi8=)     Windows 10 /   QQ 浏览器
	[回复](https://www.leeyiding.com/archives/53/comment-page-1?replyTo=430#respond-post-53)
	[2020 年 12 月 03 日](https://www.leeyiding.com/archives/53/comment-page-1#comment-430)
	
	看到你的网站，觉得很不错，希望能与你互相友情链接…
	我的网站：建站知道网 - wozhidaole.com.cn/
	
	如果同意的话，回复后互相上链接！
	
7. ![[./_resources/为Netflix_UWP设置系统代理和中文简体_-_追梦繁星.resources/001517d5c00315f5f237950f007e2978.jpeg]] [灵异故事](https://www.leeyiding.com/go/aHR0cHM6Ly93d3cuaW50ZXJsaW5pbmctdG0uY29tLw==)     Windows 10 /   FireFox
	[回复](https://www.leeyiding.com/archives/53/comment-page-1?replyTo=406#respond-post-53)
	[2020 年 10 月 10 日](https://www.leeyiding.com/archives/53/comment-page-1#comment-406)
	
	日常打卡～加油 -\_-![[./_resources/为Netflix_UWP设置系统代理和中文简体_-_追梦繁星.resources/keai.png]]
	
8. ![[./_resources/为Netflix_UWP设置系统代理和中文简体_-_追梦繁星.resources/9c1f881af8a4c0e5d84528aae6271c7d.png]] [Catyo](https://www.leeyiding.com/go/aHR0cHM6Ly9ibG9nLmNhdHlvLmNuLw==)     Windows 10 /   Google Chrome
	[回复](https://www.leeyiding.com/archives/53/comment-page-1?replyTo=373#respond-post-53)
	[2020 年 08 月 05 日](https://www.leeyiding.com/archives/53/comment-page-1#comment-373)
	
	音质
	

Copyright © 2023 [追梦繁星](https://www.leeyiding.com/)

[京 ICP 备 20000749 号](http://beian.miit.gov.cn/) • Powered by [Typecho](http://typecho.org/) • Theme [Mirages](https://get233.com/archives/mirages-intro.html)

本站点已安全运行：4 年 286 天 23 小时 23 分 | 网站加载耗时 500 ms
