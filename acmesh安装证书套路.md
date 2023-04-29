### A-acme证书申请:（证书安装必须在nginx之前，不然nginx会占用80端口，懂得关闭nginx的可以无视）


```
apt-get install -y openssl cron socat curl unzip vim
```
如果是 centos 系统就先安装socat
```
yum -y install epel-release
```

```
yum update &&  yum -y install socat
```

## 绑定邮箱
- 下面这个申请代码已经更新，email=后面这里要输入你的一个邮箱要输入一个邮箱，否则会报错


```
curl https://get.acme.sh | sh -s email=my@example.com
```
例如
```
curl https://get.acme.sh | sh -s email=kjnljkn@hotmail.com
```
- 如果提示出现  E: Unable to fetch some archives, maybe run apt-get update or try with --fix-missing?  就说明
- https://blog.csdn.net/vslyu/article/details/82959552

 - 如果 centos 出现
 - ![](https://i.imgur.com/HqPn0X1.png)

- 就卸载重装  记得还是要关闭前面说的防火墙

```
yum install -y tcp_wrappers tcp_wrappers-devel readline-devel openssl-devel
yum install -y gcc

v=1.7.3.3
wget http://www.dest-unreach.org/socat/download/socat-${v}.tar.gz
tar zxvf socat-${v}.tar.gz
cd socat-${v}
./configure
make && make install

```



## 检查是否报错




```
source ~/.bashrc
```


## 一，为域名申请者证书

生成证书之前需要在你的网站根目录下放置一个文件，来验证你的域名所有权。必须保证这个域名是可以访问的。此时我们执行命令。（这个过程中 acme. sh 会全自动的生成验证文件，并放到网站的根目录，然后自动完成验证. 最后又自动删除验证文件）






```
.acme.sh/acme.sh --issue -d 你的域名 --standalone -k ec-256
```

例如
```
.acme.sh/acme.sh --issue -d fsfsfs.eu.org --standalone -k ec-256
```
或例如
```
.acme.sh/acme.sh --issue -d vpvpvp.top --standalone -k ec-256
```

或例如
```
.acme.sh/acme.sh --issue -d vpvpvp2.top --standalone -k ec-256
```

或例如
```
.acme.sh/acme.sh --issue -d vpvpvp3.top --standalone -k ec-256
```


需要20分钟 可以去干别的任务

- 如果出现下图这个 说明前面命令没加邮箱
- ![image.png](../assets/image_1633242713571_0.png)
-  如果出现下图这个说明
- 
-  如果出现下图这个说明没关闭防火墙
- 就是出现

```
The CA is processing your order, please just wait
```
那么 ubuntu 系统就用
```
#Ubuntu系统
开放所有端口
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT
iptables -F
Ubuntu 镜像默认设置了 Iptable 规则，关闭它
apt-get purge netfilter-persistent
reboot
```
然后
```
Y
```

![](https://i.imgur.com/s5gi63i.png)



- 如果出现绿色文字的这个提示就说明证书安装成功了


Tue 24 May 2022 02:26:37 AM UTC] Your cert is in: /root/.acme.sh/vpvp10001rb.xyz_ecc/vpvp10001rb.xyz.cer
[Tue 24 May 2022 02:26:37 AM UTC] Your cert key is in: /root/.acme.sh/vpvp10001rb.xyz_ecc/vpvp10001rb.xyz.key
[Tue 24 May 2022 02:26:37 AM UTC] The intermediate CA cert is in: /root/.acme.sh/vpvp10001rb.xyz_ecc/ca.cer
[Tue 24 May 2022 02:26:37 AM UTC] And the full chain certs is there: /root/.acme.sh/vpvp10001rb.xyz_ecc/fullchain.cer
root@qswfm10001:~# 

# 申请好以后证书就下载到 /root/.acme.sh/里了
-
- 进入 root 里的 . acme. sh 文件夹。如果. acme. sh 文件包没有显示出来，就对这个窗口右键刷新一下可以显示出. acme. sh 文件夹。证书安装在里面了

```
cd /root/.acme.sh
```
列一下文件有哪些

```
ls
```
如果显示有这些

account.conf  acme.sh  acme.sh.env  ca  deploy  dnsapi  http.header  notify  域名_ecc

那么就成功了
进入文件包
```
cd /root/.acme.sh/我的域名_ecc
```


例如
```
cd /root/.acme.sh/fsfsfs.eu.org_ecc
```

或者例如
```
cd /root/.acme.sh/vpvpvp.top_ecc
```
或者例如
```
cd /root/.acme.sh/vpvpvp2.top_ecc
```
或者例如
```
cd /root/.acme.sh/vpvpvp3.top_ecc
```

列一下文件有哪些
```
ls
```
如果有
```
域名.conf  域名.csr  域名.csr.conf  域名.key
```
那么就成功了

记得打开所有/root/.acme.sh文件夹下的所有文件和文件包的777权限


[复制转换证书套路](复制转换证书套路.md)