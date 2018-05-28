# ServerStatus中文版一键安装及管理脚本
## ServerStatus中文版：

 - ServerStatus中文版是一个酷炫高逼格的云探针、云监控、服务器云监控、多服务器探针~。
 - 在线演示：https://tz.98k.li

## 系统要求
CentOS 7 / Debian 7+ / Ubuntu 14.04 +

推荐 Debian 7 x64，这个是我一直使用的系统，我的脚本在这个系统上面出错率最低。

> 注意，既然是个 多服务器云监控程序，那么你肯定需要两个以上的服务器（其实一个也可以，客户端和服务端可以同时安装），一个服务器做服务端，脚本会自动安装Caddy并配置好HTTP服务的，然后接收各个客户端实时发来的信息并通过网站显示出来。
> 因为客户端每秒都会发送最新的信息给服务端，所以要保证客户端与服务端直接网络通常，否则网页显示会很抽风。
> 虽然客户端每秒都会发送信息到服务端，但是对流量消耗是很小的，毕竟每次发送的数据都只有几百或上千个字符。

**ServerStatus 客户端需要 Python 2.7版本以上才可以正常运行，如果不是那么请升级（查看版本： `python -V` ）。**

> 注意：CentOS6 系统默认的Python版本是2.6，版本太低，使用客户端会出问题，请升级Python或者更换系统。

## 安装步骤
执行下面的代码下载并运行脚本。
``` bash
wget -N --no-check-certificate https://raw.githubusercontent.com/Moexin/ServerStatus-CN-OneKey/master/status.sh && chmod +x status.sh
```
下载脚本后，根据需要安装客户端或者服务端：
``` mipsasm
# 显示客户端管理菜单
bash status.sh c
 
# 显示服务端管理菜单
bash status.sh s
```
运行脚本后会出现脚本操作菜单，选择并输入 `1` 就会开始安装。

一开始会提示你输入网站服务器的域名和端口，如果没有域名可以直接回车代表使用 **本机IP:8888**。
## 简单步骤
首先安装服务端，安装过程中会提示：
是否由脚本自动配置HTTP服务(服务端的在线监控网站)[Y/n]
``` vala
# 如果你不懂，那就直接回车，如果你想用其他的HTTP服务自己配置，那么请输入 n 并回车。
# 注意，当你曾经安装过 服务端，同时没有卸载Caddy(HTTP服务)，那么重新安装服务端的时候，请输入 n 并回车。
```
然后 **添加或修改 初始示例的节点配置，注意用户名每个节点配置都不能重复**，其他的参数都无所谓了。

然后安装客户端，根据提示填写 **服务端的IP** 和前面添加/修改 对应的 **节点用户名和密码**（用于和服务端验证），然后启动就好了。
## 使用说明
进入下载脚本的目录并运行脚本：
``` jboss-cli
# 客户端管理菜单
./status.sh c
# 服务端管理菜单
./status.sh s
```
然后选择你要执行的选项即可。
``` markdown
ServerStatus 一键安装管理脚本 [vx.x.x]
-- Moexin | https://98k.li --
 
————————————
1. 安装 服务端
2. 卸载 服务端
————————————
3. 启动 服务端
4. 停止 服务端
5. 重启 服务端
————————————
6. 设置 服务端配置
7. 查看 服务端信息
8. 查看 服务端日志
————————————
9. 切换为 客户端菜单
 
当前状态: 服务端 已安装 并 已启动
 
请输入数字 [1-9]:
```
## 其他操作
### 客户端
**启动：**`/etc/init.d/status-client start`

**停止：**`/etc/init.d/status-client stop`

**重启：**`/etc/init.d/status-client restart`

**查看状态：**`/etc/init.d/status-client status`
### 服务端
**启动：**`/etc/init.d/status-server start`

**停止：**`/etc/init.d/status-server stop`

**重启：**`/etc/init.d/status-server restart`

**查看状态：**`/etc/init.d/status-server status`
### Caddy（HTTP服务）
**启动：**`/etc/init.d/caddy start`

**停止：**`/etc/init.d/caddy stop`

**重启：**`/etc/init.d/caddy restart`

**查看状态：**`/etc/init.d/caddy status`

**Caddy配置文件：**`/usr/local/caddy/Caddyfile`


----------
**安装目录：**`/usr/local/ServerStatus`

**网页文件：**`/usr/local/ServerStatus/web`

**客户端拓展信息配置文件:**`/usr/local/ServerStatus/customMsg.txt`

**配置文件：**`/usr/local/ServerStatus/server/config.json`

**客户端查看日志：**`tail -f tmp/serverstatus_client.log`

**服务端查看日志：**`tail -f /tmp/serverstatus_server.log`


----------
### 服务端网页显示异常，频繁开启/关闭
这种问题说明系统中的 Python版本低于 2.7（查看版本： `python -V` ），一般常见这种问题的都是 CentOS6 ，因为这个系统默认都是 Python2.6 版本，版本太低，使用客户端会出问题，请升级Python或者更换系统。
### 提示 wget: command not found 的错误
这是你的系统精简的太干净了，wget都没有安装，所以需要安装wget。
``` vala
# CentOS系统:
yum install -y wget
 
# Debian/Ubuntu系统:
apt-get install -y wget
```
### Caddy启动失败，打开 http://ip 显示的是 It works !
一些系统会自带 apache2 ，而 apache2 会占用80端口，导致Caddy无法绑定端口，所以只要关掉就好了。
``` nginx
netstat -lntp
# 我们可以通过这个命令查看是不是被其他软件占用了 80 端口。
```
不过 apache2 会默认开机自启动，如果不需要可以关闭自启动或者卸载 apache2 。
**停止 Apache2**
``` vim
service apache2 stop
# 尝试使用上面这个关闭，如果没效果或者提示什么错误无法关闭，那就用下面这个强行关闭进程。
kill -9 $(ps -ef|grep "apache2"|grep -v "grep"|awk '{print $2}')
```
**取消开机自启动**
``` shell
# CentOS 系统 #
chkconfig --del httpd
# Debian/Ubuntu 系统 #
update-rc.d -f apache2 remove
```
**卸载 Apache2(卸载包括了取消开机启动，无需重复)**
``` shell
# CentOS 系统 #
yum remove httpd
# Debian/Ubuntu 系统 #
apt-get remove --purge apache2
```
关闭 Apache2后，就可以尝试启动 Caddy ，并试试能不能打开网页。
``` ebnf
service caddy start
```