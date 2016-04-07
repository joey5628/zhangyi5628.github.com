---
title: Mac OS X Yosemite 10.10下配置 Apache
date: 2016-04-03 18:26:18
categories: Mac
tags: [Mac, Apache]
---
花了两个晚上休息时间才把Apache给配置好。虽然Mac系统已经集成了Apache+PHP环境，但是不同的系统版本配置还不一样。之前网上找到的配置教程大多是针对老版本的Mac OS X系统。花了很多时间按照网上搜索到的多个资料进行配置却一直不成功，这对我们不是这种前端开发人员来说也是挺头疼的，自己毕竟也是第一次配置Apache。
### 一、启动Apache
在终端中输入命令，启动Apache
```bash
$ sudo apachectl start
```
关闭Apache
```bash
$ sudo apachectl stop
```
重启 Apache
```bash
$ sudo apachectl restart
```
启动Apache之后，在浏览器中输入http://localhost/  ，看到“It works!”就代表运行正常。
### 二、开启支持用户级目录http://localhost/~username/
注：以下以 username为用户名，需要安实际情况修改。

OS X中默认有两个目录可以直接运行你的web应用，一个是系统级的Web根目录，一个是用户级的根目录。

系统级根目录地址：/Library/WebServer/Documents/   
地址：http://localhost
 
用户级根目录和对应的网址：
```
/Users/username/Sites
http://localhost/~username/
```
#### 1、～/Sites 也就是自己用户目录下的站点，需要自己手动建立。
在终端输入命令：
```bash
$ sudo mkdir ~/Sites
```
 
#### 2、在/etc/apache2/users目录下创建一个取名为“username.conf”的文件。
```bash
$ sudo vi /etc/apache2/users/username.conf
```

#### 3、创建之后将下面的这几行内容写到上面的 conf 文件中：
```XML
<Directory "/Users/username/Sites/">
    Options Indexes MultiViews
    AllowOverride All
    Order allow,deny 
    Allow from all
</Directory>
```
#### 4、文件保存之后，给它赋予相应的权限：
```bash
$ sudo chmod 755 /etc/apache2/users/username.conf
```
然后再重启Apache，网上很多写到这里就说可以访问http://localhost/～username/   ，但是我一访问就显示：  
Not Found   
The requested URL /~username/ was not found on this server. 
于是自己很纳闷为什么会这样。后来Google到了一个国外的网站说的和我的情况一样，研究了下，终于弄好。
#### 5、接下来要去掉一些httpd.conf中的注释。
运行命令进入httpd.conf:
```bash
$ sudo vi /etc/apache2/httpd.conf
```
去掉如下注释：
```
LoadModule userdir_module libexec/apache2/mod_userdir.so
Include /private/etc/apache2/extra/httpd-userdir.conf  
```
修改如下路径:
将引号中的目录修改为自己的目录
```
DocumentRoot "/Library/WebServer/Documents"......
```
将引号中的目录修改为和上面一样的目录
```
<Directory "/Library/WebServer/Documents">
```
#### 6、运行：
```bash
$ sudo vi /etc/apache2/extra/httpd-userdir.conf  
```
开启
```
Include /private/etc/apache2/users/*.conf
```
#### 7、重启Apache
```bash
$ sudo apachectl restart
```
再试一次：
http://localhost/～username/  
OK了！
### 三、设置开机启动
```bash
launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist
```
不知道是不是因为安装了别的什么软件导致的.一般的开机启动项可以在System Preferences–Users&Groups–Login Items中添加或删除.可是在这里也没有发现Apache相关的启动项.于是谷歌到了下面一个可行的方法,打开终端,执行下面的命令.
```bash
$ sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist
```
如果哪天你想让它开机启动了,则将unload 改为 load:
```bash
$ sudo launchctl load -w /System/Library/LaunchDaemons/org.apache.httpd.plist
```
launchd是Mac OS下，用于初始化系统环境的关键进程。类似Linux下的init, rc.此方法同样也适用于禁用系统的一些服务,比如打印机,蓝牙等.

