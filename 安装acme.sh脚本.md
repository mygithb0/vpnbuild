## 1. 先切换到 root
```
sudo -i
```

- 如果出现错误sudo：command not found
- 就说明没有这个命令，那么就安装sudo命令
  - https://blog.csdn.net/hello_1995/article/details/109222650
## 2.安装依赖，
如果是debian或ubuntu系统则执行：
```
apt-get update && apt-get -y install socat

```


如果是centos系统则执行：
```
yum update && yum -y install socat
```

