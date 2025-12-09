---
source: https://ycycxz.com/main/2022/10/15/vmess/#docusaurus_skipToContent_fallback
---
# vmess节点搭建

[![[./_resources/vmess节点搭建__ycycxz.com.resources/main.webp]]](https://youtube.com/@Mainians)

[Mainians](https://youtube.com/@Mainians)
October 15, 2022 · One min read

[有偿 远程协助
Paid Support](https://tx.me/ycycxz)

1. 1
	
	### 下载[#​](https://ycycxz.com/main/2022/10/15/vmess/#%E4%B8%8B%E8%BD%BD)
	
	    wget -q4O /usr/local/bin/ycycxz-sing-box https://ycycxz.com/dl/ycycxz-sing-box-linux-amd64chmod +x /usr/local/bin/ycycxz-sing-boxmkdir -p /etc/ycycxz-sing-boxycycxz-sing-box version
	
2. 2
	
	### 配置[#​](https://ycycxz.com/main/2022/10/15/vmess/#%E9%85%8D%E7%BD%AE)
	
	/etc/ycycxz-sing-box/ycycxz.json
	
	    cat <<EOF > /etc/ycycxz-sing-box/ycycxz.json{  "inbounds": [    {      "type": "vmess",      "listen_port": 42324,      "users": [        {          "uuid": "1023839a-5910-5af2-8679-bb6c0b53c6db",          "alterId": 0        }      ]    }  ]}EOF
	
3. 3
	
	### systemd[#​](https://ycycxz.com/main/2022/10/15/vmess/#systemd)
	
	    cat <<EOF > /etc/systemd/system/ycycxz-sing-box.service[Unit]Description=ycycxz-sing-boxAfter=network-online.target[Service]LimitAS=infinityLimitNOFILE=infinityLimitNPROC=infinityTasksMax=infinityExecStart=/usr/local/bin/ycycxz-sing-box run -c /etc/ycycxz-sing-box/ycycxz.json[Install]WantedBy=multi-user.targetEOFsystemctl daemon-reloadsystemctl enable ycycxz-sing-box
	
4. 4
	
	### 启动[#​](https://ycycxz.com/main/2022/10/15/vmess/#%E5%90%AF%E5%8A%A8)
	
	    systemctl start ycycxz-sing-box#systemctl status ycycxz-sing-box
	
5. 5
	
	### 卸载[#​](https://ycycxz.com/main/2022/10/15/vmess/#%E5%8D%B8%E8%BD%BD)
	
	    systemctl stop ycycxz-sing-boxrm /etc/systemd/system/ycycxz-sing-box.servicesystemctl daemon-reloadrm -rf /etc/ycycxz-sing-boxrm -f /usr/local/bin/ycycxz-sing-box
	
6. 6
	
	### 客户端[#​](https://ycycxz.com/main/2022/10/15/vmess/#%E5%AE%A2%E6%88%B7%E7%AB%AF)
	
	小火煎等
	

* 7
	
	### 视频教程[#​](https://ycycxz.com/main/2022/10/15/vmess/#%E8%A7%86%E9%A2%91%E6%95%99%E7%A8%8B)
	
How was it? Did this tutorial work?  Yes  [No](https://github.com/mainians/YouTube/issues/new?body=The%20tutorial%20on%3A%0A%0Ahttps%3A%2F%2Fycycxz.com%2Fmain%2F2022%2F10%2F15%2Fvmess%2F%23docusaurus_skipToContent_fallback%0A%0AHere%27s%20what%20went%20wrong%3A%0A%0A%3C%21--%20Insert%20command%20output%20and%20details.%20Thank%20you%20for%20reporting%21%20%3A%29%20--%3E&title=Tutorial%20on%20https%3A%2F%2Fycycxz.com%2Fmain%2F2022%2F10%2F15%2Fvmess%2F%23docusaurus_skipToContent_fallback%20failed)

**Tags:**

* [vmess](https://ycycxz.com/main/tags/vmess)

* [sing-box](https://ycycxz.com/main/tags/sing-box)
