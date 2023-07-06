https://maobuni.com/2022/04/26/debian-update-error/

在我们使用Debian 10系统中，如果我们使用更新，部分会出现如下所示的错误

root@debian:# apt-get update  
Get:1 http://security.debian.org/debian-security buster/updates InRelease [65.4 kB]  
Get:2 http://deb.debian.org/debian buster InRelease [122 kB]  
Get:3 http://deb.debian.org/debian buster-updates InRelease [51.9 kB]  
Get:4 http://nginx.org/packages/mainline/debian buster InRelease [3604 B]  
Get:5 http://nginx.org/packages/mainline/debian buster/nginx amd64 Packages [46.3 kB]  
Reading package lists… Done  
E: Repository ‘http://security.debian.org/debian-security buster/updates InRelease’ changed its ‘Suite’ value from ‘stable’ to ‘oldstable’  
N: This must be accepted explicitly before updates for this repository can be applied. See apt-secure(8) manpage for details.  
N: Repository ‘http://deb.debian.org/debian buster InRelease’ changed its ‘Version’ value from ‘10.9’ to ‘10.10’  
E: Repository ‘http://deb.debian.org/debian buster InRelease’ changed its ‘Suite’ value from ‘stable’ to ‘oldstable’


运行下面的命令成功解决

```
apt-get --allow-releaseinfo-change update
apt-get dist-upgrade
```
