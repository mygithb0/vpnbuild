https://www.youtube.com/watch?v=Azj8-1rdF-o&list=PL5TbbtexT8T3d_7UX2aSFhoMYk-cl4kf4&index=8


Gcore的CDN使用套路，拯救被封的节点

https://www.youtube.com/watch?v=Mme5yaLQE7Y
Gcore官网：https://gcore.com

mi

tls in tls的流量特征导致被封
所以 windows电脑到cdn套一个tls，从cdn服务器到谷歌不用套tls

原理

windows电脑系统代理127.0.0.1：10808
出站类型vless端口以443为目标我的域名网站为目标出站

出xray的windows客户端里xray软件vless加密加上tls再加密出站

过防火墙

过CDN

接站443进服务器上的xray软件接站并解密回落到其他端口

x-ui面板搭建
x-ui面板搭建教程

https://www.youtube.com/watch?v=n5koU-pj094
https://github.com/KEJIXIAOLU/dajian


安装&升级x-ui面板一键脚本
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
mi里

设置用户访问面板端口
mi里


安装BBR2加速
出于安全考虑，安装/更新完成后需要强制修改端口与账户密码
选是 y
请设置您的账户名默认是:admin
请设置您的账户密码默认是:admin

请设置面板访问端口默认是:54321





启动x-ui

systemctl start x-ui

关闭防火墙

ufw disable
 
浏览器访问
域名:XUI的端口


把xray的内核切换到最新版本
xray状态>切换版本

入站列表>创建节点>
备注随便写 suibianxie
协议选vless
监听ip默认留空也就是监听0.0.0.0
端口设置为80就是以80接站
传输协议ws
流量探测snffing服务端没必要启用

查看 复制下链接 导入到v2ray里面

裸直接连一次试试

ping一次试试

打开youtube试试

将从windows电脑的出站改成域名
也就是原来是 v2rayN直接连服务器IP
现在是v2rayN连域名再连 


进入cloudflare网站
然后cloudflare上套cdn
前一段加tls，后一段不要

dns》小云朵打开
ssl》打开第一段
网络》打开ws可以通过

