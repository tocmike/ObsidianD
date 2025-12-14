---
source: https://blog.laoda.de/archives/npm-xui
---
# 【Docker魔法系列】NPM与XUI共存！Nginx Proxy Manager搭配X-UI实现Vless+WS+TLS 教程！

![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/u224cj-2.webp]]

[咕咕鸽](https://blog.laoda.de/s/about)
2022-05-19 / 72 评论 / 12 点赞 / 14,906 阅读 / 1,418 字 / 推送成功！

05/19

![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/svg_9.svg]] 温馨提示：
	本文最后更新于 2023-01-13，若内容或图片失效，请留言反馈。部分素材来自网络，若不小心影响到您的利益，请联系我们删除。

 [![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/zj36hf-0.gif]] 广告](https://blog.laoda.de/archives/vps-lcayun)

之前分享过搭建[可以与宝塔共存的一个“魔法”服务器状态监控应用——xui](https://blog.laoda.de/archives/xui)，支持Vmess+WS+TLS。

最近Docker视频出的比较多，前阵子又出现了宝塔国内版存在隐私泄露的问题，很多小伙伴其实都不用宝塔了，那么，在我们现在装了Nginx Proxy Manager（NPM）的环境下，`80`和`443`端口都转由NPM来管理了，可以让XUI和NPM共存吗？如何用NPM来反代XUI呢？今天我们就来折腾一下！

## 1\. 搭建环境

* 服务器：~~腾讯香港轻量应用服务器24元/月VPS一台~~展示用的服务器是[甲骨文圣何塞永久免费服务器](https://www.oracle.com/free)，本期搭建用的是[Vultr](https://loll.cc/vultr)的服务器，按小时计费，可随时销毁（最好是选**非大陆的服务器**）（[腾讯轻量购买链接](https://loll.cc/tx)）[移动、联通用户建议可以用Racknerd试试](https://blog.laoda.de/archives/cheap-vps-racknerd)

* 系统：Debian 10（[DD脚本](https://blog.laoda.de/archives/useful-script#dd%E7%9B%B8%E5%85%B3) 非必需DD用原来的系统也OK）
* 域名一枚，并做好解析到服务器上（[域名购买、域名解析](https://blog.laoda.de/archives/namesilo) [视频教程](https://www.bilibili.com/video/BV1Sy4y1k7kZ/)）
* 安装好Docker、Docker-compose（[相关脚本](https://blog.laoda.de/archives/hello-docker#5%E5%AE%89%E8%A3%85dockerdocker-compose)）
* 安装好Nginx Proxy Manager（[相关教程](https://blog.laoda.de/archives/nginxproxymanager)）

## 2\. 搭建视频

YouTube：<https://youtu.be/aYC4BTzbw8c>

## 3\. 搭建方式

### 3.1 服务器初始设置

服务器初始设置，参考

[**新买了一台服务器“必须”要做的6件小事**](https://blog.laoda.de/archives/vps-script)

[【Docker系列】不用宝塔面板，小白一样可以玩转VPS服务器！](https://blog.laoda.de/archives/hello-docker)

### 3.2 下载xui

新版（支持功能更多）：

__`bash <(curl -Ls https://raw.githubusercontent.com/FranzKafkaYu/x-ui/master/install.sh) 1`__

Bash

GitHub地址：<https://github.com/FranzKafkaYu/x-ui>

旧版：

__`bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh) 1`__

Bash

根据提示设置`端口信息`、`用户名`、`密码`。

### 3.3 登陆xui面板并配置

![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/sr0928-0.webp]]

切换最新版本：

![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/srde88-0.webp]]

修改面板路径：

![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/srgxxx-0.webp]]

记得`保存配置`。

添加`入站列表`:

![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/ss2h72-0.webp]]

如果配置，只需要修改`4`个地方：

![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/sst10j-0.webp]]

点击`添加`，之后点击`重启面板`

![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/stdsd0-0.webp]]

### 3.4 NPM配置

#### 2022-12-15更新简单方法

无需手动进入目录修改，只需要在面版即可操作！

选择任意一个NPM代理上，分别在下面两个位置填入下面两个部分内容：

![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/u4xzin-2.webp]]

代替了旧方法中的：

__  `location ^~ /fuckgfw {              #fuckgfw换成你前面设置的面板的url根路径       proxy_pass http://158.101.3.171:54331/fuckgfw;  # IP填服务器IP，这边不能填127.0.0.1，因为是在容器里，54331换成你xui面板的端口       proxy_set_header Host $host;       proxy_set_header X-Real-IP $remote_addr;       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;   } 123456`__

Nginx

![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/u51sws-2.webp]]

__  `location /plogger {                 # plogger填你前面设置的ws的路径           proxy_redirect off;           proxy_pass http://158.101.3.171:13997;    # IP填服务器IP，这边不能填127.0.0.1，因为是在容器里，13997换成你入站规则那边的IP           proxy_http_version 1.1;           proxy_set_header Upgrade $http_upgrade;           proxy_set_header Connection "upgrade";           proxy_set_header Host $http_host;           proxy_read_timeout 300s;           # Show realip in v2ray access.log           proxy_set_header X-Real-IP $remote_addr;           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;   } 123456789101112`__

Nginx

保存即可！

* * *

以下为旧方法，不建议采用

登陆服务器，来到NPM的安装路径下（这边假设大家都是用[【Docker系列】一个反向代理神器——Nginx Proxy Manager](https://blog.laoda.de/archives/nginxproxymanager)这篇文章的方法搭建的）：

__`cd /root/data/docker_data/npm 1`__

Bash

查看当前文件：

__`ls -al 1`__

Bash

![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/swrqhj-0.webp]]

我们的文件都在`data`目录下，一层一层找，找到我们的Nginx配置文件。

__`cd data/nginx/proxy_host 1`__

Bash

![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/syntz8-0.webp]]

可以看到两个文件（如果你NPM反代多的话，可能有很多个）

这边比较讨厌，是数字命名的，其实我们随便选一个也行。

![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/szyhi6-0.webp]] ![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/t15atw-0.webp]]

* * *

以上为旧方法，不建议采用。

在上述位置加入以下内容：

__  `location ^~ /fuckgfw {              #fuckgfw换成你前面设置的面板的url根路径       proxy_pass http://158.101.3.171:54331/fuckgfw;  # IP填服务器IP，这边不能填127.0.0.1，因为是在容器里，54331换成你xui面板的端口       proxy_set_header Host $host;       proxy_set_header X-Real-IP $remote_addr;       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;   }   location /plogger {                 # plogger填你前面设置的ws的路径           proxy_redirect off;           proxy_pass http://158.101.3.171:13997;    # IP填服务器IP，这边不能填127.0.0.1，因为是在容器里，13997换成你入站规则那边的IP           proxy_http_version 1.1;           proxy_set_header Upgrade $http_upgrade;           proxy_set_header Connection "upgrade";           proxy_set_header Host $http_host;           proxy_read_timeout 300s;           # Show realip in v2ray access.log           proxy_set_header X-Real-IP $remote_addr;           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;   } 123456789101112131415161718`__

Nginx

保存后，重启一下NPM。

__`cd /root/data/docker_data/npm    # 来到docker-compose.yml所在的文件夹下  docker-compose restart  # 重启 123`__

Bash

## 4\. 客户端连接

以小火煎为例子，其他客户端类似：

点击打开二维码：

![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/tt2r0u-0.webp]] ![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/tu4uac-0.webp]] ![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/tupyfd-0.webp]] ![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/tuzhi5-0.webp]]

完整配置参考：

![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/tvfzmm-0.webp]] ![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/tviqsr-0.webp]]

启动节点：

![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/tw10vi-0.webp]]

浏览器输入`https://ip.skk.moe/`

![[./_resources/【网页正文】NPM与XUI共存！Nginx_Proxy_Manager搭配X-UI实现Vless+WS+TLS_教程！.resources/twneod-0.webp]]

搞定！

## 5\. 注意事项（重要）

由于我们是直接修改的配置文件，所以，在反代的这个站点，不用轻易在NPM后台面板上修改原来的配置（比如打开，然后点确定），这样会破坏掉我们这边写的Nginx配置文件，导致节点无法正常使用。
