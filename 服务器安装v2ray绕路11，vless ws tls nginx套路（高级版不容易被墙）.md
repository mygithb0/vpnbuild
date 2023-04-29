
# [安装acme.sh脚本](安装acme.sh脚本)

# 用[acme.sh安装证书套路](4logseq/4logseq-/vpn/acme.sh安装证书套路.md)

# 用[复制转换证书套路](复制转换证书套路.md)

# [安装nginx套路](安装nginx套路.md)


# 修改nginx.conf的基本配置
注意nginx.conf文件在
## /etc/nginx/conf

```
cd /etc/nginx/conf
```
注意打开路径下所有文件包的777权限

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


 ```
cd /etc/nginx/conf/conf.d
```

 

# 创建并修改conf.d/default.conf文件里的内容
手动修改法
###### 先创建default.conf 文件
```
cat >/etc/nginx/conf/conf.d/default.conf 
```



在
## /etc/nginx/conf/conf.d/
文件夹里双击打开default.conf 文件

```
cd /etc/nginx/conf/conf.d/
```


粘贴代码进default.conf 文件里

注意后面先试试直接连接V2RAY
再试试间接连接80端口进nginx
最后再试试443
以未来将nginx安装在8443端口为例
凡是  地址栏里的地址 加上后缀 /ray的 都从443端口的nginx上转发到 8443端口的v2ray

### 注意装好nginx以后 先连接测试8443端口上的v2ray回过头来再调试从nginx转发流量的问题


如果域名是vpvpvp.eu.org并且v2ray的端口设置在8443并且2v2ray的路径装在/ray/并且伪装网站路径安装在/usr/wwwroot的话就

```
server {
listen 80;
				listen 443 ssl;
				server_name vpvpvp.top;
				root /usr/wwwroot;
				ssl on;
				ssl_certificate /data/vpvpvp.top/fullchain.crt;
				ssl_certificate_key /data/vpvpvp.top/vpvpvp.top.key;
					ssl_ciphers TLS13-AES-256-GCM-SHA384:TLS13-CHACHA20-POLY1305-SHA256:TLS13-AES-128-GCM-SHA256:TLS13-AES-128-CCM-8-SHA256:TLS13-AES-128-CCM-SHA256:EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+ECDSA+AES128:EECDH+aRSA+AES128:RSA+AES128:EECDH+ECDSA+AES256:EECDH+aRSA+AES256:RSA+AES256:EECDH+ECDSA+3DES:EECDH+aRSA+3DES:RSA+3DES:!MD5;
				ssl_prefer_server_ciphers on;
				ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
				ssl_session_cache shared:SSL:50m;
				ssl_session_timeout 1d;
				ssl_session_tickets on;


 location /ray/ {   
            #proxy_redirect off;
            proxy_pass http://127.0.0.1:8443/; 
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            #proxy_set_header Host $host;
            #proxy_set_header X-Real-IP $remote_addr;
            #proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                      }


			}

```






# 4.创建并配置nginx.service文件
手动创建
```
cat >/etc/systemd/system/nginx.service
```

```
cd /etc/systemd/system/
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


- ctril + c退出编辑界面或者
- 退出finalshell 再进入


# 加载并启动nginx

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
附
[nginx启动失败的拯救套路](nginx启动失败的拯救套路.md)

因为前面设置了vpvpvp.top:443/是nginx所在的端口，为了检车这个端口是否能访问
浏览器打开
https://vpvpvp.top:443/
不行就先打开这个域名绑定的IP地址 
这些都试试
https://92.118.151.196:443/
https://vpvpvp.top:443/
http://92.118.151.196:443/
http://vpvpvp.top:443/


# [安装伪装网站（可以不安装）](安装伪装网站（可以不安装）)


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

```
yum install curl
```
##### 使用v2fly网站上的一键脚本下载并安装v2ray 安裝最新發行的 geoip.dat 和 geosite.dat

- 打开网页
		- https://github.com/v2fly/fhs-install-v2ray
	- 从网页上复制的代码下来
	- 安裝和更新 V2Ray
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


# 创建v2ray.service后台服务文件并修改
手动创建
###### 创建v2ray.service后台服务文件

```
cat >/etc/systemd/system/v2ray.service
```

###### 手动打开v2ray.service文件并把下面粘贴进去

## cd /etc/systemd/system/

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


#### 生成uuid（或者不生成了，每次都用一样的）见下面
```
cat /proc/sys/kernel/random/uuid 
```
- 把uuid记下来
#### 或者不生成了，每次都用一样的，例如
# UUID
```
548ed45a-5538-470d-9666-14784d18031b
```
	
#### 配置v2ray服务器：创建config. json文件并配置
- 下载官方给的配置即可：https://github.com/v2fly/v2ray-examples
	- 找到VLESS-TCP-TLS (maximal by rprx)这个版本点击进去
	- 这里官方给出了三个配置文件
	- 因为nginx已经配置好了，我们点击打开config_server.json的配置
		- 复制过来 修改红色标注的地方 删除中文注释 （注意前面设置的证书的路径是/data/example.com）

###### 创建config. json文件
在/usr/local/etc/v2ray/文件夹下创建config. json文件

```
cat >/usr/local/etc/v2ray/config. json
```


###### 手动打开修改config. json文件

# config. json(v2ray)

```
cd /usr/local/etc/v2ray/
```

#### 把下面代码粘贴进config. json文件注意修改uuid和证书路径

假如要搭建成vmess ws tls的

从v2ray的github官网上到v2ray的配置文件
注意找到vmess  websocket tls 的这一个config.json文件

[v2fly/v2ray-examples: v2ray-core 的模板们 (github.com)](https://github.com/v2fly/v2ray-examples)

并且
协议vmess
监听IP是 0.0.0.0 (就是全部都监听)    或者  127.0.0.1
端口设置成8443
传输改成ws
id复制一下例如

548ed45a-5538-470d-9666-14784d18031b

security先设置成zero例如

#### 第一次先将tls设置改为zero关闭，假如v2ray想要安在的端口是8443，监听所有端口（0.0.0.0），而不是只监听本机（127.0.0.1这样会出现域名+8443这个网址用浏览器大不开），就把uuid和证书路径都填进去，假如证书路径在vpvpvp.top例如

```
{
    "log": {
        "loglevel": "warning"
    },
    "routing": {
        "domainStrategy": "AsIs",
        "rules": [
            {
                "type": "field",
                "ip": [
                    "geoip:private"
                ],
                "outboundTag": "block"
            }
        ]
    },
    "inbounds": [
        {
            "listen": "0.0.0.0",
            "port": 8443,
            "protocol": "vmess",
            "settings": {
                "clients": [
                    {
                        "id": "548ed45a-5538-470d-9666-14784d18031b"
                    }
                ]
            },
            "streamSettings": {
                "network": "ws",
                "security": "zero",
                "tlsSettings": {
                    "certificates": [
                        {
                            "certificateFile": "/data/vpvpvp.top/fullchain.crt",
                            "keyFile": "/data/vpvpvp.top/vpvpvp.top.key"
                        }
                    ]
                }
            }
        }
    ],
    "outbounds": [
        {
            "protocol": "freedom",
            "tag": "direct"
        },
        {
            "protocol": "blackhole",
            "tag": "block"
        }
    ]
}
```


uuid可以每次都用一样的 不用每次生成
```
548ed45a-5538-470d-9666-14784d18031b
```


注意刚才复制后证书的绝对路径在这里/data/vpvpvp.top文件包里



ctril c退出



- 如果你不放心可以看一下是否保存成功了
```
cat /usr/local/ect/v2ray/config. json

```


 - 如果没成功说明 cat 命令
- 如果没成功就找到这个文件双击打开 cat /usr/local/ect/v2ray/config.json



- 把上面代码复制进去



# 启动vray
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


上面代码复制好以后并且 启动v2ray以后,v2ray运行没问题以后

如果浏览器打开vpvpvp.top，查看会跳转到nginx网站了，就说明nginx安装成功了，端口打开了，有时候虽然nginx的active状态还是红色，但是不影响

如果打开vpvpvp.top/ray网站，如果返回bad request页面就说明/ray路径绑定的前面设置的v2ray路径所在的8443端口连接到v2ray了

如果出现502页面，则说明8443端口不通。也就是v2ray连接不成功

每次修改nginx配置文件后必须使用 systemctl reload nginx 命令重新加载配置文件


## [检测连通性，分别用国内外端口检测工具检测](检测连通性，分别用国内外端口检测工具检测.md)

站长工具端口扫描

http://tool.chinaz.com/port/
从国外端口扫描

https://www.yougetsignal.com/tools/open-ports/


## [检测连通性用搭好的IP地址和域名输入浏览器检测](检测连通性用搭好的IP地址和域名输入浏览器检测.md)

域名+端口 打不开  IP+端口可以



检查nginx的端口域名和IP是否都打得开

网页http://193.32.151.147:3443/ 
网页https://193.32.151.147:3443/ 
可以打开但是

http://vpvpvp.eu.org:3443/
https://vpvpvp.eu.org:3443/

打不开就是域名被污染的问题
用手机试试

检查v2ray的端口域名和IP是否都打得开


网页http://193.32.151.147:6443/ 
网页https://193.32.151.147:6443/ 
可以打开但是

http://vpvpvp.eu.org:6443/
https://vpvpvp.eu.org:6443/

打不开就还是域名被污染的问题




## [检测连通性用cmd里的telnet命令检测](检测连通性用cmd里的telnet命令检测.md)

检查6443端口打开了没

telnet 45.207.45.171 6443
telnet 193.32.151.147 6443

检查3443端口打开了没

telnet 45.207.45.171 4443
telnet 193.32.151.147 3443

附
[查看端口对应的程序](查看端口对应的程序)



# windows的v2ray客户端下载和配置套路

#### 第一次先填IP地址不填域名，看能不能连通

标签 随便填
端口先试8443 直接连接v2ray      
主机  填域名 例如：
```
45.207.45.171
```


TLS设置 无
路径 无
传输协议 ws
加密方式 zero

类型 选vmess
uuid，例如刚才生成的：
```
548ed45a-5538-470d-9666-14784d18031b
```



#### 可以连通的话第二次


标签 随便填
主机  填域名 例如：
```
vpvpvp.top
```
  
成功后再试80端口连接nginx  转发到v2ray的这条路径
成功后再试443端口连接nginx  转发到v2ray的这条路径

```
443
```

TLS设置 安全类型 选TLS
流控
加密  选zero 或者none
底层传输方式
传输协议 选tcp
伪装类型  选none
伪装域名 例如：

```
vpvpvp.top
```
path路径填前面设定的/ray
```
/ray
```
底层传输安全 选tls 
跳过证书验证 选false

