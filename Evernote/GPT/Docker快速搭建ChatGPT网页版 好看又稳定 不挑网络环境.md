---
source: https://kejilion.blogspot.com/2023/02/dockerchatgpt.html
---
### Docker快速搭建ChatGPT网页版 好看又稳定 不挑网络环境

\- [二月 20, 2023](https://kejilion.blogspot.com/2023/02/dockerchatgpt.html)

[![[./_resources/Docker快速搭建ChatGPT网页版_好看又稳定_不挑网络环境.resources/AVvXsEjoZ8J6kenQe-4soo6dbz25B45a_5F5grfE9tY2FABl3u.png]]](https://blogger.googleusercontent.com/img/a/AVvXsEjoZ8J6kenQe-4soo6dbz25B45a_5F5grfE9tY2FABl3uuzqkXDvoIvzfKtN9Sk5hYH1e34YiEQtxgypLPptKc70mEhs6JiLNZV4Qn1kjwg3TRU-I9okB3ENSTKz_zQqQg2i8-b3H1bEknGhIaKt6Y4ltUFemX_lwVZjdnp5wfT289y65rq-di3kV8h)
 

**已经有帐号了。获取自己的OPENAI的APIkey**

<https://platform.openai.com/account/api-keys>

**更新环境**

apt update -y  && apt upgrade -y && apt install -y curl wget sudo socat

**安装 Docker**

curl -fsSL https://get.docker.com -o get-docker.sh && sh get-docker.sh

curl -L "https://github.com/docker/compose/releases/download/v2.16.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose 

chmod +x /usr/local/bin/docker-compose

**创建GPT目录，创建配置文件**

cd /home/ && mkdir gpt && cd gpt && nano docker-compose.yml

**compose配置代码**

version: '3.8'

services:

  app:

    image: jason61/gpt-web:latest

    ports:

      - 3002:3002

    environment:

      OPENAI\_API\_KEY**:** **Zeix8v6K00KpP**      

      **# 用自己的API KEY   冒号后要带一个空格再输入key**

**运行指令**

cd /home/gpt && docker-compose up -d

太刁了，支持本地存储，支持语义上下文！感谢TG小伙伴感谢GitHub原作者

**原作者GitHub地址**

<https://github.com/Chanzhaoyu/chatgpt-web>

[![[./_resources/Docker快速搭建ChatGPT网页版_好看又稳定_不挑网络环境.resources/AVvXsEhF4JaGvgA3GnIaW2OiO-CxMwtJYSQizK4N2mzI57aE2k.png]]](https://blogger.googleusercontent.com/img/a/AVvXsEhF4JaGvgA3GnIaW2OiO-CxMwtJYSQizK4N2mzI57aE2keAKaQDEB2oQrXUCq2U8vwUv7u_QvRHdf2JctKgOSSZ93htDpxb3wrwPpXQN3WzC8LHa25qohdUv7h6i1N2y0VCt0Nj5s0fVNFzNwd7wg0KmdjqpcSKh4Kd9dxxTLN3-NvXIhQnN7lOaIMB)

**创建nginx目录结构**

mkdir -p /home/nginx

touch /home/nginx/nginx.conf

mkdir -p /home/nginx/certs

**申请证书**

curl https://get.acme.sh | sh

~/.acme.sh/acme.sh --register-account -m xxxx@gmail.com

~/.acme.sh/acme.sh --issue -d **gpt.kjlion.ga** --standalone

**下载证书**

~/.acme.sh/acme.sh --installcert \-d **gpt.kjlion.ga**  \--key-file /home/nginx/certs/key.pem --fullchain-file /home/nginx/certs/cert.pem

**进入目录编辑文件**

cd /home/nginx/ && nano nginx.conf

**反向代理配置，代理指定IP加端口**

events {

    worker\_connections  1024;

}

http {

  client\_max\_body\_size 1000m;  

  #上传限制参数1G以内文件可上传

  server {

    listen 80;

    server\_name **gpt.kjlion.ga**;

    return 301 https://$host$request\_uri;

  }

  server {

    listen 443 ssl;

    server\_name **gpt.kjlion.ga**;

    ssl\_certificate /etc/nginx/certs/cert.pem;

    ssl\_certificate\_key /etc/nginx/certs/key.pem;

    location / {

      proxy\_pass http://**0.0.0.0:3002**;

      proxy\_set\_header Host $host;

      proxy\_set\_header X-Real-IP $remote\_addr;

      proxy\_set\_header X-Forwarded-For $proxy\_add\_x\_forwarded\_for;

    }

  }

}

**部署容器**

docker run -d --name nginx -p 80:80 -p 443:443 -v /home/nginx/nginx.conf:/etc/nginx/nginx.conf -v /home/nginx/certs:/etc/nginx/certs -v /home/nginx/html:/usr/share/nginx/html nginx:latest

**查看运行状态**

docker ps -a

**开机自启动**

docker update --restart=always nginx

docker update --restart=always gpt-app-1

Docker常用命令

<https://kejilion.blogspot.com/2023/02/docker.html>
