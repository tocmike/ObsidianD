---
source: https://www.wangxuyin.cn/203.html
---
# [WANGXUYIN](https://blog.wangxuyin.cn/)

Ctrl+c,Ctrl+v expert！

 [![[./_resources/AWS_EC2_开启root用户及使用密码登录_-_WANGXUYIN.resources/svg_1.svg]]](https://weibo.com/sbgay)[![[./_resources/AWS_EC2_开启root用户及使用密码登录_-_WANGXUYIN.resources/svg_2.svg]]](https://blog.wangxuyin.cn//feed)

* [首页](https://blog.wangxuyin.cn/)

* [关于](https://www.wangxuyin.cn/about.html)

## AWS EC2 开启root用户及使用密码登录

September 23, 2020

### AWS EC2开启root用户登录

AWS EC2 创建时会要求使用密钥对登录，或自己生成或自动生成，密钥对生成后只能下载一次。

如果要使用root + 密码的方式登录按以下操作即可:

1.使用下载的密钥对登录，注意用户名为`ec2-user`。
2.登录后执行:
`sudo passwd root`可直接设置root用户的密码。
`su root`输入设置的密码切换至root.
3.编辑 `sshd` 服务配置文件：
`vi /etc/ssh/sshd_config`
将`PermitRootLogin` 设置为`yes`,
确保`passwordAuthentication` 为`yes`.
保存后重启sshd服务`service sshd restart`或者`/bin/systemctl restart sshd.service`即可。
4.`exit` 退出`su`的`shell`,`logout`退出 `ec2-user`的shell,使用root账号加密码登录吧。

![[./_resources/AWS_EC2_开启root用户及使用密码登录_-_WANGXUYIN.resources/ad4d92619c815de6ba963b6fce014b60.jpeg]]

### SBGAY

作者描述
 ![[./_resources/AWS_EC2_开启root用户及使用密码登录_-_WANGXUYIN.resources/svg_3.svg]] Beijing![[./_resources/AWS_EC2_开启root用户及使用密码登录_-_WANGXUYIN.resources/svg_4.svg]] [http://blog.wangxuyin.cn](http://blog.wangxuyin.cn/)

### 添加新评论

称呼 \*

Email \*

网站

内容

Made in [love](http://blog.wangxuyin.cn/). ![[./_resources/AWS_EC2_开启root用户及使用密码登录_-_WANGXUYIN.resources/svg_5.svg]] Blog since 2015.
