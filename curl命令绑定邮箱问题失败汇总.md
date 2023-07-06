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


#### curl commandnot found

```
apt-get update -y && apt-get install curl -y
```

如果apt-get update失败就用

[DEBIAN源更新报错解决方法](DEBIAN源更新报错解决方法.md)

```
apt-get --allow-releaseinfo-change update apt-get dist-upgrade
```




再重新执行


```
apt-get update -y && apt-get install curl -y
```

