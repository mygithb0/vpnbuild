也就是说在输入go version进行测试的时候发现报错not found

解决方法如下：

1.需要去cd /usr/local 查看有没有go 目录，如果有的话就是go语言安装成功了,如果没有请重新go语言的安装

2.再去cd /usr/local/bin 查看bin里面有没有go 文件，如果没有的话，换一个go的版本安装


然后

需要建立同步连接代码如下：
```
sudo ln -fs /usr/local/go/bin/go /usr/local/bin/go
```

3.这时候再输入
```
go version
```

看看有没有，如果有的话就会显示版本信息。
