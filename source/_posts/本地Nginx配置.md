---
title: 本地Nginx配置
date: 2017-11-22 18:30:42
tags:
---

例：现在我想将127.0.0.1:8081代理到songtao.adbaitai.com

首先：

进入到本地的hosts文件

```
127.0.0.1  songtao.adbaitai.com
```

#### 本地nginx路径：

```nginx
/usr/local/etc/nginx/nginx.conf
```

![](http://ww1.sinaimg.cn/large/005QDhBjgy1flr1t8vg6pj313i16cdl4.jpg)

### 问题汇总：

#### 1.403 forbidden

```nginx
server  {
        listen 80;
        server_name h6.crmpre.adbaitai.com;


        location / {
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header Host $http_host;
            proxy_set_header X-Nginx-Proxy true;
            proxy_set_header Connection "";
            proxy_pass http://localhost:8084;
        }

        /*这个地方路径已经指定了，但是浏览器报了403 forbidden,说明我没有权限*/
        location ^~ /mofang/tpl
        {
            root  /Users/songtao/Desktop/laitui-web/mofang/src/app/viewer/tpl;
      		rewrite ^/mofang.tpl/(.*)$ /$1 break;
        }
    }
```

解决办法，到根目录执行一下

```javascript
chmod 777 Desktop/
```

#### 2.没有权限

当本地重启Nginx的时候报

```nginx
nginx: [alert] could not open error log file: open() "/usr/local/var/log/nginx/error.log" failed (13: Permission denied)
2017/12/14 14:32:51 [emerg] 1163#0: open() "/usr/local/var/log/nginx/access.log" failed (13: Permission denied)
```

说明你没有权限，命令前面加`sudo`即可解决

#### 3.80端口被占用

```nginx
nginx: [emerg] bind() to 0.0.0.0:80 failed (48: Address already in use)

nginx: [emerg] bind() to 0.0.0.0:80 failed (48: Address already in use)

nginx: [emerg] bind() to 0.0.0.0:80 failed (48: Address already in use)
```

首先，查找一下80端口被哪些进程占用着，通过`sudo lsof -i :80`查看

```nginx
COMMAND   PID    USER   FD   TYPE            DEVICE SIZE/OFF NODE NAME
Google    662 songtao  143u  IPv4 0xe2f6ca59a471def      0t0  TCP localhost:52028->localhost:http (CLOSE_WAIT)
Google    662 songtao  243u  IPv4 0xe2f6ca58e54473f      0t0  TCP 192.168.10.122:52114->titan.adbaitai.com:http (CLOSE_WAIT)
/*一般像这种以httpd开头的USRE为root的，直接干掉就行了*/
httpd     740    root    4u  IPv6 0xe2f6ca58960510f      0t0  TCP *:http (LISTEN)
nginx     746    root    8u  IPv4 0xe2f6ca5976fe36f      0t0  TCP *:http (LISTEN)
/*这个是我本地的Nginx服务，现在是起来的状态*/
nginx     747  nobody    8u  IPv4 0xe2f6ca5976fe36f      0t0  TCP *:http (LISTEN)
httpd     748    _www    4u  IPv6 0xe2f6ca58960510f      0t0  TCP *:http (LISTEN)
```

通过`sudo kill -9 740`[PID]

然后再执行`sudo nginx`

执行一下`ps -ef | grep nginx`查看一下Nginx的执行情况

```nginx
 0   746     1   0 10:01上午 ??         0:00.00 nginx: master process nginx
   -2   747   746   0 10:01上午 ??         0:01.08 nginx: worker process
  501  1188  1118   0  2:55下午 ttys000    0:00.00 grep nginx
```

到此Nginx启动成功

附上图片一张

![](http://ww1.sinaimg.cn/large/005QDhBjgy1fmgb9kb7rgj31ao1227gd.jpg)

#### 4.关闭apache服务器

之前是80端口被占了，然后我杀掉了占80 的端口，但是这边刚杀了，那边又立马重新启了一个，就这样，我杀一次，他启一次，最后终于找到问题所在，将apache停掉就解决了

直接截取了网友的评论，解决了问题

```nginx
谢谢热心回答的坛友了！！

@xuhainanjing

，/etc/init.d/httpd stop 我用的是Mac系统,etc下没有init.d

@webdna

killall httpd 这个试过了，杀掉了，立即就重新启动了 = =

最后是用下面这个方法才把apache停掉的

sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist
```

