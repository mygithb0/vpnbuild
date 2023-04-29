naive的原理和[trojan](https://bulianglin.com/g/aHR0cHM6Ly95b3V0dS5iZS9ndzJWbDFoODlXbw)很像，除去[保姆级教程](https://bulianglin.com/archives/nicename.html)搭建的节点消除了服务器tls指纹和隐藏了所有翻墙服务以及伪装成正常网站，naive协议更上一层，消除了客户端的tls指纹和tls-in-tls特征，并且naive协议基于http2，自带多路复用，对比ws需要频繁握手来讲延迟更低，前面也说过naive客户端使用chrome浏览器内核作为网络协议栈，从防火墙的角度来看，就像是你在正常的使用正常的谷歌浏览器访问正常的网站
关于tls指纹基础知识在文章末尾
搭建前域名已经做好了dns解析绑定到了当前VPS的ip地址

不良林的教程
https://bulianglin.com/archives/naive.html

[安装acme.sh依赖的脚本的套路](安装acme.sh依赖的脚本的套路.md)
# 1用[acmesh安装证书套路](acmesh安装证书套路.md)
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
# 5用go语言安装caddy
安装caddy的编译环境xcaddy
```
go install github.com/caddyserver/xcaddy/cmd/xcaddy@latest
```

编译和安装caddy
执行带nave插件的caddy
```
~/go/bin/xcaddy build --with github.com/caddyserver/forwardproxy@caddy2=github.com/klzgrad/forwardproxy@naive
```
完成后root根目录用ls命令可以在当前文件夹下看到一个名叫caddy的文件
```
ls
```

# 6 Caddyfile
在和caddy这个文件同一个目录下
一般是root根目录
右键新建一个叫Caddyfile的文件区分大小写
```
Caddyfile
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







# 7 caddy常用指令

前台运行caddy：
运行的时候会提示error无法获取到网站证书，不用管它会自动申请
```
./caddy run
```

[caddy启动问题汇总](caddy启动问题汇总.md)
运行成功后浏览器打开域名https://域名/
有403 Forbidden就说明caddy运行成功了

此时再Caddyfile里面再加上刚才删除的那两行



后台运行caddy：
```
./caddy start
```
停止caddy：
```
./caddy stop
```
重载配置：
```
./caddy reload
```
再再浏览器里输入域名看能不能跳转到伪装网站了
https://域名
例如 https://vpvpvp3.top/
能跳转就成功了

caddy配置守护进程（开机自启）：[https://github.com/klzgrad/naiveproxy/wiki/Run-Caddy-as-a-daemon](https://bulianglin.com/g/aHR0cHM6Ly9naXRodWIuY29tL2tsemdyYWQvbmFpdmVwcm94eS93aWtpL1J1bi1DYWRkeS1hcy1hLWRhZW1vbg)

### 自定义端口,新手跳过：

> naive如果要用自定义端口，需要使用json的配置方式，新手可以直接跳过

启动方式：
```
./caddy start --config config.json
```


**config.json内容：**

```
//需删除注释内容caddy才能加载
{
 "apps": {
   "http": {
     "servers": {
       "srv0": {
         "listen": [
           ":4431"   //监听端口
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
                 "naive.buliang0.tk"  //域名
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
           "certificate": "/root/a.crt",  //公钥路径
           "key": "/root/a.key",   //私钥路径
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

## naive客户端配置

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

### [TLS指纹查看和隐藏TLS指纹方法](TLS指纹查看和隐藏TLS指纹方法.md)





















```
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/xcaddy/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-xcaddy-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/xcaddy/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-xcaddy.list
sudo apt update
sudo apt install xcaddy
```






如果出现
can't load package: package github.com/caddyserver/xcaddy/cmd/xcaddy@latest: cannot use path@version syntax in GOPATH mode


package github.com/caddyserver/xcaddy/cmd/xcaddy@latest: cannot use path@version syntax in GOPATH mode

解决方法在
https://blog.csdn.net/Julycaka/article/details/106105988


意思是无法加载软件包：软件包github.com/caddyserver/xcaddy/cmd/xcaddy@latest：不能在gopath模式下使用path@path版本语法


安装不成功就用这个命令安装go语言环境然后再重复上述步骤
```
curl -sL https://gitee.com/op2dev/opscript/raw/v.0.0.2/golang/install_golang.sh  | bash -
source ~/.bash_profile
```


如果下载编译caddy所需要的文件这个步骤
go install github.com/caddyserver/xcaddy/cmd/xcaddy@latest
没反应的话

就用
https://www.jianshu.com/p/9909c00cf1b2



编译带naive插件的caddy

etc/root/gopath

```
~/gopath/bin/xcaddy build --with github.com/caddyserver/forwardproxy@caddy
```


列一下看看有没有文件了
```
ls
```


如果第二条指令执行出错，可以尝试执行`go env -w GO111MODULE=on` 再重试，还不行的话请自行搜索升级go版本方法

在caddy的同目录/root下新建一个叫Caddyfile的文件首字母大写

```
Caddyfile
```
创建完成后回到文档复制配置信息进去

### Caddyfile配置：

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
(如果需要多个用户 user pass 这行复制就可以了)


例如


```
:443, fsfsfs.eu.org
tls dxdll10001@hotmail.com
route {
 forward_proxy {
   basic_auth dxdll10001 dxdll12345
   hide_ip
   hide_via
   probe_resistance
  }
 #支持多用户
 forward_proxy {
   basic_auth dxdll10002 dxdll12345 #用户名和密码
   hide_ip
   hide_via
   probe_resistance
  }
 reverse_proxy  https://www.bing.com/  { 
   header_up  Host  {upstream_hostport}
   header_up  X-Forwarded-Host  {host}
  }
}
```


certificate success就是证书申请成功了

# 使用 systemd 启动 caddy 服务

```
systemctl daemon-reload
```

```
systemctl enable caddy
```

```
systemctl start caddy
```


# 查看当前状态

```
systemctl status caddy
```

### caddy常用指令：

启动前台运行caddy启动服务端：

```
./caddy run
```

启动后台运行caddy：
```
./caddy start
```

其他命令
停止caddy：
```
./caddy stop
```


其他命令
重载配置：
```
./caddy reload
```

caddy配置守护进程（开机自启）：[https://github.com/klzgrad/naiveproxy/wiki/Run-Caddy-as-a-daemon](https://bulianglin.com/g/aHR0cHM6Ly9naXRodWIuY29tL2tsemdyYWQvbmFpdmVwcm94eS93aWtpL1J1bi1DYWRkeS1hcy1hLWRhZW1vbg)

### 自定义端口：

> naive如果要用自定义端口，需要使用json的配置方式，新手可以直接跳过

启动方式：
```
./caddy start --config config.json
```


**config.json内容：**

```
//需删除注释内容caddy才能加载
{
 "apps": {
   "http": {
     "servers": {
       "srv0": {
         "listen": [
           ":4431"   //监听端口
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
                 "naive.buliang0.tk"  //域名
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
           "certificate": "/root/a.crt",  //公钥路径
           "key": "/root/a.key",   //私钥路径
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

## 客户端配置

naive内核下载：
https://github.com/klzgrad/naiveproxy/releases/latest

下载windows 64的内核

把内核放入v2rayN

客户端配置：
找到刚才下载的文件里面有一个config.json文件打开
```
{
  "listen": "socks://127.0.0.1:1080",
  "proxy": "https://user:替换成我的域名"
}
```

使用v2rayN加载naive内核需要将配置文件的log行删除，否则会断流
#### 将域名信息替换进去例如
```
{
  "listen": "socks://127.0.0.1:1080",
  "proxy": "https://user:vpvpvp3.top"
}
```
打开windows的v2rayN软件
新建》添加自定义配置服务》
别名随便写
地址浏览 导入刚才修改过的windows版的config.json文件
core类型naiveproxy
关闭显示日志


## TLS指纹查看

jarm工具：[https://github.com/salesforce/jarm](https://bulianglin.com/g/aHR0cHM6Ly9naXRodWIuY29tL3NhbGVzZm9yY2UvamFybQ)

下载jarm：`wget https://raw.githubusercontent.com/salesforce/jarm/master/jarm.py`  
查看网站jarm指纹：`python3 jarm.py naive.buliang0.tk`

