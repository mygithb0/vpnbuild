- SSR、V2ray、Trojan 加上这个措施，加网页 加CDN CDN就是tls 才真正做到ip不被墙教程（高级版方法是终极版方法加一个伪装网页）
	- https://www.youtube.com/watch?v=qy982O6gXCU
		- 视频里 web的意思是你的网站 有域名 有证书TLS协议
-
-
- 这个方法可以用，成功：
- 完成Godaddy域名托管到clouldfile
	- ​cloudfare账号密码clouldflare域名解析我的域名并隐藏IP步骤​
		- Godaddy域名托管到clouldfile套路
			- 进入godaddy网站》右上角我的用户名》我的产品
			- 找到这个域名
				- qwlfejlioujiuhghub.xyz
			- 点击DNS进入》把域名服务器改成cloudfile上的托管服务器名称
-
-
- 下载XSHELL软件
	- Q:\谷歌云单独\Vultr搭建V2ray
- 安装XSHELL
- 打开XSHELL连接购买的主机VPS
	- 文件》新建》主机那里输入服务器的IP复制过来》连接
		- 用户名 在Username那里 一般是root
			-
			- 购买的主机，例如：
				- 95.179.210.21
				- root
				- U8(rwM%JT1mpG+2p
				-
			- 又例如
				- 45.32.150.152
				- root
				- S2]g3*s$+J6mxt#8
			-
			-
			-
			- 出现root@vultr:
				- 就是连接上了
		- 输入一件安装指令：
```
bash <(curl -s -L https://git.io/v2ray.sh)
```
-  这个是从 v2ray 官网最新版本的 github 上找来的

```
Y
```


 
 
   - 输入：1
	   - 安装
   - 输入传输协议：4
	   - Websocket+tls
		   - 意思是网页 和 域名证书
   - 输入端口：默认不修改
	   - （因为443端口就是配置的对应服务器上的Nginx服务 Nginx服务会用我已经申请好的TLS证书 ，让我上网的HTTP 服务变成HTTPS服务）
		   - ![image.png](../assets/image_1630141788205_0.png)
					-
			- 输入一个正确的域名：把我们解析过的域名输入进去 ，例如：
				- qwlfejlioujiuhghub.xyz
					- （解释：也就是说V2RAY WIN7版要通过域名连接上VPS服务器）
			-
			- 确认出现的IP地址和购买的VPS的IP地址一样（这个也是被墙以后要被救回的IP）
			- 正确选择确认: Y
			-
			- 是否自动配置TLS选择: Y
			-
			- 是否开启伪装网站，开启： Y
			-
			- 输入一个网站来伪装：

```
https://www.arkadium.com/
```


   -
   - 是否开启广告拦截，否： N
   -
   - 是否配置Shadowsocks： N
   -
   - 按回车继续 ： 回车
   -
   - 出现root@vultr就是安装好了
   - 输入
	   - v2ray url
		   - 获得v2ray链接
   - 把vmess链接复制下来就可以用了
	   - 粘贴再dynalist》vpn里面
   - 最后打开cloudfile上的云朵，使他变成橘红色
	   - cloudfare账号密码clouldflare域名解析我的域名并隐藏IP步骤​
   - ping一下 看看是不是cloudfile的美国节点了
	   - http://ping.chinaz.com/
   -
   - 修改伪装网址
	   - https://www.youtube.com/watch?v=FUUMxcOMPp0
	   - 输入
		   - v2ray config
	   - 6修改分流路径
	   - 新建一个路径
		   - 例如 amvst
			   - 就会出现/amvst这个路径了
		   - 回车确认
	   - 再输入
		   - v2ray config
	   - 随便输入一个国外冷门网址
		   - 例如
		   - https://www.amvst.com/
	   - 重新输入
		   - v2ray url
			   - 获得v2ray链接
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
- winscp 下载
	-
	- https://sourceforge.net/projects/winscp/
-
- 用winscp连接服务器
-
- 找到目录
- root》user》share》nginx》html
- 把本地的静态网站复制进去
-
-
-
-
-
-
-
-
-
-
-
-
-