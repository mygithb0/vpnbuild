# 系统没了127.0.0.1的本地回环地址所以caddy启动报错的问题


using adjacent Caddyfile WARN    caddyfile       Unnecessary header_up X-Forwarded-Host: the reverse proxy's default behavior is to pass headers to the upstream
翻译：
使用相邻Caddyfile文件 警告 caddyfile不必要的header up X-Forwarded-Host：反向代理的默认行为是将标头传递到上游
解决方法：
在Caddyfile文件的内容里面删除这两行
header_up  Host  {upstream_hostport}
header_up  X-Forwarded-Host  {host}



WARN    Caddyfile input is not formatted; run the 'caddy fmt' command to fix inconsistencies    {"adapter": "caddyfile", "file": "Caddyfile", "line": 2}
翻译
警告Caddyfile输入未格式化；运行“caddy fmt”命令以修复不一致性｛“adapter”：“caddyfile”，“file”：“caddyfile”、“line”：2｝

#### Error: loading initial config: loading new config: starting caddy administration endpoint: listen tcp 127.0.0.1:2019: bind: cannot assign requested address
#### 翻译
#### 错误：加载初始配置：加载新配置：启动caddy管理端点：侦听tcp 127.0.0.1:2019:绑定：无法分配请求的地址

解决方法
ip链路设置成下
```
ip link set lo down
```
ip链路设置成上
```
ip link set lo up
```


Unit caddy.service is masked.


