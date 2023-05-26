naive的原理和[trojan](https://bulianglin.com/g/aHR0cHM6Ly95b3V0dS5iZS9ndzJWbDFoODlXbw)很像，除去[保姆级教程](https://bulianglin.com/archives/nicename.html)搭建的节点消除了服务器tls指纹和隐藏了所有翻墙服务以及伪装成正常网站，naive协议更上一层，消除了客户端的tls指纹和tls-in-tls特征，并且naive协议基于http2，自带多路复用，对比ws需要频繁握手来讲延迟更低，前面也说过naive客户端使用chrome浏览器内核作为网络协议栈，从防火墙的角度来看，就像是你在正常的使用正常的谷歌浏览器访问正常的网站
关于tls指纹基础知识在文章末尾
搭建前域名已经做好了dns解析绑定到了当前VPS的ip地址

不良林的教程
https://bulianglin.com/archives/naive.html

[安装acme.sh依赖的脚本的套路](安装acme.sh依赖的脚本的套路.md)
# 1用[acme.sh安装证书套路](4logseq/4logseq-/acme.sh安装证书套路.md)
# 1[复制转换证书套路](复制转换证书套路.md)

## 2先切换到 root
```
sudo -i
```

### 编译安装caddy+naive：
安装go语言编译环境
# 3[手动下载安装go语言](手动下载安装go语言.md)
或者[自动安装go语言方法](自动安装go语言方法.md)

## 4通过查看go语言版本看go语言是否安装好了
```
go version
```
如果提示出现
go version go版本号 linux/amd64
就成功了
## 5编译安装caddy+naive：
# 5用go语言安装NaïveProxy＋Caddy
NaïveProxy是caddy的一个插件，这个插件替代了Caddy里原有的的forwardproxy所以实现了代理的功能
```
需要安装NaïveProxy，且不是单独安装Caddy，务必按照命令执行。
NaiveProxy项目主页：https://github.com/klzgrad/forwardproxy
项目主页：https://github.com/caddyserver/caddy
Caddy官方文档：https://caddyserver.com/docs/
Caddy这个工具，配置起来有点反直觉，它为了实现无配置的傻瓜式上手可用，发明了一套奇怪的使用逻辑，类似docker-compose的Caddyfile配置启动参数，caddy会在启动时帮你完成配置
```

#### 安装caddy的编译环境xcaddy
```
go install github.com/caddyserver/xcaddy/cmd/xcaddy@latest
```
#### 安装带nave插件的caddy
```
~/go/bin/xcaddy build --with github.com/caddyserver/forwardproxy@caddy2=github.com/klzgrad/forwardproxy@naive
```
完成后root根目录用ls命令可以在当前文件夹下看到一个名叫caddy的文件
```
ls
```
#### 将caddy的启动文件复制到/usr/bin/文件夹
```
cp caddy /usr/bin/
```

###### 通过检查一下caddy版本检测一下caddy是否安装上了
```
/usr/bin/caddy version
```
如果出现版本号例如v2.6.4 h1就成功了

###### 设置bind权限，443可打开
```
setcap cap_net_bind_service=+ep /usr/bin/caddy
```

# 6 新建一个Caddyfile文件并配置


通常大家都用的IPv4地址，故而，这里需要配置一个域名的A记录，指向你的服务器公网IPv4地址，且需要一套可信的证书文件，不要用自签的证书。

###### 创建一个存放证书的目录
```
mkdir -p /etc/ssl/caddy
```
###### 创建一个存放Caddyfile的目录/etc/caddy/
```
mkdir /etc/caddy/
```

```
cd /etc/caddy/
```
###### 再/etc/caddy/路径下创建一个叫Caddyfile的文件
```
vim Caddyfile
```
###### 在同一个文件夹/etc/caddy/下新建Caddyfile--debian文件

```
# The Caddyfile is an easy way to configure your Caddy web server.
#
# Unless the file starts with a global options block, the first
# uncommented line is always the address of your site.
#
# To use your own domain name (with automatic HTTPS), first make
# sure your domain's A/AAAA DNS records are properly pointed to
# this machine's public IP, then replace ":80" below with your
# domain name.

:80 {
	# Set this path to your site's directory.
	root * /usr/share/caddy

	# Enable the static file server.
	file_server

	# Another common task is to set up a reverse proxy:
	# reverse_proxy localhost:8080

	# Or serve a PHP site through php-fpm:
	# php_fastcgi localhost:9000
}

# Refer to the Caddy docs for more information:
# https://caddyserver.com/docs/caddyfile

```


下载编译caddy所需要的文件复制粘贴进Caddyfile文件里
https://github.com/caddyserver
### 6.1复制进服务端的Caddyfile的配置信息：
假如伪装网址是https://demo.cloudreve.org
```
:443, naive.buliang0.tk #你的域名
tls example@example.com #你的邮箱
route {
 forward_proxy {
   basic_auth user pass #用户名和密码
   hide_ip
   hide_via
   probe_resistance
  }
 #支持多用户
 forward_proxy {
   basic_auth user2 pass2 #用户名和密码
   hide_ip
   hide_via
   probe_resistance
  }
 reverse_proxy  https://demo.cloudreve.org  { #伪装网址
   header_up  Host  {upstream_hostport}
   header_up  X-Forwarded-Host  {host}
  }
}
```
或者只用单用户
```
:443, naive.buliang0.tk #你的域名
tls example@example.com #你的邮箱
route {
 forward_proxy {
   basic_auth user pass #用户名和密码
   hide_ip
   hide_via
   probe_resistance
  }
 reverse_proxy  https://demo.cloudreve.org  { #伪装网址
   header_up  Host  {upstream_hostport}
   header_up  X-Forwarded-Host  {host}
  }
}
```
###### 6.1填好以后例如
填好以后并且先删除这两行，等caddy运行成功以后再加上
header_up  Host  {upstream_hostport}
header_up  X-Forwarded-Host  {host}
等caddy运行成功以后再加上
例如
```
:443, vpvpvp3.top
tls dxdll10001@hotmail.com 
route {
 forward_proxy {
   basic_auth dxdll10001 dxdll12345
   hide_ip
   hide_via
   probe_resistance
  }
 reverse_proxy  https://demo.cloudreve.org  
}
```


###### 启动caddy试试第1次
```
# 启动
caddy run --config "/etc/caddy/Caddyfile"
```
出现很多INFO之类的提示就说明成功了
目前来说，国内的网络环境有Qos限流，会导致传输大文件比如观看youtube时发生quic断流，如果你那边经常发生quic断流的情况，就是用https而不是quic即可


###### 在/etc/systemd/system/目录下
###### 新建naive.service服务文件并配置

```
# /etc/systemd/system/naive.service
[Unit]
Description=Caddy
Documentation=https://caddyserver.com/docs/
After=network.target network-online.target
Requires=network-online.target

[Service]
Type=notify
User=root
Group=root
ExecStart=/usr/bin/caddy run --environ --config /etc/caddy/Caddyfile
ExecReload=/usr/bin/caddy reload --config /etc/caddy/Caddyfile
TimeoutStopSec=5s
LimitNOFILE=1048576
LimitNPROC=512
PrivateTmp=true
ProtectSystem=full
AmbientCapabilities=CAP_NET_BIND_SERVICE

[Install]
WantedBy=multi-user.target
```



#### 6.2设置caddy开机自启

###### 使 Caddy 可执行并将 caddy 二进制文件移动到/usr/bin/
```
chmod +x caddy
mv caddy /usr/bin/
```


###### 启动caddy试试第2次
```
/usr/bin/caddy run --config /etc/caddy/Caddyfile
```


##### 为 caddy 创建唯一的 Linux 组和用户

```
groupadd --system caddy
```


```
useradd --system \
--gid caddy \
--create-home \
--home-dir /var/lib/caddy \
--shell /usr/sbin/nologin \
--comment "Caddy web server" \
caddy
```

注意输入进去以后是这样

```
useradd --system \
> --gid caddy \
> --create-home \
> --home-dir /var/lib/caddy \
> --shell /usr/sbin/nologin \
> --comment "Caddy web server" \
> caddy
```





# 7 caddy常用指令

前台运行caddy：
运行的时候会提示error无法获取到网站证书，不用管它会自动申请
###### 启动caddy试试第3次
```
# 启动
caddy run --config "/etc/caddy/Caddyfile"
```



[caddy启动问题汇总](caddy启动问题汇总.md)
运行成功后浏览器打开域名https://域名/
有403 Forbidden就说明caddy运行成功了

此时再Caddyfile里面再加上刚才删除的那两行



后台运行caddy：
```
/usr/bin/caddy start
```
停止caddy：
```
/usr/bin/caddy stop
```
重载配置：
```
/usr/bin/caddy reload
```
再再浏览器里输入域名看能不能跳转到伪装网站了
https://域名
例如 https://vpvpvp3.top/
能跳转就成功了

其他caddy的启动命令
# 启动caddy试试第4次
### 使用 systemd 启动 caddy 服务

```
systemctl daemon-reload
```

```
systemctl enable caddy
```

```
systemctl start caddy
```


### 查看当前状态
```
systemctl status caddy
```



```
systemctl stop caddy
```



caddy配置守护进程（开机自启）：[https://github.com/klzgrad/naiveproxy/wiki/Run-Caddy-as-a-daemon](https://bulianglin.com/g/aHR0cHM6Ly9naXRodWIuY29tL2tsemdyYWQvbmFpdmVwcm94eS93aWtpL1J1bi1DYWRkeS1hcy1hLWRhZW1vbg)

### 自定义端口

naive如果要用自定义端口，需要使用json的配置方式
###### 在/etc/caddy/文件夹下
###### 新建caddy_server.json文件
即

```
cd /etc/caddy/
```

/etc/caddy/caddy_server.json

**config.json内容：**

```
#  /etc/caddy/caddy_server.json
//需删除注释内容caddy才能加载
{
 "apps": {
   "http": {
     "servers": {
       "srv0": {
         "listen": [
           ":443"   //监听端口
         ],
         "routes": [
           {
             "handle": [
               {
                 "auth_user_deprecated": "user",   //用户名
                 "auth_pass_deprecated": "pass",  //密码
                 "handler": "forward_proxy",
                 "hide_ip": true,
                 "hide_via": true,
                 "probe_resistance": {}
               }
             ]
           },
           {
             "handle": [
               {
                 "handler": "reverse_proxy",
                 "headers": {
                   "request": {
                     "set": {
                       "Host": [
                         "{http.reverse_proxy.upstream.hostport}"
                       ],
                       "X-Forwarded-Host": [
                         "{http.request.host}"
                       ]
                     }
                   }
                 },
                 "transport": {
                   "protocol": "http",
                   "tls": {}
                 },
                 "upstreams": [
                   {
                     "dial": "demo.cloudreve.org:443"  //伪装网址
                   }
                 ]
               }
             ]
           }
         ],
         "tls_connection_policies": [
           {
             "match": {
               "sni": [
                 "vpvpvp3.top"  //域名
               ]
             },
             "certificate_selection": {
               "any_tag": [
                 "cert0"
               ]
             }
           }
         ],
         "automatic_https": {
           "disable": true
         }
       }
     }
   },
   "tls": {
     "certificates": {
       "load_files": [
         {
           "certificate": "/etc/ssl/caddy/fullchain.crt",  //公钥路径
           "key": "/etc/ssl/caddy/vpvpvp3.top.key",   //私钥路径
           "tags": [
             "cert0"
           ]
         }
       ]
     }
   }
 }
}
```

运行服务端

```
/usr/bin/caddy run --config /etc/caddy/caddy_server.json
```
[config.json运行失败问题汇总](config.json运行失败问题汇总.md)
## naive客户端配置
先试手机端
新建http2
填入地址端口用户名密码 
可以上网就说明节点搭建成功了

然后用电脑连接手机端的热点


下载wendows版的naive客户端的内核文件到本地
[https://github.com/klzgrad/naiveproxy/releases/latest](https://bulianglin.com/g/aHR0cHM6Ly9naXRodWIuY29tL2tsemdyYWQvbmFpdmVwcm94eS9yZWxlYXNlcy9sYXRlc3Q)
解压放放到v2rayN的安装目录
主要是把naive.exe放到v2rayN的安装目录

客户端配置：

```
{
  "listen": "socks://127.0.0.1:1080",
  "proxy": "https://user:pass@域名"
}
```
#### 将节点的域名替换掉例如
```
{
  "listen": "socks://127.0.0.1:1080",
  "proxy": "https://user:pass@vpvpvp3.top"
}
```


使用v2rayN加载naive内核需要将配置文件的log行删除，否则会断流
[naive手机端可以连电脑端连不上问题](naive手机端可以连电脑端连不上问题.md)

### [TLS指纹查看和隐藏TLS指纹方法](TLS指纹查看和隐藏TLS指纹方法.md)








