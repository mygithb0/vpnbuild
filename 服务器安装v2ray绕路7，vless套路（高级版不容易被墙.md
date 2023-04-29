教学视频

https://www.youtube.com/watch?v=SYAuvtF9wMY
https://meta-tube.de/videos/watch/923f8cc5-e0e7-4a1a-816d-7fe04e02bbda


一键脚本 和安卓配置 教学视频
Q:\OD同步\OneDrive - Business\软件\IT软件\翻墙搭建视频\vless搭建视频\jennie的vless手动搭建视频

https://www.youtube.com/watch?v=PvM6Xw6ocDk
jennie的图文教程链接
Q:\OD同步\OneDrive - Business\软件\IT软件\翻墙搭建视频\vless搭建视频\jennie的vless手动搭建视频\jennie的说明网页


https://jeanniestudio.top/2020/08/24/%E6%89%8B%E5%8A%A8%E6%90%AD%E5%BB%BAvless+tcp+tls/


安装完使用后问题1断联故障排查套路
先ping一下IP地址 域名
关闭clouldflare（这个是上不了网的主要原因）
shell打开服务器
重启nginx
systemctl restart nginx
本地V2RAY
首选项 连接设置
使用本地DNS 关闭
绕过中国大陆 关闭
绕过中国大陆 重新打开


准备VPS 准备域名 绑定这个域名到VPS

前言
v2ray的官网转移到了https://www.v2fly.org/，而且添加了新的传输协议：vless,VLESS 是一个无状态的轻量传输协议，它分为入站和出站两部分，可以作为 V2Ray 客户端和服务器之间的桥梁。
vmess和vless的区别
vless与VMess 不同，VLESS 不依赖于系统时间，认证方式同样为 UUID，但不需要 alterId。
官方附加说明：尽管 Websocket+TLS+Web 可能称得上是现阶段最好的方案，但绝对不是推荐新手一上来就尝试的方案，更不是 V2Ray 唯一的用法。同时，你应当了解，每个地区的网络状况不同 (主要指对不同协议的 QoS 程度)，你可以将所有配置都尝试一遍来寻找最适合自己的，尽量少问、最好不问“为什么我的 V2Ray 这么慢？”这样的问题。


准备条件：
1.一个vps，我使用谷歌云演示
2.一个域名（不会申请域名的小伙伴可以看这个视频：）,域名和vps的ip地址关联成功
3域名解析到了服务器
4域名托管到了cloudflare

安装步骤
用funal shell连接主机
或 XSHELL连接主机
申请证书
# 一、安装acme.sh脚本
## 1. 先切换到 root
```
sudo -i
```

如果出现错误sudo：command not found
就说明没有这个命令，那么就安装sudo命令
https://blog.csdn.net/hello_1995/article/details/109222650
## 2.安装依赖
如果是debian或ubuntu系统则执行：






```
apt-get update && apt-get -y install socat
```


0




如果是centos系统则执行

```
yum update && yum -y install socat
```







用安装acme脚本申请证书套路
# 用[acme.sh安装证书套路](acme.sh安装证书套路.md)

# 用[复制转换证书套路](复制转换证书套路.md)

# 安装nginx套路
安装nginx（这里开始 在jennie那个页面复制 jennie的图文教程链接 https://jeanniestudio.top/2020/08/24/%E6%89%8B%E5%8A%A8%E6%90%AD%E5%BB%BAvless+tcp+tls/）
1.安装依赖包
## 下载然后安装 openssl-1.1.1
（使 nginx 支持TLS 1.3）

注意：原理是把oppenssl和nginx的安装包压缩包都下载并解压到/usr/local/src文件夹里然后后面才好nginx安装到/etc/nginx文件夹里

#### 进入/usr/local/src文件夹
```
cd /usr/local/src
```

#### 下载openssl安装包到/usr/local/src文件夹里
```
wget -nc --no-check-certificate https://www.openssl.org/source/openssl-1.1.1.tar.gz -P /usr/local/src
```


已经下载到 /user/local/src 下面了

如果出现错误提示（不复制）

wget：command not found 


就说明找不到 wget 命令
那么就输入
```
apt install wget
```

```
Y
```

			
			
#### 解压openssl的压缩包文件到/usr/local/src/文件夹里

```
tar -zxvf /usr/local/src/openssl-1.1.1.tar.gz -C /usr/local/src
```



#### 安装其他依赖
如果是debian和ubuntu执行下面语句：

```
apt -y install build-essential libpcre3 libpcre3-dev zlib1g-dev git dbus manpages-dev aptitude g++
```



0



如果是centos执行：



```
yum -y groupinstall "Development tools"
```



```
yum -y install pcre pcre-devel zlib-devel epel-release gcc gcc-c++

```




## 2.下载解压nginx源码
```
wget -nc --no-check-certificate http://nginx.org/download/nginx-1.18.0.tar.gz -P /usr/local/src
```


 解压 nginx 源码
```
tar -zxvf /usr/local/src/nginx-1.18.0.tar.gz -C /usr/local/src
```


解压完出现一个 nginx 文件夹

## 3.编译和安装nginx
#### 先进入cd /usr/local/src/nginx-1.18.0这个目录
注意：这个目录是安装包所在的目录，安装过程一直要在安装包所在目录里操作

```
cd /usr/local/src/nginx-1.18.0
```


	

#### 再创建一个mkdir /etc/nginx文件夹
 ```
 mkdir /etc/nginx
 ```

#### 调用nginx和openssl安装包
注意--with前面是的空格是空格键打的，不是tab键打的
```
./configure --prefix=/etc/nginx \
        --with-http_ssl_module \
        --with-http_gzip_static_module \
        --with-http_stub_status_module \
        --with-pcre \
        --with-http_realip_module \
        --with-http_flv_module \
        --with-http_mp4_module \
        --with-http_secure_link_module \
        --with-http_v2_module \
        --with-cc-opt='-O3' \
        --with-openssl=../openssl-1.1.1
```
注意 上面的/openssl-1.1.1是已经下载并解压好的openssl文件包，注意这个文件包名和之前下载解压好的oppenssl是否一致


注意复制进finalshell是这样的（不复制这个代码）
```
 ./configure --prefix=/etc/nginx \
>      --with-http_ssl_module \
>      --with-http_gzip_static_module \
>      --with-http_stub_status_module \
>      --with-pcre \
>      --with-http_realip_module \
>      --with-http_flv_module \
>      --with-http_mp4_module \
>      --with-http_secure_link_module \
>      --with-http_v2_module \
>      --with-cc-opt='-O3' \
>      --with-openssl=../openssl-1.1.1^C

```


## 编译&&安装nginx

注意还是在cd /usr/local/src/nginx-1.18.0文件夹里操作
#### 编译&&安装nginx
```
make && make install
```


#### 修改nginx.conf的基本配置
```
		sed -i 's/#user nobody;/user root;/' /etc/nginx/conf/nginx.conf
		sed -i 's/worker_processes 1;/worker_processes 3;/' /etc/nginx/conf/nginx.conf
		sed -i 's/ worker_connections 1024;/ worker_connections 4096;/' /etc/nginx/conf/nginx.conf
		sed -i '$i include conf.d/*.conf;' /etc/nginx/conf/nginx.conf
		sed -i '/listen 80;/a\ return 301 https://$http_host$request_uri;' /etc/nginx/conf/nginx.conf
```
		
		
		
		
###### 删除临时文件，也可以不删除，就不删除吧 免得错（这一步忽略）
```
		rm -rf /usr/local/src/nginx-1.18.0
		rm -rf /usr/local/src/nginx-1.18.0.tar.gz
		rm -rf /usr/local/src/openssl-1.1.1g
		rm -rf /usr/local/src/openssl-1.1.1g.tar.gz
```
 
 
#### 创建nginx的conf.d文件夹
 ```
mkdir /etc/nginx/conf/conf.d
```

 ![image.png](../assets/image_1630129036243_0.png)

#### 创建并修改conf.d/default.conf文件里的内容
手动修改法
###### 先创建default.conf 文件
```
cat >/etc/nginx/conf/conf.d/default.conf 
```
在/etc/nginx/conf/conf.d/文件夹里双击打开default.conf 文件

粘贴代码进default.conf 文件里
```
server {
				listen 6443自定义一个非的端口，可以设成6443;
				server_name 你的域名;
				root /usr/wwwroot;
				ssl on;
				ssl_certificate /证书所在的路径/fullchain.crt;
				ssl_certificate_key /证书所在的路径/证书钥匙名.key;
					ssl_ciphers TLS13-AES-256-GCM-SHA384:TLS13-CHACHA20-POLY1305-SHA256:TLS13-AES-128-GCM-SHA256:TLS13-AES-128-CCM-8-SHA256:TLS13-AES-128-CCM-SHA256:EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+ECDSA+AES128:EECDH+aRSA+AES128:RSA+AES128:EECDH+ECDSA+AES256:EECDH+aRSA+AES256:RSA+AES256:EECDH+ECDSA+3DES:EECDH+aRSA+3DES:RSA+3DES:!MD5;
				ssl_prefer_server_ciphers on;
				ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
				ssl_session_cache shared:SSL:50m;
				ssl_session_timeout 1d;
				ssl_session_tickets on;
			}
```

例如
```
server {
				listen 6443;
				server_name fsfsfs.eu.org;
				root /usr/wwwroot;
				ssl on;
				ssl_certificate /data/fsfsfs.eu.org/fullchain.crt;
				ssl_certificate_key /data/fsfsfs.eu.org/fsfsfs.eu.org.key;
					ssl_ciphers TLS13-AES-256-GCM-SHA384:TLS13-CHACHA20-POLY1305-SHA256:TLS13-AES-128-GCM-SHA256:TLS13-AES-128-CCM-8-SHA256:TLS13-AES-128-CCM-SHA256:EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+ECDSA+AES128:EECDH+aRSA+AES128:RSA+AES128:EECDH+ECDSA+AES256:EECDH+aRSA+AES256:RSA+AES256:EECDH+ECDSA+3DES:EECDH+aRSA+3DES:RSA+3DES:!MD5;
				ssl_prefer_server_ciphers on;
				ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
				ssl_session_cache shared:SSL:50m;
				ssl_session_timeout 1d;
				ssl_session_tickets on;
			}
```



自动修改法

注意：将下面代码中的端口号、域名和伪装网站目录修改成你自己的（注意;证书位置不对）
	
```
		cat >/etc/nginx/conf/conf.d/default.conf <<EOF
			server {
				listen 6443自定义一个非的端口，可以设成6443;
				server_name 你的域名;
				root /usr/wwwroot;
				ssl on;
				ssl_certificate /证书所在的路径/fullchain.crt;
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
 记下来我定义的端口比如6443（6443可以随便定义成其他数字只要不是443就行），这个是nginx的监听端口
 改好以后例如：

	
	
```
		cat >/etc/nginx/conf/conf.d/default.conf <<EOF
			server {
				listen 6443;
				server_name www.ribenjapan101.xyz;
				root /usr/wwwroot;
				ssl on;
				ssl_certificate /data/www.ribenjapan101.xyz/fullchain.crt;
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
	

## 4.创建并配置nginx.service文件
手动创建
```
cat >/etc/systemd/system/nginx.service
```

将下面的代码复制进/etc/systemd/system/文件里的nginx.service文件里

```
[Unit]  
Description=The NGINX HTTP and reverse proxy server  
After=syslog.target network.target remote-fs.target nss-lookup.target  
[Service]  
Type=forking  
PIDFile=/etc/nginx/logs/nginx.pid  
ExecStartPre=/etc/nginx/sbin/nginx -t  
ExecStart=/etc/nginx/sbin/nginx -c /etc/nginx/conf/nginx.conf  
ExecReload=/etc/nginx/sbin/nginx -s reload  
ExecStop=/bin/kill -s QUIT \$MAINPID  
PrivateTmp=true  
[Install]  
WantedBy=multi-user.target
```



或自动创建
创建服务文件
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
	


ctril + c退出编辑界面或者
退出finalshell 再进入


## 加载并启动nginx

#### 首先这个是启动nginx
```
systemctl daemon-reload
```



#### 然后重启一下nginx

```
systemctl restart nginx
```





#### 随时查看一下nginx状态
```
systemctl status nginx
```


然后按住ctril +c可以退出状态查看页


其他命令
启动
```
systemctl start nginx
```




停止
```
systemctl stop nginx
```



假如nginx启动失败怎么弄，出现Failed to start The NGINX HTTP and reverse proxy server怎么办

用
[nginx启动失败的拯救套路](nginx启动失败的拯救套路.md)
nginx启动失败 拯救套路





# 安装伪装网站
#### 创建伪装网站目录
```
mkdir /usr/wwwroot
```

#### 回到root根目录
```
cd /root
```


#### 下载安装伪装网站
```
git clone https://github.com/JeannieStudio/Programming.git /usr/wwwroot
```
如果出现fatal: Unable to read current working directory: No such file or directory
就说明这个目录已经删掉了
不如切换到root 目录下
```
cd /root
```



然后再重新执行
```
git clone https://github.com/JeannieStudio/Programming.git /usr/wwwroot
```
浏览器访问一下伪装网站（不粘贴近服务器）
复制到浏览器打开试试
```
https://我的域名:6443/
```
例如
```
https://fsfsfs.eu.org:6443/
```

域名：6443
6443是前面设置的nginx的监听端口
如果页面打不开





# 安装服务器上的v2ray
用官方快速安装脚本，地址：https://github.com/v2fly/fhs-install-v2ray
#### 安装cURL
如果是debian或ubuntu系统:
``` 
apt install curl
```
	 如果出现pt: command not found 就是pt命令没安装
```
apt install pt
```

如果是centOS系统：

```
yum makecache
```

0
```
yum install curl
```

##### 使用v2fly网站上的一键脚本下载并安装v2ray 安裝最新發行的 geoip.dat 和 geosite.dat

打开网页
https://github.com/v2fly/fhs-install-v2ray
从网页上复制的代码下来
安裝和更新 V2Ray


``` 
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
```

安裝最新發行的 geoip.dat 和 geosite.dat
```
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-dat-release.sh)
```

 
 
 如果安装失败可以试试 其他操作 移除 V2Ray

``` 
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh) --remove
```


#### 创建v2ray.service后台服务文件并修改
jennie的图文教程链接
https://jeanniestudio.top/2020/08/24/%E6%89%8B%E5%8A%A8%E6%90%AD%E5%BB%BAvless+tcp+tls/


手动创建
###### 创建v2ray.service后台服务文件

```
cat >/etc/systemd/system/v2ray.service
```

###### 手动打开v2ray.service文件并把下面粘贴进去

```
cd /etc/systemd/system/
```

```
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
```

或自动创建

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
配置v2ray

#### 生成uuid
```
cat /proc/sys/kernel/random/uuid 
```
把uuid记下来
或者不生成了，每次都用一样的，例如
```
f9fa4886-23dc-4e2b-9b78-23b8a3324236
```
	
#### 配置v2ray服务器：创建config. json文件并配置
下载官方给的配置即可：https://github.com/v2fly/v2ray-examples
找到VLESS-TCP-TLS (maximal by rprx)这个版本点击进去
这里官方给出了三个配置文件
因为nginx已经配置好了，我们点击打开config_server.json的配置
复制过来 修改红色标注的地方 删除中文注释 （注意前面设置的证书的路径是/data/example.com）

###### 创建config. json文件
在/usr/local/etc/v2ray/文件夹下创建config. json文件

```
cat >/usr/local/etc/v2ray/config. json
```


###### 手动打开修改config. json文件
```
cd /usr/local/etc/v2ray/
```

把下面代码粘贴进config. json文件注意修改uuid和证书路径
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
                        "id": "填写你的 UUID", 
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
                        "dest": "/dev/shm/h2.sock",  
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
                            "certificateFile": "/证书所在文件包的绝对路径/fullchain.crt",   
                            "keyFile": "/私钥所在文件包的绝对路径/以域名定义的文件名.key"   
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
例如
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
                        "id": "f9fa4886-23dc-4e2b-9b78-23b8a3324236",  
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
                        "dest": "/dev/shm/h2.sock",  
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
                            "certificateFile": "/data/fsfsfs.eu.org/fullchain.crt",  
                            "keyFile": "/data/fsfsfs.eu.org/fsfsfs.eu.org.key"   
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

注意刚才复制后证书的绝对路径在这里/data/fsfsfs.eu.org文件包里


或者自动生成和配置

```
cat >/usr/local/etc/v2ray/config.json <<EOF  
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
                        "dest": "/dev/shm/h2.sock",  
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
                            "certificateFile": "/path/to/fullchain.crt", // 换成你的证书，绝对路径  
                            "keyFile": "/path/to/example.com.key" // 换成你的私钥，绝对路径  
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
EOF
```

例如

```
cat >/usr/local/etc/v2ray/config.json <<EOF  
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
                        "dest": "/dev/shm/h2.sock",  
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
                            "certificateFile": "/path/to/fullchain.crt", // 换成你的证书，绝对路径  
                            "keyFile": "/path/to/example.com.key" // 换成你的私钥，绝对路径  
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
EOF
```


证书的绝对路径在这里

ctril c退出

如果你不放心可以看一下是否保存成功了
```
cat /usr/local/ect/v2ray/config. json

```


如果没成功说明 cat 命令
如果没成功就找到这个文件双击打开 cat /usr/local/ect/v2ray/config.json



把上面代码复制进去


## 启动vray
#### 启动 v2ray

```
systemctl daemon-reload
```


#### 重启一下 v2ray
```
systemctl restart v2ray
```



#### 随时查看下v2ray状态
```
systemctl status v2ray
```





	
其他命令

```
systemctl enable v2ray
```


其他相关命令（现在不用）：

启动v2ray命令是
```
systemctl start v2ray
```

重启v2ray命令是
```
systemctl start v2ray
```

停止v2ray命令是
 ```
 systemctl stop v2ray
 ```

下一步是
# [vless加上cdn的正确操作方法](vless加上cdn的正确操作方法.md)

下一步是
# windows的Qvray客户端下载和配置套路
- 1.win平台：
下载Qv2ray，使用手册：这里：https://qv2ray.net/getting-started/
#### 第一步：下载 Qv2ray软件
- https://github.com/Qv2ray/Qv2ray/releases
			- ![image.png](../assets/image_1630129242446_0.png)
			-
#### 第二步：下载 V2Ray 核心
- 拉到下面找到 手动下载 链接
			- https://hub.fastgit.org/v2fly/v2ray-core/releases
				- ![image.png](../assets/image_1630129248561_0.png)
				-
			- 下载好后解压
			- 解压后打开文件包
		- 注意按照他说的在本地Qv2ray\Qv2ray.v2.7.0-pre1.Windows-x64\deployment文件夹下新建文件夹
		- /config/vcore
		- 把上面的文件全部复制进去
#### 第三步：向 Qv2ray 中输入或导入节点

###### 手动输入节点法

- 第四步：让浏览器/软件走 Qv2ray 代理
		-
- （1）下载Qv2ray: https://github.com/Qv2ray/Qv2ray/releases
- windows用户下载：Qv2ray.v2.6.3.Windows-x64.7z，并解压即可。

标签 随便填
主机  填域名 例如：
```
fsfsfs.eu.org
```
端口
```
443
```
类型 选vless
uuid，例如刚才生成的：
```
f9fa4886-23dc-4e2b-9b78-23b8a3324236
```
协议设置 传输协议 选tcp
TLS设置 安全类型 选TLS




###### 安卓版手动输入节点法


别名 随便填
地址  填域名 例如：
```
fsfsfs.eu.org
```
端口
```
443
```
用户id就是填uuid，例如刚才生成的：
```
f9fa4886-23dc-4e2b-9b78-23b8a3324236
```
流控
加密  选none
底层传输方式
传输协议 选tcp
伪装类型  选none
伪装域名 例如：
```
fsfsfs.eu.org
```
path填
```
/path
```
底层传输安全 选tls 
跳过证书验证 选false

###### 导入法（现在不用这个方法了）
- 找到刚才的网页 官方给的配置：https://github.com/v2fly/v2ray-examples
			- 找到VLESS-TCP-TLS (maximal by rprx)这个版本点击进去
			- 这里官方给出了三个配置文件
			- config_client.json这个文件点进去
				- 复制代码到本地记事本 修改红色标注部分 然后保存 把文件名改成config.json,然后复制进QV2RAY根目录
					
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



# [长期不用时故障排除](长期不用时故障排除.md)


-
- [补充知识nginx和V2ray的端口原理](补充知识nginx和V2ray的端口原理.md)
	