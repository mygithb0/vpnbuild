

教学视频
https://www.youtube.com/watch?v=SZlpGd7MCTc




切换到root权限
```
sudo -i
```

进入到root根目录文件夹
```
cd /root
```





在下载Go存档文件之前，请浏览器访问GO的官方网站
https://golang.google.cn/dl/
找到最新的GO版本文件例如
go1.20.2.src.tar.gz
找到最新版的GO的下载链接
https://golang.google.cn/dl/go1.20.2.src.tar.gz



按照以下步骤在Debian 10上安装Go
首先[使用](https://www.myfreax.com/wget-command-examples/)`[wget](https://www.myfreax.com/wget-command-examples/)`[命令下载](https://www.myfreax.com/wget-command-examples/)Go压缩包：


```
wget https://golang.google.cn/dl/go1.20.2.linux-amd64.tar.gz
```


## 将GO所在的目录添加到环境变量文件里
参考阅读[什么是环境变量,PATH环境变量设置教程](什么是环境变量,PATH环境变量设置教程.md)

手动打开文件/root/ .profile

添加这一行到最后
```
export PATH=$PATH:/usr/local/src/go/bin
```
或者用方法2
[添加到永久的环境变量里的方法](添加到永久的环境变量里的方法.md)

保存退出

或者用方法2,[linux的文本编辑器编辑和保存方法](linux的文本编辑器编辑和保存方法.md)




# 运行环境变量使它生效

```
echo $PATH
```

如果出现
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/src/go/bin
就第一次成功了

运行生效
```
source ~/.profile
```

然后
```
echo $PATH
```

echo $PATH
如果出现比上面哪个多一行的提示
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/src/go/bin:/usr/local/src/go/bin
就第二次成功了

[使用tar命令解压缩tar存档](https://www.myfreax.com/create-and-unzip-files-using-tar-in-linux/)，并将文件提取到`/usr/local`目录：

```
sudo tar -C /usr/local/ -xzf go1.20.2.linux-amd64.tar.gz
```


通过键入以下命令将新的`PATH`环境变量应用于当前的shell会话：

```bash
source ~/.profile
```

要验证是否已成功安装Go，请运行以下命令，该命令将打印Go版本：


```
go version
```

如果出现
go version go版本 linux/amd64的提示就说明成功了

如果出现
 go version go version go: command not found
参考 [go语言找不到的问题](go语言找不到的问题.md)
