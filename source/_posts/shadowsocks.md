---
title: "ShadowSocks 教程"
date: 2015-10-06 11:02:30
categories: tools
---
# ShadowSocks

## 服务器
1. 安装服务

   	apt-get install python-pip
   	pip install shadowsocks
   安装完成后使用`ssserver -p 443 -k password -m aes-256-cfb`进行测试，从客户端发起连接，发现能科学上网了
2. 设置开机启动

   	mkdir /etc/shadowsocks
   	vim /etc/shadowsocks/config.json
   	# 在config.json中复制入以下配置：
   	{
   	    "server":"0.0.0.0",
   	    "server_port":8388,
   	    "local_address": "127.0.0.1",
   	    "local_port":1080,
   	    "password":"yourpassword",
   	    "timeout":300,
   	    "method":"aes-256-cfb"
   	}
   其中端口和密码可按需进行修改
3. 启动服务

   	ssserver -c /etc/shadowsocks/config.json

## 客户端
1. 安装必要的软件

        yum install -y epel-release
        yum install -y python
        yum install python-pip privoxy
        pip install shadowsocks
2. 修改相应的配置

        echo "forward-socks5t / 127.0.0.1:1080 ." >> /etc/privoxy/config
        echo "export http_proxy=http://127.0.0.1:8118" >> /etc/profile
        export http_proxy=http://127.0.0.1:8118
3. 启动服务

        nohup sslocal -c /share/shadowsocks/config.json &
        service privoxy restart