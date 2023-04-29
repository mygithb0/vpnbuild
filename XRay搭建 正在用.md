-
- 教程
	- https://www.youtube.com/watch?v=d3WixFDz5BA
	- https://www.youtube.com/watch?v=XmVBAr31-nQ
- 教程的博客
	- https://ybfl.xyz/98.html
-
- 整体5个步骤
	- 1.解析域名，A-acme证书申请
	- 2.伪装网站搭建，安装nginx上传伪装网站文件，修改nginx.conf
	- 3.安装xray，修改xrayconfig
	- 4、启动xray，设为开启启动，重启xray和nginx
	- 5、设置证书自动签
- 一、解析域名，A-acme证书申请

如果是 centos 就手动申请证书
[手动安装证书 证书上传服务器套路](手动安装证书%20证书上传服务器套路.md)
[证书申请失败解决方法 手动申请证书 套路](证书申请失败解决方法%20手动申请证书%20套路.md)


# 设置好VPS主机
[正在用FriendHosting密码和官网](正在用FriendHosting密码和官网.md)
[gcorelabs云主机 日本使用套路](gcorelabs云主机%20日本使用套路.md)

# 用ssh连接到主机

[端口基础知识](端口基础知识.md)

## [安装acme.sh依赖的脚本的套路](安装acme.sh依赖的脚本的套路.md)




### A-acme证书申请:（证书安装必须在nginx之前，不然nginx会占用80端口，懂得关闭nginx的可以无视）
# [acme.sh安装证书套路](acme.sh安装证书套路.md)

更改文件夹名字将文件夹名字，从，域名_ecc，改为, 域名
```
#mv 旧文件夹名 新文件夹名
mv /链接/旧文件夹名 /链接/新文件夹名

```

例如
``
#mv 旧文件夹名 新文件夹名
```
mv /root/.acme.sh/vpvp10001rb.xyz_ecc /root/.acme.sh/vpvp10001rb.xyz
```
#rename 旧文件夹名 新文件夹名

```

#rename 旧文件夹名 新文件夹名
rename /root/.acme.sh/vpvp10001rb.xyz_ecc /root/.acme.sh/vpvp10001rb.xyz

```





```
cd /root/.acme.sh
```
列一下文件有哪些

```
ls
```
如果显示有这些

account.conf  acme.sh  acme.sh.env  ca  deploy  dnsapi  http.header  notify  vpvp10001rb.xyz

那么就改成功了



- 把证书的四个文件的权限打开

全选这些文件》右键》文件权限》勾选的全部打勾打开

ca.cer 
fullchain.cer 
vpvp10001 rb.xyz.cer 
vpvp10001 rb.xyz.conf 
vpvp10001 rb.xyz.csr 
vpvp10001rb.xyz.csr.conf 
vpvp10001rb.xyz.key


- 如果 root 文件包里的文件刷新不出来就用 cd 命令进入这个文件夹

```
cd /root/.acme.sh/域名
```
例如

```
cd /root/.acme.sh/vpvp10001rb.xyz
```
要么在
```
cd /root/.acme.sh/vpvp10001rb.xyz_ecc
```


- 列出这个文件夹下的文件
下面赋予这个文件包和里面所有的文件 777 权限
```
chmod 777 /root
```


```
chmod 777 /root/.acme.sh
```




```
chmod 777 /root/.acme.sh/域名
```

```
chmod 777 /root/.acme.sh/域名/域名.conf
```

```
chmod 777 /root/.acme.sh/域名/域名.csr
```

```
chmod 777 /root/.acme.sh/域名/域名.csr
```

```
chmod 777 /root/.acme.sh/域名/域名.csr
```

```
chmod 7777 /etc
```

```
chmod 7777 /etc/ssl
```


```
chmod 7777 /etc/ssl/private
```


- 
- 
## 复制证书并转换到另一个文件夹
```
acme.sh --installcert -d 域名 -d 域名 --key-file       /复制到的路径/域名也是文件名.key  --fullchain-file /复制到的路径/域名也是文件名.crt
```
写的是复制到的路径，只写复制到的路径，复制成什么后缀名，例如复制成crt后缀名


```
acme.sh --installcert -d 域名 -d 域名 --key-file       /etc/ssl/private/vpvp10001rb.xyz.key  --fullchain-file /etc/ssl/private/vpvp10001rb.xyz.crt
```
例如
```
acme.sh --installcert -d fsfsfs.eu.org -d fsfsfs.eu.org --key-file       /etc/ssl/private/fsfsfs.eu.org  --fullchain-file /etc/ssl/private/fsfsfs.eu.org.crt
```

如果出现
The domain 'fsfsfs.eu.org' is not a cert name. You must use the cert name to specify the cert to install


复制后将这两个复制后的证书文件的全部权限打开

选中
域名的文件名.crt
域名的文件名.key
这两个文件

例如选中
vpvp10001rb.xyz.crt
vpvp10001rb.xyz.key

右键》文件权限 》打勾勾选全部


例如
```
acme.sh --installcert -d vpvp10001rb.xyz -d vpvp10001rb.xyz --key-file       /etc/ssl/vpvp10001rb.xyz.key  --fullchain-file /etc/ssl/vpvp10001rb.xyz.crt
```
或者
```
.acme.sh/acme.sh --installcert -d 域名 --fullchainpath /etc/ssl/private/域名.crt --keypath /etc/ssl/private/域名.key
```

例如
```
.acme.sh/acme.sh --installcert -d vpvp10001rb.xyz --fullchainpath /etc/ssl/private/vpvp10001rb.xyz.crt --keypath /etc/ssl/private/vpvp10001rb.xyz.key
```





复制不了是因为文件夹权限没开 etc/ssl/private 和 etc/ssl 还有 etc 文件夹


打开复制后这个文件夹和它的母目录的所有权限

对etc 文件夹》右键》文件权限》勾选的全部打勾打开
对 etc/ssl/文件夹》右键》文件权限》勾选的全部打勾打开
对etc/ssl/private文件夹》右键》文件权限》勾选的全部打勾打开



![](Pasted%20image%2020220417220844.png)
<!--⚠️Imgur upload failed, check dev console-->



或者有的在


```
.acme.sh/acme.sh --installcert -d 你的域名_ecc --fullchainpath /etc/ssl/private/你的域名.crt --keypath /etc/ssl/private/你的域名.key --ecc
```



例如
```
.acme.sh/acme.sh --installcert -d vpvp10001rb.xyz --fullchainpath /etc/ssl/private/vpvp10001rb.xyz.crt --keypath /etc/ssl/private/vpvp10001rb.xyz.key --ecc

```

参考资料：后缀名 cer 和 crt 的不同之处，为什么申请证明书之后要把后缀名从 cer 改为 crt？
https://zhidao.baidu.com/question/210955188.html
https://www.cnblogs.com/Liangw/archive/2012/11/07/2758241.html

或者例如
```
.acme.sh/acme.sh --installcert -d vpvp10001rb.xyz_ecc --fullchainpath /etc/ssl/private/vpvp10001rb.xyz.crt --keypath /etc/ssl/private/vpvp10001rb.xyz.key --ecc

```

或者例如jennie的路径
```
acme.sh --installcert -d example.com --fullchainpath /data/example.com/fullchain.crt --keypath /data/example.com/example.com.key --ecc --force
```


####  如果出现
[Tue 24 May 2022 03:51:28 AM UTC] Installing key to: /etc/ssl/private/vpvp10001rb.xyz.key
[Tue 24 May 2022 03:51:28 AM UTC] Installing full chain to: /etc/ssl/private/vpvp10001rb.xyz.crt
##### 就说明成功了


上面这个命令的意思  fullchainpath后的第一个路径是原来证书所在的路径  keypath后第二个路径是证书复制到的路径

如果出现 没有这个证书文件啊
```
bash: .acme.sh/acme.sh: No such file or directory
```

就进这个文件包看一下
```
cd /root/.acme.sh/acme.sh
```
要么进这个文件包看一下
```
cd /root/.acme.sh
```


```
ls
```


- 这两个证书就被复制到这个文件夹了

复制后进去看一下有没有
```
cd /etc/ssl/private
```

```
ls
```

#### 如果出现
[Tue 24 May 2022 03:51:28 AM UTC] Installing key to: /etc/ssl/private/vpvp10001rb.xyz.key
[Tue 24 May 2022 03:51:28 AM UTC] Installing full chain to: /etc/ssl/private/vpvp10001rb.xyz.crt
#### 就成功了
如果出现
```
-bash: .acme.sh: command not found
```

就重新再次安装 acme 命令
```
curl https://get.acme.sh | sh
```


- ![image.png](../assets/image_1633243823188_0.png){:height 271, :width 594}
- 下一步是赋予上面这个文件夹 777 权限。（因为 cret 和 key 要有 777 的权限，没有就手动添加，不然启动 xray 会报错 23）
- 下一步是赋予两个后缀名为key的文件权限
- ![image.png](../assets/image_1633244189028_0.png){:height 289, :width 594}
- 两个文件都选上》右键》文件权限》“执行”都打上勾  “读取”从黑点都打成勾
- ![image.png](../assets/image_1633244288646_0.png)
- “执行”都打上勾  “读取”从黑点都打成勾
- ![image.png](../assets/image_1633244486613_0.png)
- 

## 下一步是设置好acme自动更新

```
acme.sh --upgrade --auto-upgrade

```



#### 如果出现
[Tue 24 May 2022 04:01:44 AM UTC] Already uptodate!
[Tue 24 May 2022 04:01:44 AM UTC] Upgrade success!
#### 就成功了



如果说明命令不存在bash: acme. sh: command not found
那么就用这个命令修复
```
source ~/.bashrc
```
或
```
source ~/. bash_profile
```

- 设置成功后是
- ![image.png](../assets/image_1633244589349_0.png)
- 证书就安装完成了
-
-
-
-
-
# 二，搭建一个伪装网站，安装nginx上传伪装网站文件，修改nginx.conf
### 第一步安装nginx
[安装nginx套路](安装nginx套路.md)

```
apt update && apt install nginx -y

```


- 出现这个绿色Progress: [ 100%] 就代表安装成功了：



- ![image.png](../assets/image_1633245509072_0.png)
- 创建一个网站专用的文件夹

```
mkdir -p /var/www/website/html
```


- ![image.png](../assets/image_1633245616830_0.png)
### 把伪装站点的文件全部上传到 （这一步可以先跳过最后再做）
- 

```
/var/www/website/html/     
```

-   建议你自己找个静态模板，推荐国外大站的，例如 
- 
- https://templatemo.com/
- https://zfly9.blogspot.com/2013/02/20130226a.html
- 1. https://templated.co/ 2. https://templatemo.com/ 3. https://startbootstrap.com/themes/ 4. https://github.com/learning-zone/website-templates 5. https://colorlib.com/ 6. https://bootstrapmade.com/ 7. https://bootstraptaste.com/
- 下载一个网站模板的压缩包
- 回到finalshell里，

```
/var/www/website/html/
```



```
cd /var/www/website/html/
```



文件夹下，上传这个压缩包。

上传方法 1 命令上传

```
cd /var/www/website/html
```

```
rz -y
```

或者手动上传

- ![image.png](../assets/image_1633246157423_0.png){:height 361, :width 490}
- 上传完后先用cd命令进入到这个目录下面

```
cd /var/www/website/html
```


- ![image.png](../assets/image_1633246325632_0.png)
- 用压缩这个指令  注意文件名改成自己的

```
unzip -o -d /var/www/website/html 文件名.zip
```


- 刷新一下文件包这个页面 就看到了
- ![image.png](../assets/image_1633246501247_0.png)
# 修改的 nginx.conf 这个文件




- 直接在图形化文件夹界面找到这个文件

```
cd /etc/nginx/
```

或者在图形化界面打开这个文件包

如果找补到etc/nginx/这个这个文件包，就退出finalshell再进入就刷新出来了


双击编辑nginx.conf这个文件之前，要打开文件包的编辑权限

包括要打开
etc/
etc/nginx/
etc/nginx/nginx.conf
和
etc/nginx/文件包下所有文件的编辑权限


打开方式是》选中这些文件或者文件包》右键》文件权限》打勾打开全部权限

（打勾打开全部权限简称为777）

### 双击编辑nginx.conf这个文件
找到这个nginx.conf文件》右键》打开

或者用界面编辑：

```
/etc/nginx/nginx.conf
```

如果出现

```
Permission denied
```
就
```
sudo -i
```


```
chmod 777 /root
```

```
chmod 777 /etc
```

```
chmod 777 /etc/nginx
```

```
chmod 777 /etc/nginx/nginx.conf
```

###### 或者另一种方法是输入vi命令 直接修改（见下文）
```
vi /etc/nginx/nginx.conf   
```



#### 如果适直接打开的时候，打不开：
那么就就下载这个文件 到本地电脑修改
```
nginx.conf
```

删除原来这个nginx.conf文件

```
rm /etc/nginx/nginx.conf
```


# 修改nginx.conf文件里面的内容
- 
- 
- 
- 将下面这段，添加在 http{} 内（注意要将‘你的域名’改为‘您的域名’）

```
server {
			  listen 80;
			  server_name 你的域名;
			  root /var/www/website/html;
			  return 301 https://$http_host$request_uri;
		 }
	   server {
	    listen 127.0.0.1:8080;
	    root /var/www/website/html;
	    index index.html;
	    add_header Strict-Transport-Security "max-age=63072000" always;
		}
		
```

例如
```
server {
			  listen 80;
			  server_name www.fsfsfs.eu.org;
			  root /var/www/website/html;
			  return 301 https://$http_host$request_uri;
		 }
	   server {
	    listen 127.0.0.1:8080;
	    root /var/www/website/html;
	    index index.html;
	    add_header Strict-Transport-Security "max-age=63072000" always;
		}
```

修改后完整的代码是这样的

```
user www-data;  
worker_processes auto;  
pid /run/nginx.pid;  
include /etc/nginx/modules-enabled/*.conf;  
  
events {  
    worker_connections 768;  
    # multi_accept on;  
}  
  
http {  









server {
			  listen 80;
			  server_name 你的域名;
			  root /var/www/website/html;
			  return 301 https://$http_host$request_uri;
		 }
	   server {
	    listen 127.0.0.1:8080;
	    root /var/www/website/html;
	    index index.html;
	    add_header Strict-Transport-Security "max-age=63072000" always;
		}
		







    ##  
    # Basic Settings  
    ##  
  
    sendfile on;  
    tcp_nopush on;  
    types_hash_max_size 2048;  
    # server_tokens off;  
  
    # server_names_hash_bucket_size 64;  
    # server_name_in_redirect off;  
  
    include /etc/nginx/mime.types;  
    default_type application/octet-stream;  
  
    ##  
    # SSL Settings  
    ##  
  
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # Dropping SSLv3, ref: POODLE  
    ssl_prefer_server_ciphers on;  
  
    ##  
    # Logging Settings  
    ##  
  
    access_log /var/log/nginx/access.log;  
    error_log /var/log/nginx/error.log;  
  
    ##  
    # Gzip Settings  
    ##  
  
    gzip on;  
  
    # gzip_vary on;  
    # gzip_proxied any;  
    # gzip_comp_level 6;  
    # gzip_buffers 16 8k;  
    # gzip_http_version 1.1;  
    # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;  
  
    ##  
    # Virtual Host Configs  
    ##  
  
    include /etc/nginx/conf.d/*.conf;  
    include /etc/nginx/sites-enabled/*;  
}  
  
  
#mail {  
#    # See sample authentication script at:  
#    # http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript  
#  
#    # auth_http localhost/auth.php;  
#    # pop3_capabilities "TOP" "USER";  
#    # imap_capabilities "IMAP4rev1" "UIDPLUS";  
#  
#    server {  
#        listen     localhost:110;  
#        protocol   pop3;  
#        proxy      on;  
#    }  
#  
#    server {  
#        listen     localhost:143;  
#        protocol   imap;  
#        proxy      on;  
#    }  
#}
```

修改完成后，文件》保存
它就会自动上传了

如果是下载下来修改的

就是改完以后上传上去

如果上传不了就用rz命令上传

参考：上传文件到 Linux 服务器上命令安装和使用方法
https://www.cnblogs.com/nbf-156cwl/p/8641165.html

先输入 rz 看上传命令是否安装了

```
rz
```


```
apt install lrzsz
```

如果是 centos 则是用这个命令安装
```
yum   -y  install  lrzsz
```

进入这个文件包

```
 cd /etc/nginx
```
上传命令
```
rz -y
```



如果还不行就试试
[Ubuntu 中 root 用户创建和登录方法](Ubuntu%20中%20root%20用户创建和登录方法.md)



###### 或者另一种方法是输入vi命令 直接修改
```
vi /etc/nginx/nginx.conf   
```



-  光标移动到这里》键盘按i 进入可编辑模式》回车
- ![image.png](../assets/image_1633246930435_0.png)
- ![image.png](../assets/image_1633246942579_0.png){:height 862, :width 591}
- 把代码粘贴进去（注意域名要改成我的域名）
- ![image.png](../assets/image_1633247499990_0.png)
- 下一步是退出可编辑模式
- 先按Esc
- 然后输入                  

```
：wq！
```

- ![image.png](../assets/image_1633247614013_0.png)
-
# 三，安装xray，修改xrayconfig文件




## 第一步安装xray

```
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install
```


- ![image.png](../assets/image_1633248181101_0.png)
## 修改xray配置文件



#### 先生成一个UUID或者去网站随机生成一个
```
cat /proc/sys/kernel/random/uuid
```

记下我的uuid例如

参见[把UUID记下来，把域名和IP地址记下来，我生成的域名和 UUID](把UUID记下来，把域名和IP地址记下来，我生成的域名和%20UUID.md)
```
71819d5b-6645-48bf-8ed6-0a9d54f60267

```

- 或者去网站随机生成一个 https://www.uuidgenerator.net/
- 抄下这个UUID
- 下一步是编辑xray的核心文件
- 找到


```
cd /usr/local/etc/xray/
```


```
/usr/local/etc/xray/config.json
```

###### 直接双击打开修改方法1 ：
- 右键》打开
- ![image.png](../assets/image_1633248562635_0.png)
- 修改下面有中文标注的地方



- 以下所有是一个框里的一整段代码
- ![image.png](../assets/image_1633245274768_0.png)
```
{
    "log": {
        "loglevel": "debug",
        "access": "/var/log/xray/access.log",
        "error": "/var/log/xray/error.log"
    },

    "inbounds": [
        {
            "port": 443,
            "protocol": "vless",
            "settings": {
                "clients": [
                    {
                        "id": "自己去生成UUID", 
                        "flow": "xtls-rprx-direct",
                        "level": 0,
                        "email": "333@ffff.com"
                    }
                ],
                "decryption": "none",
                "fallbacks": [
                    {
                        "dest": 8080
                    }
                ]
            },
            "streamSettings": {
                "network": "tcp",
                "security": "xtls",
                "xtlsSettings": {
                    "alpn": [
                        "http/1.1"
                    ],
                    "certificates": [
                        {
                            "certificateFile": "/etc/ssl/private/你的域名.crt",
                            "keyFile": "/etc/ssl/private/你的域名.key"
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

- 如果打不开就间接修改方法 2
下载这个文件
```
#下载这个文件
/usr/local/etc/xray/config.json
```

```
{
    "log": {
        "loglevel": "debug",
        "access": "/var/log/xray/access.log",
        "error": "/var/log/xray/error.log"
    },

    "inbounds": [
        {
            "port": 443,
            "protocol": "vless",
            "settings": {
                "clients": [
                    {
                        "id": "自己去生成UUID", 
                        "flow": "xtls-rprx-direct",
                        "level": 0,
                        "email": "333@ffff.com"
                    }
                ],
                "decryption": "none",
                "fallbacks": [
                    {
                        "dest": 8080
                    }
                ]
            },
            "streamSettings": {
                "network": "tcp",
                "security": "xtls",
                "xtlsSettings": {
                    "alpn": [
                        "http/1.1"
                    ],
                    "certificates": [
                        {
                            "certificateFile": "/etc/ssl/private/你的域名.crt",
                            "keyFile": "/etc/ssl/private/你的域名.key"
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

删除云端的 config. json 文件
```
cd /usr/local/etc/xray
```

```
rm /usr/local/etc/xray/config.json
```

改好的文件上传
```
cd /usr/local/etc/xray
```


```
rz -y
```

刷新一下
![](https://i.imgur.com/a1TMjRZ.png)





 - ![image.png](../assets/image_1633248853380_0.png)
- 下一步是将上面两个文件及其文件包的权限全打开
- 选择这两个文件》右键》权限》全开
- 选择这个 private 文件包》右键》权限》全开
- 选择这个 ssl 文件包》右键》权限》全开
- 下一步是启动 xray 并设置开机自启

退出finalshell再连接进来

# 启动xray和nginx
- 第一步启动 xray


```
systemctl start xray
```

- 查看xray开启状态

```
systemctl status xray
```

如果出现绿色的提示active (running)就代表成功了
- ![image.png](../assets/image_1633249028244_0.png)
- 退出finalshell再连接进来
- 
- 其他暂时步用的命令
- 重启xray


```
systemctl restart xray
```

查看nginx状态


```
systemctl status nginx
```
如果出现绿色的提示active (running)就代表成功了
- 重启nginx

```
systemctl reload nginx
```

- 查看nginx状态


```
systemctl status nginx
```
如果出现绿色的提示active (running)就代表成功了

- 再次查看xray开启状态

```
systemctl status xray
```
如果出现绿色的提示active (running)就代表成功了

退出 finalshell 再连接进来


- 重启自动开启xray


```
systemctl enable xray
```


- 重启自动开启nginx
```
systemctl enable nginx
```



- 四，设置证书自动签
- 找到这个文件

```
/etc/ssl/private/xray-cert-renew.sh
```


- 如果找不到就退出下finallshell重新进入
- 右键打开
-
- 或者如果还找不到就新建 xray-cert-renew. sh
```
xray-cert-renew. sh
```

下载到本地
```
cd /etc/ssl/private
```

```
sz /etc/ssl/private/xray-cert-renew.sh
```

或者
```
sz /etc/ssl/private/.xray-cert-renew.sh.swp
```

删除云端
```
rm /etc/ssl/private/.xray-cert-renew.sh�.swp
```

```
rm /etc/ssl/private/.xray-cert-renew.sh�.swp
```

```
rm /etc/ssl/private/.xray-cert-renew.sh�.swp
```


上传
```
cd /etc/ssl/private
```

```
rz -y
```




或者





```
vi /etc/ssl/private/xray-cert-renew.sh
```



- 里面填入以下内容，注意修改中文注释的地方
- 以下所有是一个框里的一整段代码
- ![image.png](../assets/image_1633245171382_0.png)


```
#!/bin/bash

 
.acme.sh/acme.sh --install-cert -d a-你的域名 --ecc --fullchain-file  /etc/ssl/private/你的域名.crt --key-file  /etc/ssl/private/你的域名.key
echo "Xray Certificates Renewed"
				  
chmod +r /etc/ssl/private/你的域名.key
echo "Read Permission Granted for Private Key"

sudo systemctl restart xray
echo "Xray Restarted"



```


- 下一步是退出编辑模式

按Esc然后输入           
```
：wq！
```




- ![image.png](../assets/image_1633250000882_0.png){:height 350, :width 616}
				-
				-
- 下一步是保存后加权限

```
chmod +x /etc/ssl/private/xray-cert-renew.sh
```


- ![image.png](../assets/image_1633250159631_0.png)
- 下一步是设置自动任务

```
crontab -e
```

- 选择1

```
1
```

- ![image.png](../assets/image_1633250262797_0.png)
- 就进入这个界面了
上面出现
```
16 0 * * * "/root/.acme.sh"/acme.sh --cron --home "/root/.acme.sh" > /dev/null

```

下面出现
```
                                [ Read 1 line ]
^G Get Help      ^O Write Out     ^W Where Is      ^K Cut Text        
^J Justify       ^C Cur Pos       M-U Undo         M-A Mark Text




^X Exit          ^R Read File     ^\ Replace       ^U Paste Text    
^T To Spell      ^_ Go To Line    M-E Redo         M-6 Copy Text

```



- ![image.png](../assets/image_1633250313496_0.png)


先把上面这串文字删了
```
#先把上面这串文字删了
16 0 * * * "/root/.acme.sh"/acme.sh --cron --home "/root/.acme.sh" > /dev/null

```


下一步是在里面输入以下代码，意思为每月1日自动申请证书
- 复制下面这个 粘贴到上面那张图的那个界面里  ，   意思为每月1日自动申请证书

```
0 1 1 * *   bash /etc/ssl/private/xray-cert-renew.sh
```


```
下一步是按Ctril X 退出》选择Y
```

```
Y
```

出现这个界面以后再回车一次
```
File Name to Write: /tmp/crontab.w7TKrG/crontab                                                                                          
^G Get Help                       M-D DOS Format                  
M-A Append                        M-B Backup File
^C Cancel                         M-M Mac Format                   
M-P Prepend                       ^T To Files

```



就是出现这个界面以后再回车一次
- ![image.png](../assets/image_1633250574789_0.png)
- ![image.png](../assets/image_1633250595517_0.png)
- 最后就是查看本地能否正常连接了 顺便检验前面的Xray是否能正常运行
- [windows10 win10本地V2ray客户端连接Xray教程](windows10%20win10本地V2ray客户端连接Xray教程.md)
-