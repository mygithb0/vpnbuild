https://www.youtube.com/watch?v=s90feRmdr9A&t=427s

https://bulianglin.com/archives/nicename.html

## 节点搭建

# 更新软件源

```
apt update
```
### 启用 BBR TCP 拥塞控制算法

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
admin
admin
设置用户访问面板端口
54321



# 安装nginx

```
apt install nginx
```

# 安装acme：

```
curl https://get.acme.sh | sh
```

会出现三行红字 不用管它


### 添加软链接：

```
ln -s  /root/.acme.sh/acme.sh /usr/local/bin/acme.sh
```

### 切换CA机构： 

```
acme.sh --set-default-ca --server letsencrypt
```

### 申请证书：

```
acme.sh  --issue -d 你的域名 -k ec-256 --webroot  /var/www/html
```
例如
```
acme.sh  --issue -d fsfsfs.eu.org -k ec-256 --webroot  /var/www/html
```
出现四行绿字说明成功

### 安装证书：

```
acme.sh --install-cert -d 你的域名 --ecc --key-file       /etc/x-ui/server.key  --fullchain-file /etc/x-ui/server.crt --reloadcmd     "systemctl force-reload nginx"
```
例如
```
acme.sh --install-cert -d fsfsfs.eu.org --ecc --key-file       /etc/x-ui/server.key  --fullchain-file /etc/x-ui/server.crt --reloadcmd     "systemctl force-reload nginx"
```
出现绿字Reload success说明成功



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
如何打开x-ui面板
浏览器输入
```
我的域名:54321
```
例如
```
fsfsfs.eu.org:54321
```

注意54321是之前设的x-ui面板端口
输入之前设置的账号密码进行登录
admin
admin
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

    include /etc/nginx/mime.types;
    default_type application/octet-stream;
    gzip on;

    server {
        listen 443 ssl;
        
        server_name nicename.co;  #你的域名
        ssl_certificate       /etc/x-ui/server.crt;  #证书位置
        ssl_certificate_key   /etc/x-ui/server.key; #私钥位置
        
        ssl_session_timeout 1d;
        ssl_session_cache shared:MozSSL:10m;
        ssl_session_tickets off;
        ssl_protocols    TLSv1.2 TLSv1.3;
        ssl_prefer_server_ciphers off;

        location / {
            proxy_pass https://bing.com; #伪装网址
            proxy_redirect off;
            proxy_ssl_server_name on;
            sub_filter_once off;
            sub_filter "bing.com" $server_name;
            proxy_set_header Host "bing.com";
            proxy_set_header Referer $http_referer;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header User-Agent $http_user_agent;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto https;
            proxy_set_header Accept-Encoding "";
            proxy_set_header Accept-Language "zh-CN";
        }


        location /ray {   #分流路径
            proxy_redirect off;
            proxy_pass http://127.0.0.1:10000; #Xray端口
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
        
        location /xui {   #xui路径
            proxy_redirect off;
            proxy_pass http://127.0.0.1:9999;  #xui监听端口
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

    include /etc/nginx/mime.types;
    default_type application/octet-stream;
    gzip on;

    server {
        listen 443 ssl;
        
        server_name fsfsfs.eu.org;  #你的域名
        ssl_certificate       /etc/x-ui/server.crt;  #证书位置
        ssl_certificate_key   /etc/x-ui/server.key; #私钥位置
        
        ssl_session_timeout 1d;
        ssl_session_cache shared:MozSSL:10m;
        ssl_session_tickets off;
        ssl_protocols    TLSv1.2 TLSv1.3;
        ssl_prefer_server_ciphers off;

        location /548ed45a-5538-470d-9666-14784d18031b {
            proxy_pass http://bing.com/; #伪装网址
            proxy_redirect off;
            proxy_ssl_server_name on;
            sub_filter_once off;
            sub_filter "bing.com" $server_name;
            proxy_set_header Host "bing.com";
            proxy_set_header Referer $http_referer;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header User-Agent $http_user_agent;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto https;
            proxy_set_header Accept-Encoding "";
            proxy_set_header Accept-Language "zh-CN";
        }


        location /ray {   #分流路径
            proxy_redirect off;
            proxy_pass http://127.0.0.1:10000; #Xray端口
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
        
        location /548ed45a-5538-470d-9666-14784d18031b-xui {   #xui路径
            proxy_redirect off;
            proxy_pass http://127.0.0.1:54321;  #xui监听端口
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






每次修改nginx配置文件后必须使用 **systemctl reload nginx** 命令重新加载配置文件

### 重新加载nginx配置文件
```
systemctl reload nginx
```

配置好之后就可以通过刚才配置好的xui路径在浏览器进入面板了

例如
fsfsfs.eu.org/548ed45a-5538-470d-9666-14784d18031b-xui

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