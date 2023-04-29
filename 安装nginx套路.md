
- 安装nginx（这里开始 在jennie那个页面复制 jennie的图文教程链接 https://jeanniestudio.top/2020/08/24/%E6%89%8B%E5%8A%A8%E6%90%AD%E5%BB%BAvless+tcp+tls/）
- 1.安装依赖包
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


- 已经下载到 /user/local/src 下面了
	  - ![image.png](../assets/image_1630128978195_0.png)
	
	如果出现错误提示（不复制）
```
wget：command not found 
```

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
	
-  ![image.png](../assets/image_1630128993554_0.png)
		
#### 安装其他依赖
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



## 2.下载解压nginx源码
```
wget -nc --no-check-certificate http://nginx.org/download/nginx-1.18.0.tar.gz -P /usr/local/src
```


 - 解压 nginx 源码
```
tar -zxvf /usr/local/src/nginx-1.18.0.tar.gz -C /usr/local/src
```


  - 解压完出现一个 nginx 文件夹
		- ![image.png](../assets/image_1630129014194_0.png)
## 3.编译和安装nginx
#### 先进入cd /usr/local/src/nginx-1.18.0这个目录
注意：这个目录是安装包所在的目录，安装过程一直要在安装包所在目录里操作

```
cd /usr/local/src/nginx-1.18.0
```


	
   - ![image.png](../assets/image_1630129022297_0.png)
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
等待20分钟左右先去做别的任务

[服务器安装v2ray绕路25vlesswstlsnginx套路高级版不容易被墙](服务器安装v2ray绕路25vlesswstlsnginx套路高级版不容易被墙.md)