https://www.youtube.com/watch?v=s90feRmdr9A&t=427s

https://bulianglin.com/archives/nicename.html



# 一、安装acme.sh脚本

## 

1. 先切换到 root

```
sudo -i
```

-   如果出现错误sudo：command not found
-   就说明没有这个命令，那么就安装sudo命令
    -   [https://blog.csdn.net/hello_1995/article/details/109222650](https://blog.csdn.net/hello_1995/article/details/109222650)

## 

2.安装依赖，

如果是debian或ubuntu系统则执行：

```
apt-get update && apt-get -y install socat

```

如果是centos系统则执行：

```
yum update && yum -y install socat
```

-   用安装acme脚本申请证书套路








# 用[acme.sh安装证书套路](4logseq/4logseq-/vpn/acme.sh安装证书套路.md)

# [复制转换证书套路](4logseq/4logseq-/复制转换证书套路.md)


## 节点搭建

# 更新软件源

```
apt update
```
### 启用 BBR TCP 拥塞控制算法（暂时不用这个步骤避免出错）

```
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
```



# 安装x-ui：

```
bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh)
```

如果出现curl: command not found
ubuntu/debian 系统安装 Curl 方法: 

```
apt-get update -y && apt-get install curl -y
```

是否设置账号
y
账号密码
dxdll10001
dxdll12345
设置用户访问面板端口
9999

设置xui开机自启

```
x-ui enable 
```



### 复制安装证书到另一个路径方法2（暂时不用）：

```
acme.sh --install-cert -d 你的域名 --ecc --key-file       /etc/x-ui/server.key  --fullchain-file /etc/x-ui/server.crt --reloadcmd     "systemctl force-reload nginx"
```
例如
```
acme.sh --install-cert -d fsfsfs.eu.org --ecc --key-file       /etc/x-ui/server.key  --fullchain-file /etc/x-ui/server.crt --reloadcmd   
```


```
acme.sh --installcert -d fsfsfs.eu.org --fullchainpath /etc/x-ui/fullchain.crt --keypath /etc/x-ui/fsfsfs.eu.org.key --ecc --force
```



出现绿字Reload success说明成功


#### 创建一个路径“/data/以你的域名命名的文件包名”用于存放复制后的证书



```
mkdir /etc/x-ui
```


#### 

转换证书到另一个要复制到的文件夹

```
acme.sh --installcert -d 域名 --fullchainpath /域名要复制进的路径/以域名命名的文件包名/fullchain.crt --keypath /域名要复制进的路径/以域名命名的文件包名/以域名命名的文件名.key --ecc --force
```

例如

```
acme.sh --installcert -d fsfsfs.eu.org --fullchainpath /data/fsfsfs.eu.org/fullchain.crt --keypath /data/fsfsfs.eu.org/fsfsfs.eu.org.key --ecc --force
```

如果出现以下提示就成功了  
[Wed 25 May 2022 03:39:51 AM UTC] Installing key to: /data/fsfsfs.eu.org/fsfsfs.eu.org.key  
[Wed 25 May 2022 03:39:51 AM UTC] Installing full chain to: /data/fsfsfs.eu.org/fullchain.crt

进入这个文件包看一下

```
cd /data/你的域名
```

例如

```
cd /data/fsfsfs.eu.org
```

```
ls
```

打开所有文件和文件包的777权限  
包括这些文件  

/etc/x-ui/fsfsfs.eu.org.key  
/etc/x-ui/fullchain.crt









# 安装nginx

```
apt install nginx
```

## 寻找适合的伪装站

http站点优先，推荐个人网盘符合单节点大流量特征

例如

cloudreve.fengbohan.com

这个是通过谷歌搜索示例关键字：intext:登录 Cloudreve
搜到的

#  配置nginx

配置文件路径：
```
cd /etc/nginx/
```
找到nginx.conf文件双击打开
```
/etc/nginx/nginx.conf
```

配置文件内容：
注意替换自己的域名，证书的位置不用动，替换伪装网站的域名三处,节点的分流信息和x-ui面板的路径信息

### 浏览器里设置好xray和xui端口并在nginx配置文件里保持一致,然后复制修改好的完全替换进去
如何打开x-ui面板 并将x-ui面板 打开的端口设置为9999
浏览器输入
```
我的域名:9999
```
例如
```
fsfsfs.eu.org:9999
```

注意54321是之前设的x-ui面板端口 这里设置为9999
输入之前设置的账号密码进行登录
dxdll10001
dxdll12345
然后将xray的版本设置为最新版:
系统状态》xray状态》切换版本

入站列表》+添加一个节点
备注随便写 suibianxie
协议vless
监听IP是127.0.0.1
端口设置成10000
传输改成ws
id复制一下例如
548ed45a-5538-470d-9666-14784d18031b
1bcd33d6-7b22-4edf-9cec-fdd41c51f578
然后粘贴到下面的路径里例如
/548ed45a-5538-470d-9666-14784d18031b
回到nginx配置文件的最后两处替换，一处是替换id将location /ray处替换成location /548ed45a-5538-470d-9666-14784d18031b

另一处是让端口保持一致:
也就是确保127.0.0.1:冒号后面的端口和节点监听端口是10000

面板设置》面板监听ip改成127.0.0.1
面板监听端口保持不变54321
面板url根路径复制刚才的id进去后面再加上-xui字段
例如
/548ed45a-5538-470d-9666-14784d18031b-xui
保存配置
重启面板（然后面板就再也进不去了）

回到刚才nginx配置找到location /xui处将刚才填写的x-ui面板路径写进去后面再加上-xui字段
例如
location /548ed45a-5538-470d-9666-14784d18031b-xui
找到后面127.0.0.1的xui监听端口是127.0.0.1:54321



```
cd /etc/nginx/
```


例如

```
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
    worker_connections 1024;
       }

http {
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;

    default_type application/octet-stream;
    gzip on;

    server {
        listen 443 ssl;
        
        server_name fsfsfs.eu.org;  
        
        ssl_certificate   /data/fsfsfs.eu.org/fullchain.crt;  
        ssl_certificate_key  /data/fsfsfs.eu.org/fsfsfs.eu.org.key; 
        
        ssl_session_timeout 1d;
        ssl_session_cache shared:MozSSL:10m;
        ssl_session_tickets off;
        ssl_protocols    TLSv1.2 TLSv1.3;
        ssl_prefer_server_ciphers off;

        location / {
            proxy_pass http://bing.com/; 
            proxy_redirect off;
            proxy_ssl_server_name on;
           
	
            proxy_set_header Host "bing.com";
            proxy_set_header Referer $http_referer;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header User-Agent $http_user_agent;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto https;
            proxy_set_header Accept-Encoding "";
            proxy_set_header Accept-Language "zh-CN";
                   }


        location /ray {   
            proxy_redirect off;
            proxy_pass http://127.0.0.1:10000; 
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                      }
        
        location /xui {   
            proxy_redirect off;
            proxy_pass http://127.0.0.1:9999;  
            proxy_http_version 1.1;
            proxy_set_header Host $host;
                       }
           }

    server {
        listen 80;
        location /.well-known/ {
               root /var/www/html;
                               }
        location / {
                rewrite ^(.*)$ https://$host$1 permanent;
                   }
            }
      }
```

浏览器打开fsfsfs.eu.org查看会不会跳转到bing网站了，会跳转说明nginx就成功了，虽然nginx的active状态还是红色，但是不影响

打开fsfsfs.eu.org/ray网站，如果返回bad request就说明10000端口连接到xray了
如果出现502，则说明10000端口的xray连接不成功



每次修改nginx配置文件后必须使用 **systemctl reload nginx** 命令重新加载配置文件


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
ExecStartPost=/bin/sleep 0.1
ExecStartPre=/etc/nginx/sbin/nginx -t  
ExecStart=/etc/nginx/sbin/nginx -c /etc/nginx/conf/nginx.conf  
ExecReload=/etc/nginx/sbin/nginx -s reload  
ExecStop=/bin/kill -s QUIT \$MAINPID  
PrivateTmp=true  

[Install]  
WantedBy=multi-user.target
```

如果出现PID文件找不到


ps -ef|grep nginx
 cd /etc/nginx/sbin
 ps -ef|grep nginx
 
 






## 加载并启动nginx

#### 

首先这个是启动nginx

```
systemctl daemon-reload
```

#### 

然后重启一下nginx

```
systemctl restart nginx
```


如果提示
Job for nginx.service failed because the control process exited with error code.
See "systemctl status nginx.service" and "journalctl -xe" for details.






#### 

随时查看一下nginx状态

```
systemctl status nginx
```

-   然后按住ctril +c可以退出状态查看页
    
-   其他命令
    
-   启动
    

```
systemctl start nginx
```

-   停止

```
systemctl stop nginx
```

### 重新加载nginx配置文件
```
systemctl reload nginx
```

配置好之后就可以通过刚才配置好的xui路径在浏览器进入面板了

例如
fsfsfs.eu.org/548ed45a-5538-470d-9666-14784d18031b-xui

或者例如
fsfsfs.eu.org/xui

并且浏览器直接访问域名的时候就可以跳入刚才设定好的伪装站点
fsfsfs.eu.org

### 回到xui面板
回到xui面板的入站列表》找到这一条添加的》查看

然后将里面的信息复制下来

协议: vless
地址: 127.0.0.1
端口: 10000
uuid: 548ed45a-5538-470d-9666-14784d18031b
传输: ws
host: 无
path: /548ed45a-5538-470d-9666-14784d18031b

节点导入进windows上的v2ray软件里

将地址改成域名
端口 改成443
传输层安全tls

windows的v2ray切换到这个节点
服务器》设为活动服务器