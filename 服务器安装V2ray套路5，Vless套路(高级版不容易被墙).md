- 教学视频
- https://www.youtube.com/watch?v=SYAuvtF9wMY
- https://meta-tube.de/videos/watch/923f8cc5-e0e7-4a1a-816d-7fe04e02bbda
-
-
- 一键脚本 和安卓配置 教学视频
- Q:\OD同步\OneDrive - Business\软件\IT软件\翻墙搭建视频\vless搭建视频\jennie的vless手动搭建视频
-
- https://www.youtube.com/watch?v=PvM6Xw6ocDk
- jennie的图文教程链接
- Q:\OD同步\OneDrive - Business\软件\IT软件\翻墙搭建视频\vless搭建视频\jennie的vless手动搭建视频\jennie的说明网页
-
-
- https://jeanniestudio.top/2020/08/24/%E6%89%8B%E5%8A%A8%E6%90%AD%E5%BB%BAvless+tcp+tls/
-
-
- 安装完使用后问题1断联故障排查套路
	- 先ping一下IP地址 域名
	- 关闭clouldflare（这个是上不了网的主要原因）
	- xshell打开服务器
		- 重启nginx
			- systemctl restart nginx
	- 本地V2RAY
		- 首选项 连接设置
		- 使用本地DNS 关闭
		- 绕过中国大陆 关闭
		- 绕过中国大陆 重新打开
-
-
- 准备VPS 准备域名 绑定这个域名到VPS
-
-
-
- 前言
- v2ray的官网转移到了https://www.v2fly.org/，而且添加了新的传输协议：vless,VLESS 是一个无状态的轻量传输协议，它分为入站和出站两部分，可以作为 V2Ray 客户端和服务器之间的桥梁。
- vmess和vless的区别
- vless与VMess 不同，VLESS 不依赖于系统时间，认证方式同样为 UUID，但不需要 alterId。
- 官方附加说明：尽管 Websocket+TLS+Web 可能称得上是现阶段最好的方案，但绝对不是推荐新手一上来就尝试的方案，更不是 V2Ray 唯一的用法。同时，你应当了解，每个地区的网络状况不同 (主要指对不同协议的 QoS 程度)，你可以将所有配置都尝试一遍来寻找最适合自己的，尽量少问、最好不问“为什么我的 V2Ray 这么慢？”这样的问题。
-
-
- 准备条件：
- 1.一个vps，我使用谷歌云演示
- 2.一个域名（不会申请域名的小伙伴可以看这个视频：）,域名和vps的ip地址关联成功
- 3域名解析到了服务器
- 4域名托管到了cloudflare
-
- 安装步骤
- 用funal shell连接主机
- 或 XSHELL连接主机
- 申请证书
- 一、安装acme.sh脚本
- 1. 先切换到 root
```
sudo -i
```

- 如果出现错误sudo：command not found
- 就说明没有这个命令，那么就安装sudo命令
  - https://blog.csdn.net/hello_1995/article/details/109222650
- 2.安装依赖，debian或ubuntu执行：
```
apt-get update && apt-get -y install socat

```


- centos执行：
```
yum update && yum -y install socat
```


- 用安装acme脚本申请证书套路
- 用[acme.sh安装证书套路](acme.sh安装证书套路.md)
-
-
-
-
-
-
-
- 安装nginx（这里开始 在jennie那个页面复制 jennie的图文教程链接 https://jeanniestudio.top/2020/08/24/%E6%89%8B%E5%8A%A8%E6%90%AD%E5%BB%BAvless+tcp+tls/）
- 1.安装依赖包
- 下载/安装 openssl-1.1.1（使 nginx 支持TLS 1.3）
```
cd /usr/local/src
```


```
wget -nc --no-check-certificate https://www.openssl.org/source/openssl-1.1.1g.tar.gz -P /usr/local/src
```


- 已经下载到 /user/local/src 下面了
	  - ![image.png](../assets/image_1630128978195_0.png)
		- 如果出现
		- 
		- 
```
wget：command not found 
```

就说明找不到 wget 命令
			- 那么就输入
```
apt install wget
```

```
Y
```

			
			
 - 解压

```
tar -zxvf /usr/local/src/openssl-1.1.1.tar.gz -C /usr/local/src
```
	
-  ![image.png](../assets/image_1630128993554_0.png)
		
- 其他依赖
- 如果是debian和ubuntu执行下面语句：

```
apt -y install build-essential libpcre3 libpcre3-dev zlib1g-dev git dbus manpages-dev aptitude g++
```

- 如果是centos执行：
```
yum -y groupinstall "Development tools"

```


```
yum -y install pcre pcre-devel zlib-devel epel-release gcc gcc-c++

```



- 2.下载解压nginx源码
```
wget -nc --no-check-certificate http://nginx.org/download/nginx-1.18.0.tar.gz -P /usr/local/src
```


 - 解压 nginx 源码
```
tar -zxvf /usr/local/src/nginx-1.18.0.tar.gz -C /usr/local/src
```


  - 解压完出现一个 nginx 文件夹
		- ![image.png](../assets/image_1630129014194_0.png)
- 3.编译和安装nginx
- 编译配置
	- 先输入这个切换到这个目录下
```
cd /usr/local/src/nginx-1.18.0
```


	
   - ![image.png](../assets/image_1630129022297_0.png)
 再输入 创建一个文件夹
 ```
 mkdir /etc/nginx
 ```
 
	  + （注意这个加号不是代码里的内容）
```
	 ./configure --prefix=/etc/nginx \
	  with-http_ssl_module \
	 with-http_gzip_static_module \
	 with-http_stub_status_module \
	 with-pcre \
	 with-http_realip_module \
	 with-http_flv_module \
	 with-http_mp4_module \
	 with-http_secure_link_module \
	 with-http_v2_module \
	 with-cc-opt='-O3' \
	 with-openssl=../openssl-1.1.1g
```
	  
编译&&安装
```
make && make install
```
- 修改基本配置
```
		sed -i 's/#user nobody;/user root;/' /etc/nginx/conf/nginx.conf
		sed -i 's/worker_processes 1;/worker_processes 3;/' /etc/nginx/conf/nginx.conf
		sed -i 's/ worker_connections 1024;/ worker_connections 4096;/' /etc/nginx/conf/nginx.conf
		sed -i '$i include conf.d/*.conf;' /etc/nginx/conf/nginx.conf
		sed -i '/listen 80;/a\ return 301 https://$http_host$request_uri;' /etc/nginx/conf/nginx.conf
```
		
		
		
		
- 删除临时文件
```
		rm -rf /usr/local/src/nginx-1.18.0
		rm -rf /usr/local/src/nginx-1.18.0.tar.gz
		rm -rf /usr/local/src/openssl-1.1.1g
		rm -rf /usr/local/src/openssl-1.1.1g.tar.gz
```
 添加配置文件
 ```
mkdir /etc/nginx/conf/conf.d
```

 ![image.png](../assets/image_1630129036243_0.png)
- 注意：将下面代码中的端口号、域名和伪装网站目录修改成你自己的
	- （注意;证书位置不对）
	
```
		cat >/etc/nginx/conf/conf.d/default.conf <<EOF
			server {
				listen 6443自定义一个非的端口，可以设成6443;
				server_name 你的域名;
				root /usr/wwwroot;
				ssl on;
				ssl_certificate /证书所在的路径/fullchan.crt;
				ssl_certificate_key /证书所在的路径/证书钥匙名.key;
					ssl_ciphers TLS13-AES-256-GCM-SHA384:TLS13-CHACHA20-POLY1305-SHA256:TLS13-AES-128-GCM-SHA256:TLS13-AES-128-CCM-8-SHA256:TLS13-AES-128-CCM-SHA256:EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+ECDSA+AES128:EECDH+aRSA+AES128:RSA+AES128:EECDH+ECDSA+AES256:EECDH+aRSA+AES256:RSA+AES256:EECDH+ECDSA+3DES:EECDH+aRSA+3DES:RSA+3DES:!MD5;
				ssl_prefer_server_ciphers on;
				ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
				ssl_session_cache shared:SSL:50m;
				ssl_session_timeout 1d;
				ssl_session_tickets on;
			}
		EOF
```
 - 记下来我定义的端口比如6443，这个是nginx的监听端口
 - 改好以后例如：
	 - ![image.png](../assets/image_1630129071978_0.png){: height 228, : width 660}
	
	
```
		cat >/etc/nginx/conf/conf.d/default.conf <<EOF
			server {
				listen 6443;
				server_name www.ribenjapan101.xyz;
				root /usr/wwwroot;
				ssl on;
				ssl_certificate /data/www.ribenjapan101.xyz/fullchan.crt;
				ssl_certificate_key /data/www.ribenjapan101.xyz/www.ribenjapan101.xyz.key;
				ssl_ciphers TLS13-AES-256-GCM-SHA384:TLS13-CHACHA20-POLY1305-SHA256:TLS13-AES-128-GCM-SHA256:TLS13-AES-128-CCM-8-SHA256:TLS13-AES-128-CCM-SHA256:EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+ECDSA+AES128:EECDH+aRSA+AES128:RSA+AES128:EECDH+ECDSA+AES256:EECDH+aRSA+AES256:RSA+AES256:EECDH+ECDSA+3DES:EECDH+aRSA+3DES:RSA+3DES:!MD5;
				ssl_prefer_server_ciphers on;
				ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
				ssl_session_cache shared:SSL:50m;
				ssl_session_timeout 1d;
				ssl_session_tickets on;
			}
		EOF
```
	


- ![image.png](../assets/image_1630129093444_0.png)
-
- ![image.png](../assets/image_1630129108455_0.png)
-
-
-
-
- 4.配置nginx服务
- 创建服务文件
```
		cat >/etc/systemd/system/nginx.service <<EOF
		[Unit]
		Description=The NGINX HTTP and reverse proxy server
		After=syslog.target network.target remote-fs.target nss-lookup.target
		[Service]
		Type=forking
		 PIDFile=/etc/nginx/logs/nginx.pidExecStartPre=/etc/nginx/sbin/nginx -t
		ExecStart=/etc/nginx/sbin/nginx -c /etc/nginx/conf/nginx.conf
		ExecReload=/etc/nginx/sbin/nginx -s reload
		ExecStop=/bin/kill -s QUIT \$MAINPID
		PrivateTmp=true
		[Install]
		WantedBy=multi-user.target
		EOF
```
	

 - ![image.png](../assets/image_1630129120286_0.png)
	
 - ![image.png](../assets/image_1630129129474_0.png)
	-
	-
-
-
-
-
-
- 退出finalshell 再进入
-
- 加载并启动nginx

```
systemctl daemon-reload
```



- 首先这个是启动nginx

```
systemctl restart nginx
```


- 然后重启一下nginx


- 查看一下nginx状态
```
systemctl status nginx
```


- 然后按住ctril +c可以退出状态查看页


- 其他命令
```
systemctl start nginx
```



- 启动

```
systemctl stop nginx
```


- 停止
	- 假如nginx启动失败怎么弄，出现Failed to start The NGINX HTTP and reverse proxy server怎么办
		-
		- ![image.png](../assets/image_1630129150453_0.png)
		- 用
			- [nginx启动失败的拯救套路](nginx启动失败的拯救套路.md)
			- nginx启动失败 拯救套路
-
-
-
-
-
-
-
-
- 安装伪装网站
- 创建伪装网站目录
```
mkdir /usr/wwwroot
```

- 回到root根目录
```
cd /root
```


- 下载安装伪装网站
```
git clone https://github.com/JeannieStudio/Programming.git /usr/wwwroot
```
  - 如果出现fatal: Unable to read current working directory: No such file or directory
		- 就说明这个目录已经删掉了
		- 不如切换到root 目录下
```
cd /root
```



  - 然后再重新执行
```
git clone https://github.com/JeannieStudio/Programming.git /usr/wwwroot
```
- 浏览器访问一下伪装网站
```
https://我的域名:6443/
```

- 域名：6443
		- 6443是前面设置的nginx的监听端口
	- 如果页面打不开
		- ![image.png](../assets/image_1630129165497_0.png)
		-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
- 安装v2ray
- 用官方快速安装脚本，地址：https://github.com/v2fly/fhs-install-v2ray
- 安装cURL
- debian或ubuntu:
	-
	``` 
	apt install curl
	
	```
	 - 如果出现pt: command not found 就是pt命令没安装
```
apt install pt
```

- centOS：
```
yum makecache
```

```
yum install curl
```

- 使用一键脚本下载并安装v2ray 安裝最新發行的 geoip.dat 和 geosite.dat
	- 打开网页
		- https://github.com/v2fly/fhs-install-v2ray
	- 安裝和更新 V2Ray
	``` 
	bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
	```
	- 安裝最新發行的 geoip.dat 和 geosite.dat
	```
	bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-dat-release.sh)
	```
	- ![image.png](../assets/image_1630129181732_0.png)
	-
		-
		-
		-
		-
		- 如果安装失败可以试试 其他操作 移除 V2Ray
``` 
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh) --remove
```

- 安装完成以后要修改一下后台服务文件
	- jennie的图文教程链接
	- https://jeanniestudio.top/2020/08/24/%E6%89%8B%E5%8A%A8%E6%90%AD%E5%BB%BAvless+tcp+tls/
	- +
```
		cat >/etc/systemd/system/v2ray.service <<EOF
		[Unit]
		Description=V2Ray Service
		After=network.target nss-lookup.target
	
		[Service]
		User=root
		CapabilityBoundingSet=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
		AmbientCapabilities=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
		NoNewPrivileges=true
		 Environment=V2RAY_LOCATION_ASSET=/usr/local/share/v2ray/
		ExecStart=/usr/local/bin/v2ray -config /usr/local/etc/v2ray/config.json
		Restart=on-failure
		
		[Install]
		WantedBy=multi-user.target
		EOF
```
- 配置v2ray
- 生成uuid的命令，更改uuid：
```
cat /proc/sys/kernel/random/uuid 
```
- 把uuid记下来
	- 得到
	-
- 配置v2ray服务器
- 参考官方给的配置即可：https://github.com/v2fly/v2ray-examples
	- 找到VLESS-TCP-TLS (maximal by rprx)这个版本点击进去
	- 这里官方给出了三个配置文件
	- 因为nginx已经配置好了，我们点击打开config_server.json的配置
		- 复制过来 修改红色标注的地方 删除中文注释 （注意前面设置的证书的路径是/data/example.com）
			- 创建文件

```
cat >/usr/local/etc/v2ray/config. json
```


- 进去以后
```
{
	"log": {
	"loglevel": "warning"
	},
	"inbounds": [
	{
	"port": 443,
	"protocol": "vless",
	"settings": {
	"clients": [
	{
	"id": "", // 填写你的 UUID
	"level": 0,
	"email": "love@v2fly.org"
	}
	],
	"decryption": "none",
	"fallbacks": [
	{
	"dest": "/dev/shm/default.sock",
	"xver": 1
	},
	{
	"alpn": "h2",
	"dest": "/dev/shm/h2c.sock",
	"xver": 1
	}
	]
	},
	"streamSettings": {
	"network": "tcp",
	"security": "tls",
	"tlsSettings": {
	"alpn": [
	"h2",
	"http/1.1"
	],
	"certificates": [
	{
	"certificateFile": "/path/to/fullchain.crt", // 换成你的证书，绝对路径 /data/example.com/fullchain.crt
	"keyFile": "/path/to/private.key" // 换成你的私钥，绝对路径/data/example.com/example.com.key
	}
	]
	}
	}
	}
	],
	"outbounds": [
	{
	"protocol": "freedom"
	}
	]
	}

```






- 证书的绝对路径在
	-
	- ![image.png](../assets/image_1630129200129_0.png)
-
-
	- ctril c退出
-
-
-
-
-
-
-
-
-
-
-
- 如果你不放心可以看一下是否保存成功了
```
cat /usr/local/ect/v2ray/config. json

```


 - 如果没成功说明 cat 命令
- 如果没成功就找到这个文件双击打开 cat /usr/local/ect/v2ray/config.json



- 把上面代码复制进去
	- ![image.png](../assets/image_1630129215228_0.png){:height 705, :width 587}
	-
-
-
-
- 执行命令：
```
systemctl daemon-reload
```


  - 启动 v2ray
```
systemctl restart v2ray
```



  - 重启v2ray
```
systemctl status v2ray
```




- 查看v2ray状态
	
- 其他命令
```
systemctl enable v2ray
```


  - 相关命令：
```
systemctl start v2ray
```
- 启动v2ray
```
systemctl start v2ray
```
重启v2ray
 ```
 systemctl stop v2ray
 ```
- 停止v2ray
- 客户端下载和配置
- 1.win平台：
- 下载Qv2ray，使用手册：这里：https://qv2ray.net/getting-started/
	- 第一步：下载 Qv2ray软件
		- https://github.com/Qv2ray/Qv2ray/releases
			- ![image.png](../assets/image_1630129242446_0.png)
			-
	- 第二步：下载 V2Ray 核心
		- 拉到下面找到 手动下载 链接
			- https://hub.fastgit.org/v2fly/v2ray-core/releases
				- ![image.png](../assets/image_1630129248561_0.png)
				-
			- 下载好后解压
			- 解压后打开文件包
		- 注意按照他说的在本地Qv2ray\Qv2ray.v2.7.0-pre1.Windows-x64\deployment文件夹下新建文件夹
		- /config/vcore
		- 把上面的文件全部复制进去
	- 第三步：向 Qv2ray 中导入节点
		- 找到刚才的网页 官方给的配置：https://github.com/v2fly/v2ray-examples
			- 找到VLESS-TCP-TLS (maximal by rprx)这个版本点击进去
			- 这里官方给出了三个配置文件
			- config_client.json这个文件点进去
				- 复制代码到本地记事本 修改红色标注部分 然后保存 把文件名改成config.json,然后复制进QV2RAY根目录
					- +
```
						{
						"log": {
						"loglevel": "warning"
						},
						"inbounds": [
						{
						"port": 10800,
						"listen": "127.0.0.1",
						"protocol": "socks",
						"settings": {
						"udp": true
						}
						}
						],
						"outbounds": [
						{
						"protocol": "vless",
						"settings": {
						"vnext": [
						{
						"address": "example.com", // 换成你的域名或服务器 IP（发起请求时无需解析域名了）
						"port": 443,
						"users": [
						{
						"id": "", // 填写你的 UUID
						"encryption": "none",
						"level": 0
						}
						]
						}
						]
						},
						"streamSettings": {
						"network": "tcp",
						"security": "tls",
						"tlsSettings": {
						"serverName": "example.com" // 换成你的域名
						}
						}
						}
						]
						}
```
 - 或第三步：在 Qv2ray 中手动输入节点
		-
	- 第四步：让浏览器/软件走 Qv2ray 代理
		-
- （1）下载Qv2ray: https://github.com/Qv2ray/Qv2ray/releases
- windows用户下载：Qv2ray.v2.6.3.Windows-x64.7z，并解压即可。
-
-
-
- V2RAYNG安卓版各个参数填写说明
	- 别名
		- 随便填
	- 地址
		- 申请的域名
			- example.com
	- 端口
		- 443
	- 用户id
		- uuid
			- 打开XSHELL服务器连接 VPS输入命令
			- 得到
			- fa168a28-9a0d-4265-8653-7dcc0a59d666
	- 流控
	- 加密
		- none
	- 底层传输方式
	- 传输协议
		- tcp
	- 伪装类型
		- none
	- 伪装域名
		- singaporexinjiapo777.xyz：6443
	- path
		- /path
	- 底层传输安全
		- tls
	- 跳过证书验证
		- false
-
-
- 补充知识 端口原理
	-
	- 流量包原理：
	- （浏览器【8889端口出】 -win10的V2rayN【8889端口进】经过加密【443端口出】） - TCP协议流过长城防火墙 - （ VPS上的 【443端口进】服务器上的Nginx或Caddy - 【转发到本地的24119端口的服务上】就是V2ray服务器解密） - 目标服务器youtube
		- ![image.png](../assets/image_1630129313715_0.png){:height 338, :width 660}
		-
	- 解释
		- 8889端口是开始菜单》代理》
		- http请求是用80端口接
		- https请求是用443端口接
		-
		- ![image.png](../assets/image_1630129325085_0.png)
		-
		- ![image.png](../assets/image_1630129333619_0.png)
		-
		-
		-
		-
		-
		-
		-
		-
		-
		-
		-
		-
		-
-